---
name: goekin-backlog-hygiene
description: Encontra e propõe a limpeza do que polui o board do GoEkin, como duplicatas, issues paradas, issues sem dono e inconsistências de status. Aplica só o que o humano aprovar. Use em rotina semanal ou mensal, ou quando pedirem "faz uma faxina no board" ou "tem issue duplicada?".
---

# goekin-backlog-hygiene: limpar o board

A skill **acha e propõe**. Só aplica o que o humano aprovar no relatório. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md). Nada se apaga no GoEkin: o fim da linha é arquivar, que é reversível.

## Passo a passo

### 1. Listar

`list_issues` do time, paginando (`cursor`), dos estados ativos (backlog, a fazer, em andamento, revisão), com `updatedAt`, responsável e ciclo.

### 2. Detectar

Os limites abaixo são o padrão. Se o time tiver outros, use os dele.

| O que procurar | Regra |
|---|---|
| **Duplicatas** | Títulos e descrições parecidos. Confirme lendo as duas com `get_issue`. |
| **Paradas** | Em andamento sem atualização há mais de 14 dias; a fazer sem responsável há mais de 30; em revisão sem PR citado nos comentários. |
| **Inconsistências** | Issue em Backlog com ciclo; sub-issues abertas de pai concluído; `blockedBy` apontando para issue concluída (`get_issue` com `includeRelations`). |
| **Órfãs** | Sem projeto, sem pai e sem label há muito tempo. |

Não há busca por similaridade no GoEkin: a comparação de texto é sua. Na dúvida, trate como "possível duplicata" e deixe o humano decidir.

### 3. Relatório

```markdown
**Higiene do backlog — time <TIME> — <data>**

Duplicatas (<n>)
- GOE-a e GOE-b: <por que parecem iguais> → sugestão: marcar GOE-b como duplicata de GOE-a

Paradas (<n>)
- GOE-c: em andamento há 21 dias sem comentário → sugestão: pedir atualização a <responsável>

Inconsistências (<n>)
- GOE-d: em Backlog, mas está no ciclo 3 → <ação humana>
```

Cada item traz **a ação proposta**.

### 4. Aplicar, depois do ok

Cada ação só roda se o humano aprovar aquele item (ou todos, de forma explícita):

- Duplicata: `save_issue` com `duplicateOf`. É uma transição para um status cancelado, então só com ok.
- Arquivar: `archive_issue`.
- Parada: `save_comment` assinado pedindo atualização. **No máximo um comentário por issue por semana.**
- Backlog dentro de ciclo: o agente **não mexe em ciclo**. Aponte no relatório e peça ao humano para resolver, ou pela ordem direta dele.

## Não faça

- Arquivar, cancelar ou marcar duplicata sem ok.
- Cobrar o responsável mais de uma vez por semana.
- Mover issue de status por conta própria.
