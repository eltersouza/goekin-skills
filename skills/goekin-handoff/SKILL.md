---
name: goekin-handoff
description: Padroniza a passagem de trabalho de um agente para outro, ou entre agente e humano, usando a issue do GoEkin como contrato. Registra o que foi feito, o que falta, onde está o código e a próxima ação. Use quando uma etapa terminar, quando um agente for parar no meio, ou quando disserem "passa isso para o QA" ou "deixa um handoff".
---

# goekin-handoff: passar o trabalho com contexto

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-handoff**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-handoff concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-handoff**`.


A issue é o contrato. Quem recebe não deveria precisar perguntar nada a quem passou. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md).

**Sobre o status:** o handoff só **passa** o trabalho. Quem **começa** é o próximo agente, e é a skill de execução dele que move a issue para em andamento. O handoff só muda o status quando a etapa que termina pede isso, e nunca para a fazer nem para concluída.

## Passo a passo

### 1. Ler

`get_issue` e `list_comments`. Confirme o que de fato aconteceu (PR aberto, branch, testes), sem repetir o que o comentário anterior já diz.

### 2. Escrever o comentário de handoff

Todos os campos são obrigatórios. Campo sem resposta diz "nada" ou "não sei", e não some.

```markdown
**<Agente>, via GSS · goekin-handoff**

**Handoff para <pessoa ou agente>**

**Feito:** <o que está pronto>
**Pendente:** <o que falta, na ordem>
**Onde está:** <branch, PR, commit (head), ambiente>
**Como testar / verificar:** <comandos ou passos>
**Cuidados:** <armadilhas, decisões já tomadas, o que não mexer>
**Próxima ação esperada:** <uma frase>
```

### 3. Atualizar a issue

- Troque o responsável com `save_issue` (`assignee`), com `expectedUpdatedAt`. Para achar o usuário, `list_users`.
- Status:
  - Passando para **QA ou revisão** com PR aberto: a issue fica em **revisão**.
  - Passando para quem vai **continuar o trabalho**: o status não muda. O receptor move para em andamento ao começar.
  - Issue em Backlog ou a fazer: não mude. Só avise que o trabalho ainda não foi autorizado a começar.

### 4. Avisar

Avise o próximo responsável pelo canal combinado (a conversa, uma menção no comentário). O GoEkin ainda não manda evento sozinho.

### 5. Quem recebe confirma

O receptor comenta, assinado, que assumiu ("Assumi. Começando por X"). Sem essa confirmação, o handoff não está completo.

## Voltas entre agentes

Se a mesma issue voltar de QA para desenvolvimento e de volta **3 vezes**, pare e chame o humano com um resumo do impasse. Não deixe os agentes em loop.

## Não faça

- Passar adiante sem os campos obrigatórios.
- Mover a issue para a fazer ou para concluída.
- Assumir o trabalho do outro sem a confirmação dele.
