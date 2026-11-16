---
name: goekin-monitor-cycle
description: Acompanha o ciclo (sprint) em andamento no GoEkin e avisa cedo quando ele está em risco, com burn-down, escopo adicionado, bloqueios e uma projeção de término. Use quando perguntarem "como está a sprint?", "dá para entregar na sexta?" ou em rotina diária durante o ciclo.
---

# goekin-monitor-cycle: diagnóstico do ciclo

A skill **lê e avisa**. Mudanças de escopo e o encerramento do ciclo são do humano. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md). Para times sem ciclo (Kanban), não se aplica.

## Passo a passo

### 1. Ler

- `get_cycle` com `current`: datas, progresso, capacidade.
- `list_issues` com `cycle: "current"` e o time: estados, estimativas, responsáveis.
- `list_activity` desde a última checagem (peça a data, ou use "ontem"), para ver o que mudou.

### 2. Calcular

Na unidade do ciclo (pontos ou issues):

- **Concluído contra esperado hoje** (linha reta do início ao fim, descontando fim de semana quando o time não trabalha).
- **Escopo adicionado** depois do início (issues que entraram no ciclo com data posterior à de início).
- **Projeção:** no ritmo real até agora, o que termina até o fim do ciclo.

O GoEkin não guarda o histórico do burn-down: o cálculo é aproximado e a skill diz isso.

### 3. Procurar risco

- Issue **bloqueada** (veja `get_issue` com `includeRelations`) e quem a bloqueia.
- Issue **grande** em andamento sem comentário ou atualização há dias.
- Responsável com **carga demais** em comparação com os outros.
- Issues em revisão esperando há muito.
- Bugs urgentes que entraram no meio e tiram o escopo planejado.

### 4. Diagnóstico

```markdown
**Ciclo <nome> — dia <x> de <y>**

Situação: <no ritmo | atrasado em ~<n> | adiantado>
Concluído: <n/m> <unidade> (esperado hoje: <k>)
Entrou depois do início: <n>
Projeção: termina <p> de <m> <unidade> até <data>

Riscos:
- GOE-a bloqueada por GOE-b (<motivo>)
- GOE-c: 5 pontos, em andamento há 6 dias, sem atualização

Opções (decisão sua): tirar GOE-d do ciclo; quebrar GOE-c; mover o prazo.
```

**Avise só quando a projeção mudar de verdade.** Sem mudança relevante desde a última checagem, diga "sem mudança" em uma linha.

### 5. Entregar

Responda na conversa. Se a pessoa pedir registro, comente, assinado, na issue que ela indicar. Em issue específica em risco, comente o que está parado (`save_comment`), no máximo um comentário por issue por semana.

### 6. Fim do ciclo

Prepare os números para a [`goekin-retro`](../goekin-retro/SKILL.md). O encerramento (`complete_cycle`) é só do humano, com ordem direta.

## Não faça

- Tirar ou pôr issue no ciclo, mudar prazo ou capacidade sem ordem.
- Encerrar o ciclo.
- Dar alarme sem mudança de projeção.
