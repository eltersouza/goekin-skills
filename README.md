# goekin-skills

**Goekin's Skills Suite (GSS)**

Skills do [GoEkin](https://goekin.com) para agentes de LLM: Claude Code, Cursor, Hermes, claude.ai, ChatGPT e qualquer outro agente que fale com a API ou o MCP do GoEkin.

Com elas, um agente conduz o ciclo de desenvolvimento junto com a gestão do projeto: do pedido que chega (e-mail de cliente, conversa, erro em produção) ao planejamento, à implementação, ao QA, ao release, ao relatório para o cliente e à retrospectiva. O GoEkin é a fonte da verdade de cada etapa.

> Épico no GoEkin: [GOE-271](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-271). Issues deste repositório levam a label `goekin-skills`.

## Índice das skills

Este README é o índice do repositório. Cada skill tem uma linha, com o que ela faz, o status e a issue no GoEkin.

**Status:** `publicada` (pronta para instalar), `em desenvolvimento` (escrita e em teste), `planejada` (ainda é ideia no épico). Hoje todas as 20 skills estão em desenvolvimento, em teste.

### Base

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| [goekin-workflow](skills/goekin-workflow/SKILL.md) | Fluxo padrão que todo agente segue em qualquer repositório: quem move cada status, quando comentar, como nomear branch e PR e o que fazer com trabalho novo. As outras skills se apoiam nela. | em desenvolvimento | [GOE-343](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-343) |

### Entrada

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| [goekin-intake](skills/goekin-intake/SKILL.md) | Transforma e-mail, texto livre ou transcrição de reunião em issues bem escritas, em Backlog. | em desenvolvimento | [GOE-275](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-275) |
| [goekin-spec](skills/goekin-spec/SKILL.md) | Entrevista a pessoa até chegar a um spec com critérios de aceite. | em desenvolvimento | [GOE-276](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-276) |
| [goekin-triage-errors](skills/goekin-triage-errors/SKILL.md) | Lê erros do Sentry e de logs, agrupa e abre ou atualiza issues de bug. | em desenvolvimento | [GOE-288](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-288) |

### Planejamento

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| [goekin-plan-issue](skills/goekin-plan-issue/SKILL.md) | Transforma uma issue rasa numa issue pronta para implementar: lê a issue e o código e escreve na descrição contexto, abordagem, arquivos, critérios de aceite, testes, riscos e perguntas, preservando o pedido original. Não muda o status. | em desenvolvimento | [GOE-273](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-273) |
| [goekin-breakdown](skills/goekin-breakdown/SKILL.md) | Quebra um épico ou projeto em milestones e issues de 1 a 5 pontos, com dependências e caminho crítico. Mostra a proposta e só cria com o ok, tudo em Backlog. | em desenvolvimento | [GOE-277](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-277) |
| [goekin-estimate](skills/goekin-estimate/SKILL.md) | Sugere a estimativa de cada issue, com a justificativa. | em desenvolvimento | [GOE-278](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-278) |
| [goekin-refine-backlog](skills/goekin-refine-backlog/SKILL.md) | Revisa o backlog, completa o que falta nas issues e propõe a ordem. | em desenvolvimento | [GOE-279](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-279) |
| [goekin-backlog-hygiene](skills/goekin-backlog-hygiene/SKILL.md) | Encontra duplicatas, issues paradas e órfãs. | em desenvolvimento | [GOE-280](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-280) |
| [goekin-plan-cycle](skills/goekin-plan-cycle/SKILL.md) | Propõe o próximo ciclo pela capacidade do time: o que cabe, o que fica de fora e por quê. Só aplica (ciclo e a fazer juntos) com ordem explícita, e registra o plano na descrição do ciclo. | em desenvolvimento | [GOE-281](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-281) |

### Execução

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| [goekin-implement-issue](skills/goekin-implement-issue/SKILL.md) | Cria a branch, escreve o código, abre o PR e atualiza a issue. | em desenvolvimento | [GOE-274](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-274) |
| [goekin-handoff](skills/goekin-handoff/SKILL.md) | Passa o trabalho entre agentes, ou entre agente e humano, com o contexto registrado na issue. | em desenvolvimento | [GOE-284](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-284) |
| [goekin-review-pr](skills/goekin-review-pr/SKILL.md) | Revisa o PR contra a issue e os critérios de aceite. | em desenvolvimento | [GOE-285](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-285) |
| [goekin-qa](skills/goekin-qa/SKILL.md) | Testa a entrega e registra o veredito na issue. | em desenvolvimento | [GOE-286](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-286) |

### Entrega e acompanhamento

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| [goekin-release](skills/goekin-release/SKILL.md) | Gera o changelog e as release notes a partir das issues entregues. | em desenvolvimento | [GOE-287](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-287) |
| [goekin-monitor-cycle](skills/goekin-monitor-cycle/SKILL.md) | Acompanha burn-down, capacidade e riscos do ciclo. | em desenvolvimento | [GOE-282](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-282) |
| [goekin-standup](skills/goekin-standup/SKILL.md) | Resume o que andou, o que vem e o que está travado. | em desenvolvimento | [GOE-283](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-283) |
| [goekin-client-report](skills/goekin-client-report/SKILL.md) | Escreve o status do projeto para o cliente, para revisão humana antes do envio. | em desenvolvimento | [GOE-289](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-289) |
| [goekin-retro](skills/goekin-retro/SKILL.md) | Levanta o que funcionou e o que mudar no fim do ciclo. | em desenvolvimento | [GOE-290](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-290) |
| [goekin-docs](skills/goekin-docs/SKILL.md) | Gera e atualiza a documentação a partir do código. | em desenvolvimento | [GOE-291](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-291) |

## Princípios que valem para todas as skills

- O `goekin-workflow` vale para toda skill, sempre. Na primeira vez em um repositório, ele pergunta se deve registrar uma regra para os agentes usarem o GoEkin como gestor único ou principal do projeto.
- Issue nova sempre entra em Backlog, e as skills de entrada perguntam time, projeto, milestone e contexto antes de criar.
- Skills de execução movem a issue para em andamento ao começar.
- Skills de QA e revisão mandam o resultado para revisão (In Review), nunca para concluída.
- A fazer (Todo) só por um humano, à mão, ou por um agente com comando explícito dele.
- Done só depois do merge.
- Toda skill se anuncia: a resposta começa com `GSS · <skill>` e termina com `GSS · <skill> concluída`, para a pessoa saber qual skill está em uso e quando. Você também pode chamar uma skill pelo nome.
- Todo registro do agente é um comentário assinado na issue, com a marca `via GSS · <skill>`. A mesma marca vai na descrição dos PRs e nos commits (`Via: GSS`), e um projeto pode desligá-la no `AGENTS.md` ou no `CLAUDE.md`.
- Nada vai para o cliente sem revisão humana.
- Nunca expor chaves: o agente lê a chave da variável `GOEKIN_API_KEY` e nunca a escreve em arquivo, log ou mensagem.

## Como manter este índice

- Toda skill nova, alterada ou removida atualiza esta tabela no mesmo PR.
- Uma skill só vira `publicada` quando está no repositório e pode ser instalada.
- O formato do repositório (como empacotar, versionar, instalar e dar permissões) ainda vai ser decidido na [GOE-272](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-272). Até lá, cada skill fica numa pasta própria em `skills/<nome-da-skill>/`, com um `SKILL.md`.
