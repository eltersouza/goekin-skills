---
name: goekin-release
description: Prepara um release a partir das issues concluídas no GoEkin, com changelog técnico, release notes em linguagem de cliente e checklist de deploy, tudo como rascunho. Depois do deploy, registra a versão nas issues. Use quando pedirem "prepara o release", "gera o changelog da v0.4.0" ou no fim do ciclo.
---

# goekin-release: changelog, release notes e checklist

## Anuncie que esta skill está em uso

- **Ao começar**, a primeira linha da resposta é `**GSS · goekin-release**`, seguida de uma frase com o que vai fazer. Assim a pessoa sabe qual skill da suíte está ativa, mesmo sem tê-la chamado pelo nome.
- **Ao terminar, ou ao passar para outra skill**, diga `GSS · goekin-release concluída` e, se houver, a próxima (`Próxima: <skill>`).
- Os comentários que postar no GoEkin levam a assinatura `**<Agente>, via GSS · goekin-release**`.


A skill **prepara**. O deploy é de um humano ou de um pipeline aprovado, e as notas são sempre **rascunho** até alguém revisar. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md). Esta skill não muda o status de nenhuma issue.

## Passo a passo

### 1. Definir o recorte

Pergunte (ou leia da conversa): a **versão** e a **data do último release** (ou a tag anterior). Sem isso, não dá para saber o que entrou.

### 2. Listar o que entrou

`list_issues` com `state: "completed"`, do time ou projeto, com `updatedAt` desde a data do último release. O GoEkin ainda não filtra por data de conclusão: confira `completedAt` em cada issue com `get_issue` e descarte as que ficaram fora.

Confira também os PRs mergeados do período, quando o repositório estiver disponível (`gh pr list --state merged`). Issue concluída sem PR mergeado, ou PR mergeado sem issue, vai para uma lista de **inconsistências** que o humano precisa ver.

### 3. Classificar

Agrupe pelas labels do time (`list_issue_labels`): funcionalidade, melhoria, correção, segurança. Sem label, pergunte ou use "outros".

### 4. Escrever

**Changelog técnico:** uma linha por mudança, com o identificador da issue e o PR.

```markdown
## v0.4.0 — <data>
### Funcionalidades
- <resumo> (GOE-123, #45)
### Correções
- <resumo> (GOE-130, #48)
```

**Release notes para o cliente:** linguagem de negócio, sem identificadores internos, sem nome de agente, só o que o cliente percebe. Se o release é para um cliente específico, **inclua só o que é dele** (filtre por projeto).

Sugira a versão pelo conteúdo (nova funcionalidade: menor; só correção: patch; quebra de compatibilidade: maior), e deixe a decisão com o humano.

### 5. Checklist pré-deploy

- Migrations e a ordem delas.
- Variáveis de ambiente novas.
- Flags e a ordem de deploy entre serviços (por exemplo, backend antes da web).
- Rollback possível?
- Issue concluída que depende de algo que **não** está neste release: bloqueie e avise.

Entregue tudo como **rascunho**, na conversa. Se a pessoa pedir, grave o `CHANGELOG.md` do repositório por PR.

### 6. Depois do deploy

Só quando o humano disser que o deploy foi feito: comente em cada issue do release, assinado, a versão em que ela saiu (`save_comment`).

## Não faça

- Fazer o deploy, criar tag ou publicar as notas sem revisão.
- Incluir informação interna ou de outro cliente nas notas.
- Mover issues de status.
