---
name: goekin-docs
description: Gera e atualiza a documentação (README, arquitetura, referência de API, onboarding) a partir do código e das issues do GoEkin, sempre por PR, citando a origem de cada afirmação. Use quando disserem "atualiza a documentação", "gera o guia de onboarding" ou depois de um release.
---

# goekin-docs: documentação a partir do código

Esta é uma skill de **execução**: ela faz o trabalho de uma issue (a de documentação). Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): a issue vai para **em andamento** ao começar e para **revisão** ao abrir o PR, e o trabalho sempre sai por **PR**, nunca direto na principal.

## Antes de começar

1. **Precisa de issue.** Sem uma, crie uma em Backlog (perguntando o contexto) e peça ao humano que a mova para a fazer, ou que mande você movê-la. Só então comece.
2. Pergunte o **público** de cada documento: interno (time), parceiro ou cliente final. O que é interno não entra em documento público. Na dúvida, trate como interno.
3. Leia as regras do repositório (`CLAUDE.md`, `AGENTS.md`) e o formato das docs que já existem.

## Passo a passo

### 1. Levantar o que mudou

- O código, o OpenAPI e os testes do repositório.
- As issues concluídas desde a última atualização da documentação (`list_issues`, `state: "completed"`, `updatedAt`).

### 2. Comparar

Para cada documento, liste as **divergências**: o que o código faz e a documentação não diz, o que a documentação diz e o código não faz mais, exemplos que não rodam.

### 3. Escrever

- **Toda afirmação cita sua origem:** arquivo e linha, rota do OpenAPI ou issue. Se não consegue citar, **não escreva**.
- Exemplos de código têm de ser executados antes de entrar.
- Não invente comportamento. O que ficou ambíguo vira pergunta em aberto, no PR.
- Nunca inclua chave, token, URL interna sensível ou dado pessoal.

### 4. Divergências que pedem decisão

Quando a documentação e o código discordam e não está claro qual está certo, **não escolha**. Abra uma issue em **Backlog**, com label de documentação (`list_issue_labels`), relacionada à atual, descrevendo as duas versões. Pergunte o contexto, como no workflow.

### 5. Abrir o PR

Branch `goe-123-docs-<resumo>`, título `GOE-123: <resumo>`, link da issue. Mova a issue para revisão, e comente (`save_comment`, assinado) o link do PR e o que mudou. Nas issues de origem, comente o link da documentação atualizada.

## Não faça

- Commitar direto na principal.
- Documentar o que não consegue verificar no código.
- Publicar documentação interna como se fosse pública.
