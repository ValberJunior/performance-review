# Performance Review Assistant

Slash command `/performance-review` que auxilia no preenchimento do formulário mensal de avaliação de performance. Usa dados do Jira, Google Calendar e Slack para reconstruir o que aconteceu no mês e gerar justificativas objetivas por skill.

---

## Início rápido

**Primeira vez?** Peça ao seu agente de IA para configurar tudo: veja [AGENT-ONBOARD.md](AGENT-ONBOARD.md).

**Já configurado:**

```
/performance-review Janeiro 2026
/performance-review Março 2026
/performance-checkin Q3 2026
```

---

## Como funciona

1. Coleta issues do Jira, eventos do Calendar e mensagens do Slack do mês informado (conforme configuração do perfil)
2. Mapeia evidências para as 7 skills da avaliação
3. Sugere nível e justificativa para cada skill
4. Loop interativo: você ajusta, complementa e aprova cada uma
5. Gera arquivo de output pronto para copiar na avaliação

### Check-in trimestral (Performance CS)

`/performance-checkin <quarter> <ano>` monta o formulário de check-in do Performance CS (`templates/performance-cs.md`):

1. Procura as reviews mensais do quarter em `output/` e oferece rodar as que faltarem
2. Consolida níveis por skill/mês e busca no Jira as entregas, épicos e itens não concluídos do quarter
3. Loop interativo pelas 3 perguntas do formulário: resultados do trimestre, levelling (autoavaliação, o nível é seu) e resumo detalhado das atuações por mês
4. Gera `output/checkin/{ano}-Q{n}-performance-cs.md`

---

## Skills avaliadas (7)

### Soft Skills
1. **Accountability** — Compromisso com entregas, adaptação a mudanças
2. **Comunicação e Influência** — Alinhamento com stakeholders, influência cross-squad
3. **Visão Sistêmica** — Impacto organizacional, alinhamento com prioridades do negócio

### Hard Skills
4. **Planejamento e Desenho de Soluções** — Arquitetura, RFCs, ADRs, design docs
5. **Implementação e Qualidade** — PRs, code review, boas práticas, testes
6. **Atuação em Sustentação/Manutenção** — Incidentes, alertas, observabilidade
7. **Desenvolvimento Eficiente** — CI/CD, automações, IaC, redução de toil

Definições completas e heurísticas de mapeamento: `config/skills-definition.md`

---

## Integrações

| Integração | Função | Quando ativa? |
|---|---|---|
| **Jira** | Buscar issues do mês por assignee nos projetos (quadros Kanban) do perfil | **Sempre (obrigatório)** |
| **Google Calendar** | Listar eventos não-recorrentes do mês | Se `calendar_id` estiver no perfil |
| **Slack** | Buscar mensagens do usuário no mês para enriquecer evidências | Se `slack_handle` estiver no perfil |

> Calendar e Slack são opcionais e ativados automaticamente pelo perfil — sem flags manuais.
> Dados do Slack são usados apenas como insumo de análise e nunca aparecem no output final.

---

## Estrutura do repositório

```
performance-review/
  README.md                     # Este arquivo — visão geral para humanos
  AGENT-ONBOARD.md              # Instruções de setup focadas em agentes de IA
  slash-command.md              # Fonte canônico do /performance-review (mensal)
  checkin-command.md            # Fonte canônico do /performance-checkin (trimestral)
  templates/
    performance-cs.md          # Formulário do check-in trimestral (Performance CS)
  config/
    user-profile-template.md   # Template de perfil de usuário
    skills-definition.md       # Definição das 7 skills + heurísticas
  prompts/
    data-collector.md          # Instruções de coleta (Jira, Calendar, Slack)
    skill-mapper.md            # Classificação de evidências por skill
    draft-writer.md            # Geração de justificativas
  output/                      # Outputs gerados (gitignore — dados pessoais)
```

---

## Compartilhando com o time

Cada pessoa precisa:
1. Clonar ou copiar este repositório
2. Criar seu próprio `config/<nome>-profile.md` (não versionado — `.gitignore`)
3. Instalar o slash command no seu agente
4. Ter acesso ao Jira configurado no seu agente

Para um time que usa formulário diferente (skills distintas), basta editar `config/skills-definition.md`.
