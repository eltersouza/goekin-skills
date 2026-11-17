---
name: goekin-breakdown
description: Quebra um épico, projeto ou spec grande do GoEkin em milestones e issues pequenas, ordenadas e com dependências. Mostra a proposta ao humano e só cria depois do ok, tudo em Backlog. Use quando pedirem "quebra este épico", num projeto novo sem issues, numa issue grande demais (estimativa acima de 8), ou para ordenar as issues que já existem num projeto.
---

# goekin-breakdown: quebrar épico ou projeto em issues e milestones

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-breakdown**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-breakdown concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-breakdown**`.


Esta skill transforma uma coisa grande no caminho até a entrega: milestones com data, issues pequenas, a ordem entre elas e o caminho crítico.

Ela segue a [`goekin-workflow`](../goekin-workflow/SKILL.md). O ponto central é que **nada é criado sem o ok do humano**.

## Quando usar

- "Quebra a GOE-271", "quebra o projeto X".
- Um projeto novo, ainda sem issues.
- Uma issue que a [`goekin-plan-issue`](../goekin-plan-issue/SKILL.md) achou grande demais (acima de 8 pontos).
- "Ordena as issues deste projeto": nesse caso, pule a criação e faça só os passos de ordem e dependências.

## Épico é issue, projeto é projeto

- **Épico (uma issue):** as partes viram **sub-issues** (`parentId` = o épico). Se o épico estiver num projeto, as filhas herdam o projeto.
- **Projeto:** as partes viram **issues do projeto**, agrupadas em **milestones**. Se um pedaço do projeto for grande, ele vira uma issue-pai com sub-issues.
- Milestone só existe dentro de projeto. Épico sem projeto não ganha milestone.

## Passo a passo

### 1. Ler

- Épico: `get_issue` com `includeRelations: true`, `list_comments` e as filhas que já existem (`list_issues` com `parentId`).
- Projeto: `get_project` com `includeMilestones: true`, `list_milestones` e as issues que já existem (`list_issues` com `project`).
- O spec, se houver (na descrição, num anexo ou num doc citado), e as datas alvo.
- O código, se a quebra depender de como o sistema está organizado.

### Antes de propor: perguntar o contexto

Esta é uma skill de entrada: ela cria issues, então segue a regra de [`goekin-workflow`](../goekin-workflow/SKILL.md) para issue nova. Antes de montar a proposta, confirme com o humano o que ainda não estiver claro:

- **Time** das issues.
- **Projeto** e **milestones**: mostre os que existem (`list_projects`, `list_milestones`) e pergunte se usa algum deles ou se cria novos.
- **Épico pai**, labels do time e prioridade.
- **Datas alvo** e o que já foi decidido sobre o escopo.

Sem resposta, não invente: crie em Backlog sem projeto nem milestone e diga isso no comentário.

### 2. Montar a proposta

- **Tamanho:** cada issue entre 1 e 5 pontos. Nem uma hora de trabalho, nem duas semanas. Uma issue de 8 é aceitável se não der para dividir de um jeito que faça sentido. Acima disso, divida.
- **Cada issue entrega algo verificável**, com título que diga o que fazer e 1 a 3 critérios de aceite. A `goekin-plan-issue` aprofunda depois, quando for a vez dela.
- **Dependências:** marque só bloqueio real ("B não começa sem A"). Não marque bloqueio por preferência de ordem.
- **Milestones (projeto):** cada milestone tem nome, data alvo e critério de saída. Use as datas que o humano deu. Se não deu, proponha datas e marque como sugestão.
- **Caminho crítico:** a sequência de bloqueios mais longa até a entrega.
- **O que já existe:** reaproveite as issues existentes. Não crie duplicatas. Diga quais você encaixou onde.

### 3. Mostrar e esperar o ok

Mostre a proposta ao humano **antes de criar qualquer coisa**:

```markdown
**Proposta de quebra: <épico ou projeto>**

Milestone 1 — <nome> (alvo <data>)
Saída: <critério>
1. <título> — <estimativa> pts
2. <título> — <estimativa> pts — bloqueada por 1
...

Caminho crítico: 1 → 2 → 5 → 9 (<total> pts)
Já existiam e encaixei: GOE-x em M1, GOE-y em M2
Total: <n> issues, <p> pontos, <m> milestones
Perguntas: <...>
```

Se a conversa for ao vivo, mostre no chat. Se você estiver trabalhando sem ninguém olhando, poste a proposta como comentário assinado no épico ou no projeto e pare até o humano responder.

**Só crie com ordem explícita.** "Pode criar", "manda ver" ou "cria só a M1" são ordens. Silêncio não é.

### 4. Criar

Depois do ok, na ordem abaixo:

1. **Milestones:** `save_milestone` para cada uma (`name`, `targetDate` e `exitCriteria` são obrigatórios na prática).
2. **Issues:** `issues_batch` em modo `atomic`, com um `idempotencyKey` único (ex.: `breakdown-<id do épico>-<data>`), para criar tudo de uma vez ou nada. Cada item `create` leva `teamId`, `title`, `description`, `stateId` do Backlog do time, `estimate` e, quando houver, `parentId`, `projectId` e `milestoneId`. O batch usa IDs: pegue o `stateId` com `list_issue_statuses` e os IDs de projeto e milestone nas leituras.
3. **Dependências:** o batch não grava relações. Para cada bloqueio, use `save_issue` com `blockedBy`.
4. **Ordem no board:** se a ordem importar, use `save_issue` com `after` para deixar as issues na sequência do caminho.

Regras da criação:

- **Tudo em Backlog e sem ciclo.** Nunca crie direto em a fazer.
- A prioridade é só sugestão. Na dúvida, deixe sem prioridade.
- Nada de labels novas. Use as do time, e só as que o humano pediu ou que a issue-pai já usa.

### 5. Comentar no épico ou no projeto

Comentário assinado com `save_comment` no épico. Se for um projeto, comente na primeira issue do caminho crítico e diga isso ao humano:

```markdown
**<Agente>, via GSS · goekin-breakdown**

Quebra criada por ordem de <humano>:

- M1 <nome> (<data>): GOE-a, GOE-b, GOE-c
- M2 <nome> (<data>): GOE-d, GOE-e

Caminho crítico: GOE-a → GOE-b → GOE-e
Tudo em Backlog. Para planejar cada uma antes de implementar, use a goekin-plan-issue.
```

## Quando o épico muda

A skill não reconcilia sozinha. Se pedirem para reconciliar:

1. Compare o épico atual com as filhas.
2. Proponha a diferença: o que criar, o que mudar e o que deixou de fazer sentido.
3. Com o ok, aplique. Issue que não faz mais sentido vai para cancelada ou é arquivada, **só com ordem explícita**. Nada é apagado.

## O que esta skill nunca faz

- Criar milestone ou issue sem o ok do humano.
- Mover issue para fora de Backlog ou colocar em ciclo.
- Cancelar ou arquivar sem ordem explícita.
- Criar issues de 13 pontos "para quebrar depois".

## Checklist

- [ ] Li o épico ou projeto, as filhas que já existem e o spec.
- [ ] Issues de 1 a 5 pontos, com critérios de aceite e bloqueios reais.
- [ ] Mostrei a proposta com o caminho crítico e esperei o ok.
- [ ] Criei as milestones, depois as issues num `issues_batch` atômico com `idempotencyKey`, depois as dependências.
- [ ] Tudo em Backlog, sem ciclo.
- [ ] Comentei a árvore e o caminho crítico.
