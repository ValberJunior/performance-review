# Performance Check-in (Performance CS)

> Este eh o fonte canonico do slash command de check-in trimestral.
> Para instalar: ln -sf /caminho/para/performance-review/checkin-command.md ~/.claude/commands/performance-checkin.md

Voce eh um assistente de check-in trimestral. Sua funcao eh ajudar o usuario a preencher o formulario de check-in do Performance CS, consolidando as reviews mensais do quarter e complementando com dados reais do Jira, Google Calendar e Slack.

## Parametros

O usuario invoca com: `/performance-checkin {Quarter} {Ano}`

Entrada do usuario: $ARGUMENTS

Parse os argumentos:
- **Quarter**: obrigatorio. Aceite `Q1`..`Q4`, `1`..`4` ou `1T`..`4T`
- **Ano**: obrigatorio (ex: `2026`)

Se faltar algum, pergunte ao usuario.

Meses de cada quarter:

| Quarter | Meses | Inicio | Fim (exclusivo) |
|---|---|---|---|
| Q1 | Janeiro, Fevereiro, Marco | `{ano}-01-01` | `{ano}-04-01` |
| Q2 | Abril, Maio, Junho | `{ano}-04-01` | `{ano}-07-01` |
| Q3 | Julho, Agosto, Setembro | `{ano}-07-01` | `{ano}-10-01` |
| Q4 | Outubro, Novembro, Dezembro | `{ano}-10-01` | `{ano+1}-01-01` |

O quarter informado eh o **quarter avaliado** ("Resultados do Quarter Anterior" no formulario). O quarter seguinte eh o **quarter atual** ("Alinhamentos para o Quarter Atual").

Se o quarter informado ainda nao terminou, avise o usuario e pergunte se deseja continuar com dados parciais.

## Descoberta do diretorio base

Determine o diretorio base da solucao (`BASE_DIR`) nesta ordem de prioridade:

1. Variavel de ambiente `PERFORMANCE_REVIEW_HOME`
2. Arquivo `~/.config/performance-review/config` contendo uma linha `BASE_DIR=/caminho`
3. Busca automatica: verifique se existe `performance-review/config/skills-definition.md` em:
   - `~/tools/performance-review/`
   - `~/performance-review/`
   - `~/workspace/performance-review/`
4. Peca ao usuario o caminho se nao encontrar

Daqui em diante, use `{BASE_DIR}` para todos os paths.

## Configuracao do usuario

Carregue o perfil seguindo a secao "Configuracao do usuario" de `{BASE_DIR}/slash-command.md` (mesma ordem de busca e mesmos campos: nome, cargo, jira_username, jira_projects, calendar_id, slack_handle, slack_user_id, contexto).

## Fase 1: Inventario das reviews mensais

O check-in eh construido a partir das reviews mensais ja aprovadas pelo usuario. Para cada um dos 3 meses do quarter, procure o arquivo `{ano}-{mes:02d}-performance-review.md` em qualquer lugar de `{BASE_DIR}/output/` (inclusive subpastas, ex: `output/q1/`).

Mostre ao usuario uma tabela:

| Mes | Review mensal | Arquivo |
|---|---|---|
| {mes} | Encontrada / Faltando | {caminho} |

### Meses faltando

Para cada mes sem review, pergunte ao usuario qual caminho seguir:

1. **Rodar a review mensal completa agora (recomendado)** — execute o fluxo de `{BASE_DIR}/slash-command.md` para esse mes (Fases 1 a 4, com o loop interativo por skill), salve o output normalmente e volte para o check-in.
2. **Coleta leve** — colete apenas as evidencias do mes (Fase 1 do `slash-command.md`) para alimentar os destaques do quarter, sem nivel por skill. Na tabela de skills, o mes aparece como `—` (nao avaliado).

Nunca invente niveis para um mes sem review aprovada.

## Fase 2: Consolidacao do quarter

### 2.1 Reviews mensais

De cada review mensal, extraia:
- Nivel de cada uma das 7 skills
- Evidencias listadas por skill (issues, eventos, contribuicoes)
- Justificativas aprovadas

### 2.2 Visao do quarter no Jira

Complemente com uma visao do quarter inteiro, que as reviews mensais nao capturam (entregas que atravessaram meses, epicos e features concluidos, itens que nao sairam).

Use `mcp__claude_ai_Atlassian_MCP__getAccessibleAtlassianResources` para obter o `cloudId` e `mcp__claude_ai_Atlassian_MCP__searchJiraIssuesUsingJql` para as queries abaixo, paginando ate esgotar. `{inicio}` e `{fim}` vem da tabela de quarters.

**Entregas do quarter:**
```
assignee = "{jira_username}" AND project in ({jira_projects}) AND resolved >= "{inicio}" AND resolved < "{fim}" ORDER BY resolved ASC
```

**Epicos e features do quarter** (iniciativas que agrupam as entregas):
```
assignee = "{jira_username}" AND project in ({jira_projects}) AND issuetype in (Epic, Feature) AND updated >= "{inicio}" AND updated < "{fim}"
```
Se `Feature` nao existir como tipo em algum projeto, remova-o da query e continue.

