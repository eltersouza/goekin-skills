---
name: goekin-estimate
description: Sugere a estimativa (1, 2, 3, 5, 8 ou 13) de issues do GoEkin com justificativa, usando o histórico de issues já concluídas do próprio time. Use quando pedirem "estima essas issues", "quanto vale essa issue" ou antes de planejar um ciclo com issues sem estimativa.
---

# goekin-estimate: estimar pelo histórico do time

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-estimate**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-estimate concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-estimate**`.


A estimativa é **uma sugestão**, não um compromisso. A skill dá um número, a confiança e as issues de referência, para o time ganhar tempo sem planning poker. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md).

## Passo a passo

### 1. Ler a issue

`get_issue`. Sem critérios de aceite, a estimativa é fraca: diga isso e sugira a [`goekin-plan-issue`](../goekin-plan-issue/SKILL.md) antes.

### 2. Montar a referência

`list_issues` com `state: "completed"`, o mesmo time, e as que têm estimativa. Compare por título, labels e área. Use o tempo entre `startedAt` e `completedAt` como apoio, sabendo que ele inclui espera.

- Menos de 10 issues concluídas com estimativa: **confiança baixa**, e diga isso de forma explícita.
- O time não usa estimativa (só conta issues): avise e não force.

### 3. Sugerir

Para cada issue, devolva:

```markdown
GOE-123: <título> → **5** (confiança média)
Referências: GOE-80 (5, levou 3 dias), GOE-91 (3, levou 2 dias)
Por quê: <duas frases: o que pesa nesta issue em relação às referências>
```

Escala: 1, 2, 3, 5, 8, 13. **Acima de 8, não estime: proponha a [`goekin-breakdown`](../goekin-breakdown/SKILL.md).**

Aponte padrões quando existirem (por exemplo, "issues de pagamento levaram o dobro do estimado").

### 4. Gravar

- **Issue sem estimativa:** grave com `save_issue` (`estimate`, com `expectedUpdatedAt`) **só se a pessoa pediu para gravar**. Sem esse pedido, comente a sugestão com `save_comment`, assinada.
- **Issue que já tem estimativa:** **nunca sobrescreva.** Se discordar, comente a diferença e o motivo.
- Não mude o status.

## Não faça

- Tratar a sugestão como compromisso.
- Sobrescrever estimativa humana.
- Estimar sem ler a issue.
