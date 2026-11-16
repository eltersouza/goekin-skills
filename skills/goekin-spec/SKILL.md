---
name: goekin-spec
description: Conduz uma entrevista guiada com quem pediu uma funcionalidade até chegar a um spec completo, e registra o spec numa issue do GoEkin em Backlog. Use quando disserem "quero uma feature de X, me ajuda a especificar", "faz o spec disso" ou quando uma issue estiver sem spec.
---

# goekin-spec: especificar por conversa

Esta é uma skill de **entrada**. Diferente da [`goekin-intake`](../goekin-intake/SKILL.md), que converte um texto pronto, aqui você **pergunta** até tirar as ambiguidades. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): a issue nasce em **Backlog** e o contexto (time, projeto, milestone) é perguntado antes de criar.

## Princípios

- **Quem decide é a pessoa.** Você propõe opções e registra a escolha. Nunca decida no lugar dela.
- **Não pergunte o que já está decidido.** Leia antes.
- **Poucas rodadas.** Rodadas curtas de 3 a 5 perguntas. Depois de 3 rodadas, feche o spec com o que tem e liste o que sobrou como pergunta em aberto.
- **"Decide você" é uma resposta válida.** Decida, registre como "decisão delegada" e siga.

## Passo a passo

### 1. Ler o contexto

Se existir issue, `get_issue`. Veja o projeto (`get_project`), as issues parecidas (`list_issues` com `query`) e as decisões já tomadas nos comentários (`list_comments`). Anote o que já responde às perguntas abaixo.

### 2. Entrevistar, em rodadas

Cubra, nesta ordem, só o que ainda falta:

1. **Problema e usuário:** quem tem a dor, o que acontece hoje, por que agora.
2. **Fluxo principal:** o caminho feliz, passo a passo.
3. **Casos de borda e erros:** o que acontece quando dá errado, vazio, duplicado, sem permissão.
4. **Fora de escopo:** o que não entra desta vez.
5. **Sucesso:** como saber que deu certo (métrica ou comportamento observável).

Para cada pergunta, ofereça 2 ou 3 opções com a consequência de cada uma, quando houver escolha. Registre cada decisão **como foi dita**, com quem decidiu e quando.

### 3. Montar o spec

```markdown
## Problema
## Proposta
## Regras
## Critérios de aceite
- [ ] ...
## Fora de escopo
## Perguntas em aberto

---
**Pedido original:** <as palavras da pessoa, intactas>
```

Critérios de aceite verificáveis, um por linha.

### 4. Perguntar o contexto e registrar

Pergunte time, projeto, milestone, épico pai, labels e prioridade (como no workflow), mostrando as opções existentes.

- **Issue nova:** `save_issue` em Backlog, sem ciclo.
- **Issue existente:** atualize a descrição com `expectedUpdatedAt`, preservando o pedido original, e não mude o status.
- Comente com `save_comment`, assinado, o **histórico das decisões**: o que foi decidido, por quem, e o que foi delegado.
- Se a conversa foi por voz ou reunião, anexe a transcrição com `upload_attachment`.

### 5. Se for grande

Se o spec pede várias issues ou entregas, diga isso e proponha a [`goekin-breakdown`](../goekin-breakdown/SKILL.md). Não crie sub-issues por conta própria.

## Não faça

- Mover a issue para a fazer ou ciclo.
- Inventar requisito que a pessoa não disse. O que não foi dito é pergunta em aberto.
- Estender a conversa indefinidamente.
