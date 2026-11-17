---
name: goekin-refine-backlog
description: Refina e prioriza o Backlog de um time do GoEkin. Classifica cada issue (pronta, sem spec, sem estimativa, bloqueada), propõe a ordem e as prioridades com justificativa, e só aplica o que o humano aprovar. Use quando pedirem "refina o backlog", "o que vale a próxima sprint" ou antes do planejamento do ciclo.
---

# goekin-refine-backlog: refinar e priorizar

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-refine-backlog**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-refine-backlog concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-refine-backlog**`.


Deixa o topo do Backlog pronto para o planejamento. **A skill propõe e o humano aprova.** Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): nenhuma issue sai do Backlog por aqui sem o comando explícito do humano para movê-la para a fazer.

## Passo a passo

### 1. Ler

`list_issues` com `team` e `state: "backlog"`, em `orderBy: "position"`. Leia com `get_issue` as do topo (até umas 20, avisando o corte). Veja `list_projects` e `list_milestones` para as datas.

Pergunte o **objetivo do momento** se não estiver claro (cliente com prazo, estabilizar, lançar o MVP). Sem isso, a prioridade vira palpite.

### 2. Classificar cada issue

| Classe | Significa | Próximo passo |
|---|---|---|
| Pronta | Tem critérios de aceite e está desbloqueada | Candidata ao ciclo |
| Sem spec | Falta objetivo ou critério | [`goekin-plan-issue`](../goekin-plan-issue/SKILL.md) ou [`goekin-spec`](../goekin-spec/SKILL.md) |
| Sem estimativa | Falta número | [`goekin-estimate`](../goekin-estimate/SKILL.md) |
| Bloqueada | Depende de algo aberto | Dizer do quê |
| Grande | Passa de 8 ou mistura entregas | [`goekin-breakdown`](../goekin-breakdown/SKILL.md) |
| Candidata a arquivar | Perdeu o sentido ou é duplicata | Sugerir a [`goekin-backlog-hygiene`](../goekin-backlog-hygiene/SKILL.md) |

### 3. Propor a ordem

Critério livre, em palavras, ponderando: **impacto para o cliente**, **urgência e datas de milestone**, **dependências** (a que destrava outras sobe) e **esforço**. Se a pessoa pedir um modelo (RICE, ICE, valor por esforço), use o que ela pedir e mostre a conta.

```markdown
**Proposta de ordem — time <TIME>**
1. GOE-x <título> — prioridade <atual → proposta> — <por quê, em uma frase>
...

Mudanças de prioridade: <n>. Mudanças de posição: <n>.
Pendências: GOE-y precisa de spec; GOE-z depende de GOE-w, que ninguém priorizou.
```

### 4. Aplicar só o que for aprovado

Mostre o **diff** (o que muda de prioridade e de posição) antes. Depois do ok explícito:

- Prioridade: `save_issue` com `priority` e `expectedUpdatedAt`.
- Posição: `save_issue` com `after` e `before`, uma issue por vez, sem derrubar a ordem que o humano montou.
- Comente nas issues alteradas, em uma linha assinada, o motivo.

### 5. Resumo

Deixe o resumo do refinamento num comentário, na issue que a pessoa indicar, ou na conversa.

## Não faça

- Mover issue para a fazer ou para ciclo.
- Reordenar em massa sem mostrar o diff.
- Virar dona da prioridade: o humano pode discordar de tudo.
