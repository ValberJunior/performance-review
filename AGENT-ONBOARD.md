# AGENT-ONBOARD

> Este arquivo contém instruções direcionadas a agentes de IA.
> Se você é um agente configurando esta solução para um usuário, siga este guia.
> Se você é um humano, veja o [README.md](README.md) e cole o prompt da seção abaixo no seu agente.

---

## Prompt de onboarding (para o usuário colar no agente)

```
Quero configurar a solução "Performance Review Assistant".
O repositório está em: [CAMINHO_DO_REPO]

Leia o AGENT-ONBOARD.md e execute o processo de setup descrito nele.
```

---

## Instruções para o agente

Você foi chamado para configurar o Performance Review Assistant para o usuário.
Execute as etapas abaixo na ordem. Seja proativo: infira o que puder antes de perguntar.

### Etapa 1 — Criar o perfil do usuário

Objetivo: gerar `config/<nome>-profile.md` com os dados do usuário.

1. Leia `config/user-profile-template.md` para entender os campos necessários
2. Tente preencher automaticamente inferindo do contexto disponível:
   - Nome e email: `git config user.name` e `git config user.email`
   - Variáveis de ambiente do sistema
   - Arquivos de configuração do projeto (`.env`, `package.json`, etc.)
   - Memória ou contexto da sessão atual
   - Qualquer outra fonte disponível no seu ambiente
3. Gere um rascunho com o que conseguiu descobrir
4. Mostre o rascunho ao usuário e pergunte **apenas** o que não conseguiu inferir
   (campos comuns que precisam ser confirmados: Jira username, projeto principal, cargo)
5. Salve o arquivo final como `config/<nome>-profile.md`

> O arquivo `*-profile.md` está no `.gitignore` — não será versionado.

### Etapa 2 — Verificar integrações

Verifique quais integrações estão disponíveis no seu ambiente atual:

**Jira (obrigatório)**
- Tente executar uma query JQL simples para confirmar acesso:
  `assignee = "<jira_username>" ORDER BY updated DESC` com `maxResults=1`
- Se falhar: explique ao usuário o que precisa ser configurado no ambiente dele
  (não assuma qual ferramenta ele usa — adapte ao que identificar)

**Google Calendar (opcional — ativado pelo perfil)**
- Só será usado se `calendar_id` estiver configurado no perfil do usuário
- Tente listar os calendários disponíveis para confirmar acesso
- Se falhar ou não configurado: informe que a integração com Calendar não está
  disponível — o comando funcionará normalmente apenas com Jira e Slack

**Slack (opcional — ativado pelo perfil)**
- Só será usado se `slack_handle` estiver configurado no perfil do usuário
- Tente executar uma busca simples para confirmar acesso:
  `from:<slack_handle> after:<data>` via `slack_search_public_and_private`
- Se falhar ou não configurado: informe que a integração com Slack não está
  disponível — o comando funcionará normalmente com as demais fontes
- Importante: dados do Slack são usados apenas como insumo de análise.
  Nenhum conteúdo bruto do Slack (mensagens, permalinks) deve aparecer no output final

### Etapa 3 — Instalar os slash commands

São dois comandos:
- `/performance-review` (mensal) — fonte em `slash-command.md`
- `/performance-checkin` (check-in trimestral do Performance CS) — fonte em `checkin-command.md`

O método de instalação varia por agente:

**Claude Code (claude CLI)**
```bash
mkdir -p ~/.claude/commands
# Symlink (recomendado — fica sincronizado com o repo):
ln -sf [CAMINHO_DO_REPO]/slash-command.md ~/.claude/commands/performance-review.md
ln -sf [CAMINHO_DO_REPO]/checkin-command.md ~/.claude/commands/performance-checkin.md
# Ou como cópia:
\cp [CAMINHO_DO_REPO]/slash-command.md ~/.claude/commands/performance-review.md
\cp [CAMINHO_DO_REPO]/checkin-command.md ~/.claude/commands/performance-checkin.md
```

**Cursor / outros agentes**
- Coloque o conteúdo de `slash-command.md` e `checkin-command.md` nas regras/instruções do agente
- Adapte conforme o mecanismo disponível no ambiente do usuário

**Configuração do diretório base (se necessário)**
Por padrão, os slash commands descobrem o repo automaticamente. Se estiver num caminho
não-padrão, configure uma das opções abaixo:

```bash
# Opção 1: variável de ambiente
export PERFORMANCE_REVIEW_HOME=/caminho/do/repo

# Opção 2: arquivo de config
mkdir -p ~/.config/performance-review
echo "BASE_DIR=/caminho/do/repo" > ~/.config/performance-review/config
```

### Etapa 4 — Testar

Quando tudo estiver configurado:
1. Sugira um mês recente para testar (preferencialmente o mês anterior)
2. Execute `/performance-review <Mês> <Ano>` ou simule o fluxo
3. Confirme com o usuário que o resultado faz sentido
4. Se o usuário já tiver as reviews mensais de um quarter fechado, mostre também `/performance-checkin <Quarter> <Ano>`

### Comportamento esperado durante o setup

- **Não peça ao usuário o que você consegue descobrir sozinho**
- **Não execute ações destrutivas sem confirmar**
- Se uma etapa falhar, explique o motivo e o que o usuário precisa fazer para resolver
- Adapte as instruções ao ambiente que você identificar — não assuma Claude Code ou qualquer ferramenta específica
