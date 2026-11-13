# goekin-skills

**Goekin's Skills Suite (GSS)**

Skills do [GoEkin](https://goekin.com) para agentes de LLM: Claude Code, Cursor, Hermes, claude.ai, ChatGPT e qualquer outro agente que fale com a API ou o MCP do GoEkin.

Com elas, um agente conduz o ciclo de desenvolvimento junto com a gestão do projeto: do pedido que chega (e-mail de cliente, conversa, erro em produção) ao planejamento, à implementação, ao QA, ao release, ao relatório para o cliente e à retrospectiva. O GoEkin é a fonte da verdade de cada etapa.

> Épico no GoEkin: [GOE-271](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-271). Issues deste repositório levam a label `goekin-skills`.

## Índice das skills

Este README é o índice do repositório. Cada skill tem uma linha, com o que ela faz, o status e a issue no GoEkin.

**Status:** `publicada` (pronta para instalar), `em desenvolvimento`, `planejada` (ainda é ideia no épico).

### Entrada

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| Intake de documentos e textos | Transforma e-mail, texto livre ou transcrição de reunião em issues bem escritas, em Backlog. | planejada | [GOE-275](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-275) |
| Especificação por conversa | Entrevista a pessoa até chegar a um spec com critérios de aceite. | planejada | [GOE-276](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-276) |
| Triagem de bugs a partir de erros | Lê erros do Sentry e de logs, agrupa e abre ou atualiza issues de bug. | planejada | [GOE-288](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-288) |

### Planejamento

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| Planejar a implementação | Lê uma issue e o código e registra na issue o plano de implementação. | planejada | [GOE-273](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-273) |
| Quebrar épico ou projeto | Divide um épico ou projeto em issues, sub-issues e milestones. | planejada | [GOE-277](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-277) |
| Estimativa de issues | Sugere a estimativa de cada issue, com a justificativa. | planejada | [GOE-278](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-278) |
| Refinamento e priorização | Revisa o backlog, completa o que falta nas issues e propõe a ordem. | planejada | [GOE-279](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-279) |
| Higiene do backlog | Encontra duplicatas, issues paradas e órfãs. | planejada | [GOE-280](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-280) |
| Planejamento do ciclo | Monta a sprint pela capacidade do time. | planejada | [GOE-281](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-281) |

### Execução

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| Implementar uma issue | Cria a branch, escreve o código, abre o PR e atualiza a issue. | planejada | [GOE-274](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-274) |
| Handoff | Passa o trabalho entre agentes, ou entre agente e humano, com o contexto registrado na issue. | planejada | [GOE-284](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-284) |
| Revisão de PR | Revisa o PR contra a issue e os critérios de aceite. | planejada | [GOE-285](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-285) |
| QA e veredito | Testa a entrega e registra o veredito na issue. | planejada | [GOE-286](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-286) |

### Entrega e acompanhamento

| Skill | O que faz | Status | Issue |
|---|---|---|---|
| Deploy e release | Gera o changelog e as release notes a partir das issues entregues. | planejada | [GOE-287](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-287) |
| Monitoramento do ciclo | Acompanha burn-down, capacidade e riscos do ciclo. | planejada | [GOE-282](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-282) |
| Standup e resumo diário | Resume o que andou, o que vem e o que está travado. | planejada | [GOE-283](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-283) |
| Relatório para o cliente | Escreve o status do projeto para o cliente, para revisão humana antes do envio. | planejada | [GOE-289](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-289) |
| Retrospectiva | Levanta o que funcionou e o que mudar no fim do ciclo. | planejada | [GOE-290](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-290) |
| Documentação a partir do código | Gera e atualiza a documentação a partir do código. | planejada | [GOE-291](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-291) |

## Princípios que valem para todas as skills

- Issue nova sempre entra em Backlog.
- Todo só com ordem humana.
- Done só depois do merge.
- Todo registro do agente é um comentário assinado na issue.
- Nada vai para o cliente sem revisão humana.
- Nunca expor chaves: o agente lê a chave da variável `GOEKIN_API_KEY` e nunca a escreve em arquivo, log ou mensagem.

## Como manter este índice

- Toda skill nova, alterada ou removida atualiza esta tabela no mesmo PR.
- Uma skill só vira `publicada` quando está no repositório e pode ser instalada.
- O formato do repositório (como empacotar, versionar, instalar e dar permissões) ainda vai ser decidido na [GOE-272](https://goekin-stg.eas-lab.com.br/goekin/issue/GOE-272). Até lá, cada skill fica numa pasta própria em `skills/<nome-da-skill>/`, com um `SKILL.md`.
