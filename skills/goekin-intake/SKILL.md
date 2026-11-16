---
name: goekin-intake
description: Transforma e-mail, mensagem, transcrição de reunião ou documento em issues do GoEkin, sempre em Backlog, sem duplicar o que já existe e guardando a fonte. Use quando alguém colar ou anexar um texto e pedir "vira issue", "abre as tarefas dessa reunião" ou "registra esse pedido do cliente".
---

# goekin-intake: de texto de fora para issues

Esta é uma skill de **entrada**. Ela segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): toda issue criada entra em **Backlog**, sem ciclo, e o contexto (time, projeto, milestone) é perguntado antes de criar.

## Quando usar

- Alguém cola ou anexa um e-mail de cliente, uma mensagem, uma transcrição ou um PDF de requisitos e pede para virar issue.
- Não use para conversa em que a pessoa ainda está decidindo o que quer: nesse caso use a [`goekin-spec`](../goekin-spec/SKILL.md).

## Passo a passo

### 1. Separar os pedidos

Leia o texto inteiro e separe em pedidos distintos: bug, pedido de funcionalidade, dúvida, reclamação sem ação. **Prefira menos issues e mais ricas.** Uma reclamação com um problema só vira uma issue, não dez.

Se o texto for imagem e você não conseguir ler, peça o texto. Não adivinhe.

### 2. Procurar duplicatas

Para cada pedido, rode `list_issues` com `query` (palavras do pedido). Se existir uma issue que já cobre o pedido, **comente nela** com `save_comment` (a nova ocorrência, a data e quem pediu) em vez de criar outra.

### 3. Perguntar o contexto

Antes de criar, pergunte o que ainda não está claro, como manda o workflow: time, projeto, milestone (liste com `list_projects` e `list_milestones`), épico pai, labels, prioridade e o contexto (quem pediu, de onde veio). Mostre numa mensagem só a lista do que pretende criar e o que vai para issues existentes, junto com as perguntas.

Sem ninguém para responder, crie em Backlog sem projeto nem milestone e diga isso no comentário.

### 4. Proteger dados pessoais

**Mascare por padrão** CPF, telefone, e-mail pessoal, endereço e qualquer token ou senha que apareça no texto, tanto na descrição quanto no que for anexado. Se o dado for essencial para resolver, diga na issue que ele existe na fonte e quem tem acesso.

### 5. Criar as issues

- Título que diga o que fazer.
- Descrição: contexto, o que a pessoa disse (citação curta, entre aspas), impacto e critérios de aceite iniciais quando for possível deduzi-los. O que não foi dito fica como pergunta em aberto, nunca inventado.
- Labels do time (`list_issue_labels`), como bug ou feature.
- Várias issues de uma vez: use `issues_batch` com `idempotencyKey`, como na [`goekin-breakdown`](../goekin-breakdown/SKILL.md). Issues vindas do mesmo texto ficam ligadas com `relatedTo`.
- A prioridade é só sugestão.

### 6. Guardar a fonte

Anexe o texto original com `upload_attachment`, já mascarado, e cite a origem na descrição ("e-mail de <cliente>, de <data>").

### 7. Devolver o resumo

Diga o que virou issue (com o identificador), o que foi para issue existente e o que foi ignorado, com o motivo. Se a pessoa pediu resposta ao cliente, escreva um **rascunho** e entregue na conversa. **Nunca envie.**

## Não faça

- Mover issue para a fazer, ciclo ou concluída.
- Responder ao cliente sem revisão humana.
- Colar dado pessoal sem máscara.