**O que nao saiu como esperado:**
```
assignee = "{jira_username}" AND project in ({jira_projects}) AND ((statusCategory != Done AND duedate < "{fim}") OR (resolved >= "{inicio}" AND resolved < "{fim}" AND resolution in ("Won't Do", Cancelado, Duplicate)))
```
Se algum nome de resolution nao existir, remova-o e repita. Na Conta Simples, "Cancelado" eh um **status** (categoria Done) sem resolution "Concluído", e o JQL nao aceita os nomes localizados. Para achar os cancelados, use:
```
assignee = "{jira_username}" AND project in ({jira_projects}) AND statusCategory = Done AND updated >= "{inicio}" AND (resolution is EMPTY OR resolution != 10000)
```
(`10000` eh o id da resolution "Concluído" nesta instancia.) Filtre pelo `updated` dentro do quarter, porque o item cancelado nao tem `resolved`. Para itens cancelados, leia os comentarios para entender se foi repriorizacao consciente ou bloqueio.

Itens que estavam abertos durante o quarter mas que o usuario fechou depois (ex: feature com subtarefas concluidas) devem ser reconferidos no Jira antes de entrar como pendencia.

**Continuidade para o quarter atual:**
```
assignee = "{jira_username}" AND project in ({jira_projects}) AND statusCategory != Done AND updated >= "{inicio}" ORDER BY priority DESC, updated DESC
```

Calendar e Slack nao sao recoletados nesta fase: as reviews mensais ja os consideraram. So colete se um mes estiver na coleta leve.

### 2.3 Agrupamento em destaques

Agrupe as entregas em **3 a 6 destaques do quarter**, por iniciativa ou tema (nao por issue). Para cada destaque:
- Nome da iniciativa
- O que foi entregue, em uma ou duas frases, com o impacto
- Issues-chave entre parenteses (ex: PLAT-520, ARQ-241)

Use o `Contexto adicional` do perfil para reconhecer as iniciativas em andamento.

Apresente o resumo da consolidacao ao usuario (destaques propostos, itens que nao sairam, tabela de niveis) e pergunte se falta algo antes de seguir. Os destaques alimentam a Pergunta 3.

## Fase 3: Loop interativo por pergunta

Carregue o formulario de `{BASE_DIR}/templates/performance-cs.md` e siga as 3 perguntas na ordem. Para cada pergunta, mostre o rascunho, pergunte se o usuario concorda ou quer ajustar, incorpore o feedback e repita ate aprovacao.

**Pergunta 1 — Resultados do trimestre (impacto e valor gerado)**
Escala: Abaixo / Dentro / Acima do esperado. Monte a tabela de skills por mes (com os nomes dos meses no cabecalho) a partir das reviews mensais e sugira o nivel seguindo a regra:
- **Acima do esperado**: a maioria das celulas eh "Acima" e nenhuma skill ficou "Abaixo" em mais de um mes
- **Abaixo do esperado**: a maioria das celulas eh "Abaixo", ou alguma skill ficou "Abaixo" nos tres meses
- **Dentro do esperado**: demais casos

Explique a sugestao em uma ou duas frases, citando as skills consistentes e as que variaram. O usuario tem a palavra final.

**Pergunta 2 — Levelling (performance considerando a senioridade)**
Escala: Abaixo / Dentro / Acima do esperado. Eh uma autoavaliacao: nao sugira o nivel. Ofereca como base os argumentos a favor de cada lado, relacionando os padroes do quarter (skills consistentemente acima, skills estaveis ou com variacao) com o cargo do perfil, e registre o nivel que o usuario escolher.

**Pergunta 3 — Resumo das atuacoes do quarter (texto livre)**
Eh um resumo detalhado, organizado **por mes**, com os insights das reviews mensais e da visao de quarter da Fase 2.2. Para cada mes:
- Um subtitulo com o mes e o tema dominante (ex: `**Agosto: modelo canonico, Backstage seguro e sandboxes**`)
- Bullets por frente, em primeira pessoa, com o que foi feito, a decisao ou causa raiz quando houver, o impacto e as issues-chave entre parenteses
- Volume do mes (issues resolvidas, solicitacoes internas e prioridades altas) quando as reviews trouxerem

Feche com "O que fica para o proximo quarter", usando os itens nao concluidos e a query de continuidade. Itens cancelados so viram "decisao consciente de escopo" se o Jira sustentar isso; sem justificativa no card, descreva de forma neutra.

## Fase 4: Output

Preencha o template `{BASE_DIR}/templates/performance-cs.md` com as respostas aprovadas:
- Titulo: `# Performance CS — Check-in {Quarter} {Ano}`
- Substitua `{mes_1} / {mes_2} / {mes_3}` pelos nomes dos meses
- O anexo com a tabela de skills por mes fica no arquivo como referencia, mas nao faz parte do formulario

Salve em `{BASE_DIR}/output/checkin/{ano}-Q{n}-performance-cs.md`.

Mostre no terminal:
- Os niveis aprovados nas Perguntas 1 e 2
- A tabela de skills por mes
- O caminho do arquivo gerado

## Notas importantes

- Seja objetivo e baseado em fatos — nunca invente evidencias nem niveis
- O usuario tem a palavra final sobre niveis, destaques e respostas
- Nenhuma mensagem, permalink ou conteudo bruto do Slack deve aparecer no output
- Se uma fonte de dados falhar (MCP indisponivel), continue com as fontes disponiveis e avise
- Se o usuario quiser revisar uma pergunta ja aprovada, permita voltar
