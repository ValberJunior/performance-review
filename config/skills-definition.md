# Skills Definition — Performance Review

Definicao das 7 skills avaliadas no formulario mensal, com heuristicas de mapeamento e exemplos de justificativa por nivel.

---

## Soft Skills

### 1. Accountability

**Descricao:** Compromisso com entregas, prazos e responsabilidades assumidas. Demonstra ownership sobre os resultados do seu trabalho e do time.

**Heuristicas de mapeamento:**
- Bloqueios identificados proativamente e comunicados com proposta de mitigacao (nao so o problema)
- Issues repriorizadas ou canceladas com justificativa documentada — evidencia de decisao consciente
- Alternativas propostas e testadas quando o caminho original falhou (trail visivel em comentarios de issue/PR)
- Entregas mantidas em movimento durante mudanca de prioridade ou escopo (fluxo do quadro preservado mesmo com pivots)
- Decisoes tomadas de forma autonoma sem precisar escalar, visiveis em issues ou comentarios
- Follow-ups documentados em issues apos mudancas de cenario

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Quando [cenario mudou], nao consegui adaptar minha atuacao a tempo, o que gerou bloqueio em [entrega]. Preciso melhorar minha capacidade de identificar impactos e propor alternativas de forma proativa."
- **Dentro do esperado:** "Quando [prioridade/escopo mudou], identifiquei o impacto, propus [alternativa] e ajustei [entrega X] para manter o fluxo do quadro avancando. Tomei decisoes autonomas sobre [aspecto] sem precisar escalar, documentando o raciocinio no Jira."
- **Acima do esperado:** "Diante de [mudanca significativa de cenario], mapeei proativamente os riscos, propus e testei [alternativas], e conduzi os ajustes necessarios mantendo o time e stakeholders alinhados. A decisao de [acao] evitou [impacto negativo] e virou referencia para situacoes similares."

---

### 2. Comunicacao e Influencia

**Descricao:** Capacidade de comunicar ideias, decisoes e status de forma clara. Influencia positivamente decisoes tecnicas e de produto.

**Heuristicas de mapeamento:**
- Reunioes com stakeholders de outras areas (Calendar)
- Apresentacoes, demos, knowledge sharing
- Participacao em decisoes tecnicas (RFCs, ADRs com comentarios)
- Comunicacao de incidentes/postmortems
- Mentoria ou pair programming documentado

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Preciso melhorar a comunicacao proativa sobre o andamento das minhas entregas. Reconheco que em [situacao], a falta de alinhamento gerou retrabalho."
- **Dentro do esperado:** "Comuniquei de forma clara o progresso das entregas nas dailies e no Jira. Participei ativamente das discussoes tecnicas do time e alinhei com o time de [area] sobre [tema]."
- **Acima do esperado:** "Conduzi a apresentacao de [tema] para o time de [area], influenciando a decisao de [resultado]. Facilitei o alinhamento entre squads X e Y sobre [problema compartilhado]."

---

### 3. Visao Sistemica

**Descricao:** Capacidade de enxergar o impacto do seu trabalho no contexto mais amplo da organizacao. Considera dependencias, riscos e oportunidades cross-squad.

**Heuristicas de mapeamento:**
- Issues que impactam multiplos times/squads
- Trabalho cross-squad documentado (Jira links, epics compartilhados)
- Participacao em reunioes/decisoes de arquitetura organizacional
- Identificacao proativa de riscos ou dependencias
- Contribuicoes em projetos fora do escopo imediato do time

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Neste mes foquei principalmente nas entregas do meu squad sem considerar adequadamente o impacto em [area relacionada]."
- **Dentro do esperado:** "Identifiquei e comuniquei a dependencia entre [minha entrega] e [entrega do time X], garantindo alinhamento de prazos. Considerei o impacto em [sistema Y] ao implementar [mudanca]."
- **Acima do esperado:** "Propus e liderei a iniciativa de [tema] que beneficia squads X, Y e Z. Mapeei proativamente riscos de [problema] que afetaria toda a organizacao e coordenei a mitigacao."

---

## Hard Skills

### 4. Planejamento e Desenho de Solucoes

**Descricao:** Capacidade de planejar e desenhar solucoes tecnicas de forma estruturada, considerando trade-offs, escalabilidade e manutencao.

