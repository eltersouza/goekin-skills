---
name: goekin-plan-cycle
description: Planeja o próximo ciclo (sprint) de um time do GoEkin pela capacidade. Propõe o que cabe, o que fica de fora e por quê, e só aplica (issues no ciclo, em a fazer) depois da ordem explícita do humano. Use quando pedirem "monta a sprint", "planeja o próximo cycle", ou alguns dias antes do início do próximo ciclo.
---

# goekin-plan-cycle: planejar o ciclo pela capacidade

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-plan-cycle**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-plan-cycle concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-plan-cycle**`.


Esta skill propõe o conteúdo do próximo ciclo a partir do Backlog e da capacidade do time. **A decisão é do humano.** A skill propõe, e só aplica com ordem explícita, porque aplicar significa mover issues para a fazer, e na [`goekin-workflow`](../goekin-workflow/SKILL.md) só um humano decide fazer.

## Quando usar

- "Monta a sprint 2", "planeja o próximo cycle do GOE".
- Alguns dias antes de o próximo ciclo começar.

**Time sem ciclo (modo Kanban):** esta skill não se aplica. No modo sem ciclo, a prioridade definida na issue basta. Diga isso ao humano e, se ele quiser, proponha só a ordem de prioridade do Backlog.

## Regras que valem sempre

- **Issue em Backlog nunca fica dentro de ciclo.** Entrar no ciclo e ir para a fazer acontecem juntos, na mesma gravação.
- **Nada é aplicado sem ordem explícita.** "Pode aplicar", "fecha assim" ou "aplica sem a GOE-x" são ordens. Silêncio ou "parece bom" não são: confirme.
- **Avise sempre que a proposta passar de 100% da capacidade.**

## Passo a passo

### 1. Ler o ciclo

- `list_cycles` com `includeCapacity: true` (ou `get_cycle` com `current` e `next`): datas, progresso e a **capacidade sugerida** do próximo ciclo. A capacidade vem com a unidade (`issues` ou `points`) e com `estimated: true` quando é cálculo automático.
- O ciclo atual: o que ainda não vai terminar. Issues abertas no fim do ciclo passam para o próximo e **já ocupam capacidade**. Liste essas issues como a primeira parte da proposta.
- Se o time não tem ciclo seguinte criado, avise e pare.

### 2. Ajustar a capacidade

- Pergunte ao humano sobre **ausências** no período (férias, feriados, dias fora). O GoEkin ainda não registra ausências. Reduza a capacidade na proporção e explique a conta.
- Se a capacidade está marcada como estimada e o time tem pouco histórico, diga que o número é um ponto de partida.
- Use a **unidade da capacidade**. Se ela for em pontos, issue sem estimativa não tem como entrar na conta: liste essas issues à parte e sugira estimar, ou conte cada uma como 3 pontos, dizendo que fez isso.

### 3. Ler o Backlog

- `list_issues` com `team`, `state: "backlog"` e `orderBy: "position"` para pegar a ordem do board, ou `orderBy: "priority"`.
- Para cada candidata, confira:
  - **Está pronta?** Tem critérios de aceite. Se não tem, ela não deveria entrar: sugira a [`goekin-plan-issue`](../goekin-plan-issue/SKILL.md) antes.
  - **Está desbloqueada?** Se é bloqueada por algo que não termina antes ou dentro do ciclo, fica de fora.
  - **Tem data?** Issues de milestone com data dentro ou logo depois do ciclo sobem na ordem.

### 4. Montar a proposta

Encha até a capacidade, nesta ordem: o que passa do ciclo atual, o que está bloqueando outras coisas, o que vence primeiro e o resto pela prioridade. Prefira trocar uma issue grande por duas pequenas de mais valor quando a grande não couber.

```markdown
**Proposta para <ciclo> (<datas>) — time <TIME>**

Capacidade: <n> <unidade> (<sugerida pelo GoEkin | ajustada: motivo>)
Carga proposta: <m> <unidade> (<m/n>%)

Objetivo do ciclo: <uma frase>

Vem do ciclo atual:
- GOE-x <título> — <est>

Entra:
1. GOE-a <título> — <est> — <por quê>
2. ...

Fica de fora:
- GOE-z <título> — <motivo: não cabe | bloqueada por GOE-w | sem critérios de aceite>

Se sobrar tempo: GOE-q, GOE-r
Perguntas: <...>
```

Se a carga passar de 100%, diga isso na primeira linha e sugira o que tirar.

### 5. Aplicar, só com ordem explícita

1. Pegue o `stateId` de a fazer (o status `unstarted` do time) com `list_issue_statuses`, e o ID do ciclo com `get_cycle`.
2. `issues_batch` em modo `atomic`, com `idempotencyKey` (ex.: `cycle-<time>-<número>-plan`), com um `update` por issue com `stateId` e `cycleId` juntos.
3. Registre o plano na **descrição do ciclo** com `update_cycle`, no mesmo formato da proposta: objetivo, frentes, "pronto quando", "se sobrar tempo" e uma linha final "Planejado em <data> com <humano>". Se o ciclo já tinha uma descrição, preserve o que o humano escreveu e acrescente.

As issues "se sobrar tempo" **ficam em Backlog**. Elas entram no ciclo só quando alguém mandar.

### 6. Responder

Confirme o que foi aplicado: quantas issues, a carga final contra a capacidade e o link do ciclo.

## O que esta skill nunca faz

- Mover issue para a fazer ou para o ciclo sem ordem explícita.
- Colocar issue em ciclo deixando ela em Backlog.
- Mudar as datas do ciclo ou a capacidade.
- Tirar do ciclo atual uma issue em andamento.

## Checklist

- [ ] O time tem ciclos e existe o próximo ciclo.
- [ ] Li a capacidade e a unidade dela, e ajustei pelas ausências que o humano informou.
- [ ] Contei primeiro o que passa do ciclo atual.
- [ ] Candidatas prontas, desbloqueadas e na ordem de prioridade e prazo.
- [ ] A proposta mostra a carga contra a capacidade e o que ficou de fora com o motivo.
- [ ] Só apliquei depois da ordem: estado e ciclo juntos, num batch atômico.
- [ ] O plano ficou registrado na descrição do ciclo.
