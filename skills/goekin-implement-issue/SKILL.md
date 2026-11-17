---
name: goekin-implement-issue
description: Leva uma issue planejada do GoEkin até um PR aberto. Cria a branch, implementa, roda testes, abre o PR e mantém a issue atualizada (em andamento, depois revisão), sem fazer merge. Use quando mandarem "implementa a GOE-123", "pega essa issue" ou "faz o PR dessa tarefa".
---

# goekin-implement-issue: da issue ao PR

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-implement-issue**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-implement-issue concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-implement-issue**`.


Esta é uma skill de **execução**. Ela segue a [`goekin-workflow`](../goekin-workflow/SKILL.md) à risca: ao começar, a issue vai **sempre** para **em andamento**, com o comentário do plano; ao abrir o PR, vai para **revisão**. O GoEkin é a fonte da verdade do andamento.

## Antes de começar

1. **Leia o repositório:** `CLAUDE.md`, `AGENTS.md` e o README. As regras do projeto (testes, lint, estilo de commit) valem.
2. **Leia a issue** (`get_issue`, `list_comments`) e os status do time (`list_issue_statuses`).
3. **Confira o estado:**
   - Em **a fazer**, ou o humano mandou trabalhar nela: siga.
   - Em **Backlog**: **não comece.** Peça ao humano que a mova para a fazer, ou que mande você movê-la com um comando explícito.
   - **Já em andamento** com outro responsável: pare e pergunte.
4. **Confira os critérios de aceite.** Sem eles, pare e use a [`goekin-plan-issue`](../goekin-plan-issue/SKILL.md). Se a issue for grande demais, a [`goekin-breakdown`](../goekin-breakdown/SKILL.md).

## Passo a passo

### 1. Começar

- Mova a issue para **em andamento** (`save_issue` com `state`, com `expectedUpdatedAt`) e assuma com `assignee: "me"`.
- Comente o plano curto, assinado (modelo do workflow).
- Crie a branch a partir da principal atualizada: `goe-123-resumo-curto`.

### 2. Implementar

- Faça só o que a issue pede. Trabalho novo descoberto vira issue em Backlog, relacionada, sem ampliar o escopo (veja o workflow).
- Rode testes, lint e typecheck do projeto. Corrija o que quebrou por sua causa.
- Acompanhe os critérios de aceite um a um, e marque os que ficaram atendidos.
- Nunca grave chave, token ou dado pessoal em código, log ou comentário.

### 3. Abrir o PR

- Título `GOE-123: resumo`, descrição com o link da issue, os critérios atendidos e como testar.
- Mova a issue para **revisão** e comente o link do PR, o que mudou, as contagens de teste e **o que ficou de fora**.
- Se o time não tiver status de revisão, a issue fica em andamento, e o comentário registra o PR.

### 4. Se travar

Registre o bloqueio com `blockedBy` e um comentário (o que falta e de quem depende), e pare. Não siga adivinhando.

### 5. Depois do PR

- Peça a revisão: [`goekin-review-pr`](../goekin-review-pr/SKILL.md) ou [`goekin-qa`](../goekin-qa/SKILL.md), ou o revisor humano.
- Se o PR for fechado **sem merge**, a issue fica em revisão, com um comentário explicando, e o humano decide o próximo passo.
- Corrigindo a pedido da revisão, a issue volta para **em andamento** enquanto você trabalha, e para revisão quando atualizar o PR.

## Não faça

- Fazer merge. Merge só com QA e o GO do humano.
- Mover a issue para concluída. Isso é só com ordem humana, depois do merge.
- Começar uma issue que estava em Backlog.
