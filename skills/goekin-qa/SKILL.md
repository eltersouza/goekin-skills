---
name: goekin-qa
description: Valida uma entrega contra os critérios de aceite de uma issue do GoEkin, com evidência (testes rodados, passos manuais), e registra um veredito claro (aprovado, com ressalvas ou reprovado). Resultado vai para revisão (In Review), nunca para concluída. Use quando disserem "faz o QA da GOE-123", "testa esse PR" ou "dá o veredito".
---

# goekin-qa: testar a entrega e dar o veredito

Esta é uma skill de **QA**. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): o resultado vai **sempre** para **revisão** (In Review), seja aprovado, com ressalvas ou reprovado. Concluir é só com ordem humana, depois do merge.

## Passo a passo

### 1. Ler

`get_issue` e `list_comments` (critérios de aceite, handoffs, decisões). Identifique o PR e o **head exato do commit** que será testado. O veredito vale para esse commit e nada além.

### 2. Rodar a suíte

No head, rode unitários, integração, lint e typecheck, como o projeto define. Anote as **contagens** (passaram, falharam, ignorados). Falha na suíte oficial já pode ser bloqueante.

### 3. Testar critério por critério

Para cada critério, o que foi feito e o resultado. Inclua:

- O caminho feliz e os casos de borda e de erro.
- **Sabotagem:** quando o critério é "o bug não volta", reintroduza o defeito e confirme que um teste falha. Teste que não falha com o bug presente não prova nada.
- Evidência: logs, saída de comando, prints. Anexe com `upload_attachment`. Imagens em comentário ainda não são aceitas pelo GoEkin, então elas vão como anexo.

### 4. Veredito

Um de três, sem meio-termo escondido:

- **Aprovado:** todos os critérios atendidos.
- **Aprovado com ressalvas:** atendidos, com observações que não bloqueiam. Cada ressalva vira uma issue nova em **Backlog**, relacionada à de origem, no mesmo time e projeto dela (diga isso).
- **Reprovado:** algum critério não passou. Liste os **bloqueantes numerados**, com o que ocorre e como reproduzir.

Comente com `save_comment`, assinado:

```markdown
**<Agente>**

**Veredito: <aprovado | aprovado com ressalvas | reprovado>** — head `<sha curto>`

Suíte: <contagens>
Critérios:
1. <critério> — OK / FALHOU (<evidência>)

Bloqueantes: <lista numerada ou "nenhum">
Ressalvas: <lista ou "nenhuma">
Não verificado: <o que ficou de fora>
```

### 5. Depois do veredito

- Garanta que a issue esteja em **revisão**, em qualquer veredito.
- **Reprovado:** devolva ao desenvolvimento com a [`goekin-handoff`](../goekin-handoff/SKILL.md). O comentário diz o que falta. Quem decide o próximo passo é o humano.
- **Aprovado:** avise que o merge depende do GO do humano.
- **Reprovado depois do merge:** abra uma issue de correção em **Backlog**, relacionada à original. Não reabra a antiga.

## Não faça

- Aprovar sem rodar no head certo. Todo veredito cita o commit.
- Mover a issue para concluída ou fazer merge.
- Alterar o código para fazer o teste passar.
