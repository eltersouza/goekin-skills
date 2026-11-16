---
name: goekin-review-pr
description: Revisa um PR à luz da issue do GoEkin, conferindo critério de aceite por critério, escopo e riscos, e registra o resultado na issue. Resultado vai para revisão (In Review), nunca para concluída, e nunca aprova o merge. Use quando disserem "revisa o PR da GOE-123" ou quando um PR for aberto.
---

# goekin-review-pr: revisar o PR contra a issue

Esta é uma skill de **revisão**. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): o resultado vai **sempre** para **revisão** (In Review), seja favorável ou não. O resultado é uma **recomendação**. Quem decide o merge é o humano.

Esta skill **lê** o código. Quem **executa** o produto e a suíte é a [`goekin-qa`](../goekin-qa/SKILL.md). Se a pessoa pedir as duas, rode as duas.

## Passo a passo

### 1. Ler a issue

`get_issue` e `list_comments`: pedido original, critérios de aceite, decisões. Sem critérios de aceite, diga isso: a revisão será só de qualidade e escopo.

### 2. Ler o PR

Diff, testes alterados, descrição e resultado do CI (com `gh pr view`, `gh pr diff` e `gh pr checks`, ou o que o ambiente oferecer). Leia também as regras do repositório (`CLAUDE.md`, `AGENTS.md`).

### 3. Montar a matriz

| Critério de aceite | Resultado | Onde |
|---|---|---|
| <critério 1> | atendido / parcial / ausente | `arquivo:linha` |

Depois:

- **Escopo extra:** mudanças que não estavam na issue.
- **Riscos:** segurança (segredos, injeção, permissão), migration sem rollback, performance, compatibilidade, dado pessoal em log.
- **Testes:** o que cobre cada critério, e o que não cobre.
- **O que não foi verificado:** diga com clareza (por exemplo, "não rodei a suíte" ou "não testei no mobile"). Revisão superficial não pode parecer aprovação.

### 4. Registrar

- **Na issue:** `save_comment`, assinado, com o resumo (critérios atendidos e ausentes, riscos, a recomendação: *pode seguir para QA* ou *precisa de ajustes antes*, e o que não foi verificado).
- **No PR:** a revisão detalhada, como **comentário** (`gh pr review --comment`). **Não** use "approve" nem "request changes" sem ordem do humano, e **nunca** faça merge.
- Garanta que a issue esteja em **revisão**: se estiver em andamento e o PR já está aberto, mova.

### 5. Ajustes pedidos

Se precisar de ajustes, liste cada um numerado, e use a [`goekin-handoff`](../goekin-handoff/SKILL.md) para devolver ao desenvolvimento.

## Não faça

- Aprovar ou fazer merge.
- Mover a issue para concluída ou de volta para a fazer.
- Corrigir o código do PR por conta própria. Revisor aponta, autor corrige.
