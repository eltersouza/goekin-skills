---
name: goekin-plan-issue
description: Planeja a implementação de uma issue do GoEkin. Transforma uma issue rasa (um título ou uma frase) numa issue pronta para implementar, com contexto, abordagem, arquivos afetados, critérios de aceite, plano de testes, riscos e perguntas, tudo na própria issue. Use quando pedirem "planeja a GOE-123", quando uma issue não tiver critérios de aceite, ou antes de começar a implementar uma issue sem plano.
---

# goekin-plan-issue: planejar a implementação de uma issue

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-plan-issue**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-plan-issue concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-plan-issue**`.


Esta skill deixa uma issue pronta para implementar. O plano fica **na descrição da issue**, que é a fonte da verdade, e o pedido original do humano é preservado.

Ela segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): comentário assinado, nada de mover para a fazer, concluída ou cancelada sem ordem humana.

## Quando usar

- Pedido direto: "planeja a GOE-123".
- A issue não tem critérios de aceite e alguém vai começar a implementar. A `goekin-workflow` manda planejar antes.
- Uma issue que chegou rasa pelo intake (um título, uma frase, um e-mail colado).

## O planejamento não muda o status

O plano é uma proposta. A issue continua no status em que está, normalmente Backlog.

**Mover para a fazer é o humano aprovando o plano.** Por isso o agente nunca move a issue depois de planejar. Ele termina com o comentário e as perguntas.

## Passo a passo

### 1. Ler tudo o que existe

- `get_issue` com `includeRelations: true`: descrição, status, labels, projeto, milestone, pai e relações.
- `list_comments`: a discussão faz parte do pedido.
- `list_attachments` e `get_attachment`: prints, PDFs e logs anexados.
- `list_issues` com `query` usando termos do título: issues parecidas, dependências e decisões anteriores. Se o projeto ou o pai tiverem decisões registradas, leia também.

### 2. Ler o código

- Leia o código relevante no repositório (o GoEkin não tem o código). Encontre os módulos, arquivos e testes afetados e os padrões que o projeto já usa.
- **Nunca invente um plano sem ler o código.** O plano cita os arquivos que você leu.
- Se você não tem acesso ao repositório, diga isso no comentário e escreva só o que dá para planejar sem código, com a ressalva.

### 3. Decidir se dá para planejar

Pare e pergunte, em vez de inventar, quando:

- **For um bug vago** ("a tela trava às vezes"). Peça no comentário os passos de reprodução, o ambiente e o que era esperado.
- **O pedido tiver duas leituras** que levam a planos diferentes. Liste as leituras e pergunte qual vale.
- **O escopo for grande demais.** Se o plano passaria de 8 pontos ou de uma semana de trabalho, não escreva um plano gigante. Proponha a quebra com a skill [`goekin-breakdown`](../goekin-breakdown/SKILL.md).

Nesses casos, registre só o comentário com as perguntas e não mexa na descrição.

### 4. Escrever o plano na descrição

Use `save_issue` com `description` **e** `expectedUpdatedAt` (o `updatedAt` que você leu no passo 1). Se o humano editou a issue no meio do caminho, a gravação falha com `issue.version_conflict`. Nesse caso, leia de novo e refaça em cima da versão nova. Nunca sobrescreva.

Estrutura da descrição:

```markdown
## Pedido original

<o texto que estava na descrição, sem alteração; se estava vazia, o título>

## Contexto

<por que isso existe, o que já existe no código e no GoEkin, decisões anteriores com link>

## Abordagem

<como fazer, em passos curtos>

**Alternativas descartadas:** <o que foi considerado e por que não>

## Arquivos afetados

- `caminho/do/arquivo` — <o que muda>

## Critérios de aceite

1. <verificável, do ponto de vista de quem usa>
2. ...

## Plano de testes

- <testes automatizados a criar ou ajustar>
- <roteiro manual, se fizer sentido>

## Riscos

- <o que pode dar errado e como mitigar>

## Perguntas

- <o que o humano precisa decidir antes de implementar>
```

Regras da descrição:

- **O "Pedido original" nunca é editado nem resumido.** Se a issue já tinha um plano de uma rodada anterior, atualize as seções do plano e mantenha o "Pedido original" como estava.
- Critérios de aceite numerados, verificáveis e sem detalhe de implementação.
- Sem seção vazia: se não há riscos, escreva "Nenhum identificado".

### 5. Estimativa, labels e relações

- **Estimativa:** se a issue não tem estimativa, preencha com `estimate` (1, 2, 3, 5, 8 ou 13). Se já tem, não troque. Sugira a nova no comentário.
- **Labels:** não aplique. Sugira no comentário.
- **Relações:** não aplique. Sugira no comentário as que encontrou (`blockedBy`, `relatedTo`), com o motivo.

### 6. Comentar

Comentário assinado com `save_comment`:

```markdown
**<Agente>, via GSS · goekin-plan-issue**

Plano escrito na descrição. Li: `<arquivo 1>`, `<arquivo 2>`, ...

- Estimativa: <n> (<preenchida | sugerida no lugar de <m>>)
- Labels sugeridas: <...>
- Relações sugeridas: <GOE-x bloqueia esta, porque ...>

Perguntas:
1. <...>

Se o plano estiver bom, mova para a fazer.
```

## O que esta skill nunca faz

- Mudar o status da issue.
- Criar sub-issues. Ela só propõe a quebra, e quem cria é a `goekin-breakdown`, com o ok do humano.
- Apagar ou reescrever o pedido original.
- Colocar chaves, tokens ou dados pessoais na descrição ou no comentário.

## Checklist

- [ ] Li a issue com as relações, os comentários, os anexos e as issues parecidas.
- [ ] Li o código e citei os arquivos.
- [ ] Bug vago, pedido ambíguo ou escopo grande: só perguntei ou propus a quebra.
- [ ] Gravei a descrição com `expectedUpdatedAt` e o "Pedido original" intacto.
- [ ] Estimativa só preenchida se estava vazia. Labels e relações só sugeridas.
- [ ] Comentei com a assinatura, o resumo e as perguntas. O status não mudou.
