---
name: goekin-triage-errors
description: Transforma erros de produção (Sentry, logs, alertas) em bugs bem descritos no GoEkin, agrupados e sem duplicar, sempre em Backlog. Use quando pedirem "triagem dos erros de ontem", "abre bug desse erro do Sentry" ou quando um alerta chegar.
---

# goekin-triage-errors: de erro de produção para bug

Esta é uma skill de **entrada**. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md): todo bug criado entra em **Backlog**, sem ciclo, e o contexto (time, projeto) é perguntado antes de criar.

## O que você precisa

- Acesso ao erro na ferramenta de origem (o MCP ou a API do Sentry, ou os logs colados pela pessoa). Esta skill **só lê** o Sentry. Se não houver acesso, peça o evento colado.
- O repositório, quando for útil para localizar o ponto do erro.

## Passo a passo

### 1. Ler e agrupar

Liste os erros do período pedido. Agrupe os que têm a mesma causa (mesmo tipo, mesma origem na stack). **Um grupo vira no máximo uma issue**, nunca um bug por evento.

Pergunte o limiar quando não estiver claro (por exemplo, "só erros com mais de 10 ocorrências ou que afetem mais de 3 usuários"). Sem resposta, comece pelos erros novos e pelos mais frequentes e diga o corte usado.

### 2. Mascarar

Antes de colar qualquer coisa: remova e-mail, nome, CPF, telefone, token, cabeçalhos de autorização e corpo de requisição. Cole só a stack resumida.

### 3. Procurar issue existente

`list_issues` com `query` (a mensagem do erro ou o link do Sentry), incluindo as concluídas (`includeArchived` só se precisar).

- **Existe e está aberta:** comente com `save_comment` a nova ocorrência, a frequência e o release.
- **Existe e foi concluída (regressão):** crie um bug novo em Backlog, **relacionado** (`relatedTo`) à antiga, explicando que o erro voltou. Não reabra a antiga: reabrir é decisão humana, e você só sugere.
- **Não existe:** crie o bug.

### 4. Criar o bug

Em Backlog, label de bug (`list_issue_labels`), com:

- Título: o sintoma, com o local (`TypeError no checkout ao aplicar cupom`).
- Descrição: link do Sentry, stack resumida, release e ambiente, primeira e última ocorrência, contagem, usuários afetados (número, não identidade), passos prováveis de reprodução e o impacto.
- Prioridade **sugerida** pela frequência e pelo impacto. Se parecer crítico (pagamento, perda de dados, fora do ar), **avise o humano na hora**, na conversa. Urgente continua sendo só sugestão.

Pergunte o contexto antes: time e projeto. Sem resposta, crie sem projeto e diga no comentário.

### 5. Opcional

Se a pessoa quiser, sugira a [`goekin-plan-issue`](../goekin-plan-issue/SKILL.md) para o diagnóstico no código.

### 6. Resumo

Devolva: bugs criados, comentários em issues existentes, o que ficou abaixo do corte e o que parece crítico.

## Não faça

- Marcar o erro como resolvido no Sentry. Isso é do humano.
- Mover o bug para a fazer, ciclo ou concluída.
- Criar um bug por evento.