**Heuristicas de mapeamento:**
- RFCs escritos ou revisados
- ADRs (Architecture Decision Records)
- Design docs / technical specs
- Issues de tipo "spike" ou "research" concluidas
- Diagramas de arquitetura criados/atualizados
- Estimativas de esforco documentadas

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Neste mes nao produzi documentacao tecnica para as solucoes implementadas. Preciso melhorar o habito de documentar decisoes de design antes da implementacao."
- **Dentro do esperado:** "Escrevi o RFC para [solucao], documentando alternativas consideradas e trade-offs. Revisei o design doc de [colega] contribuindo com [sugestoes]."
- **Acima do esperado:** "Liderei o desenho da solucao de [projeto complexo], produzindo RFC + ADR que foram referencia para o time. A abordagem escolhida reduziu [metrica] em X%."

---

### 5. Implementacao e Qualidade

**Descricao:** Qualidade do codigo produzido, boas praticas de engenharia, code reviews e entrega de solucoes robustas.

**Heuristicas de mapeamento:**
- PRs abertos e mergeados
- Code reviews realizados (comentarios em PRs de outros)
- Issues de bug fix
- Melhorias de testes (cobertura, tipos de teste)
- Refactorings documentados
- Issues com label "tech-debt" resolvidas

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Tive PRs com ciclos de revisao acima da media por [motivo]. Preciso melhorar [aspecto especifico] antes de abrir PRs."
- **Dentro do esperado:** "Entreguei X PRs no mes com boa qualidade. Realizei Y code reviews contribuindo com sugestoes de [tipo]. Mantive a cobertura de testes em [nivel]."
- **Acima do esperado:** "Alem das entregas planejadas, identifiquei e resolvi tech debt em [area], reduzindo [metrica]. Implementei [padrao/ferramenta] que melhorou a qualidade de codigo do time."

---

### 6. Atuacao em Sustentacao/Manutencao

**Descricao:** Capacidade de atuar em incidentes, alertas e manutencao de sistemas em producao. Resposta rapida e eficaz a problemas.

**Heuristicas de mapeamento:**
- Issues de tipo incident/bug com prioridade alta
- Participacao em on-call / plantao
- Alertas investigados e resolvidos
- Postmortems escritos ou participados
- Hotfixes deployados
- Melhorias de observabilidade (dashboards, alertas)

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Durante minha semana de on-call, demorei para responder ao alerta de [sistema]. Estou revisando os runbooks para melhorar meu tempo de resposta."
- **Dentro do esperado:** "Atuei como on-call na semana X, resolvendo Y alertas dentro do SLA. Investiguei o incidente de [problema] e implementei o fix em [tempo]."
- **Acima do esperado:** "Alem da atuacao em incidentes, implementei [melhoria de observabilidade] que reduziu o MTTR em X%. Escrevi postmortem detalhado de [incidente] com acoes preventivas que evitaram recorrencia."

---

### 7. Desenvolvimento Eficiente

**Descricao:** Uso eficiente de ferramentas, automacoes e padroes para aumentar a produtividade propria e do time.

**ATENCAO — HARD SKILL:** A justificativa desta skill deve ser baseada em evidencias TECNICAS e de PROCESSO. Evite linguagem de soft skill (colaboracao, comunicacao, influencia). Foque em: o que foi construido, como foi construido, quais praticas de engenharia foram aplicadas.

**Heuristicas de mapeamento:**
- Melhorias em CI/CD pipelines
- Automacoes criadas (scripts, bots, ferramentas internas)
- Templates, boilerplates ou padroes estabelecidos
- Reducao de toil documentada
- Adocao ou melhoria de ferramentas de desenvolvimento
- Contribuicoes para developer experience (DX)
- IaC bem estruturado (modulos, versionamento, pipelines automaticos)
- Boas praticas de engenharia aplicadas: schema design, ingestao incremental, separacao de responsabilidades

**Exemplos de justificativa por nivel:**

- **Abaixo do esperado:** "Nao investi tempo em automacoes ou melhorias de processo neste mes. Reconheco que tarefas repetitivas de [tipo] poderiam ser automatizadas."
- **Dentro do esperado:** "Construi [sistema/automacao] provisionado via IaC (Terraform/K8s) com pipelines de CI/CD, seguindo boas praticas de engenharia como [schema design, ingestao incremental, etc.]. A solucao eliminou o processo manual de [atividade]."
- **Acima do esperado:** "Liderei a construcao de [ferramenta/padrao] que foi adotada por outros squads. Estabeleci padroes de [IaC/CI-CD/automacao] que reduziram o tempo de [metrica] e foram disseminados para o time."
