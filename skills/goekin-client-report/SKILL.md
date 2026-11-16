---
name: goekin-client-report
description: Escreve o relatório de status de um projeto do GoEkin para o cliente, em linguagem de negócio e sem detalhes internos, como rascunho para revisão humana antes do envio. Use quando pedirem "relatório do cliente X", "status semanal do projeto" ou "quando fica pronto?".
---

# goekin-client-report: status para o cliente

A skill escreve o **rascunho**. **Nada vai para o cliente sem revisão humana.** Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md) e não muda status de issue.

## Passo a passo

### 1. Definir o projeto e o período

Qual projeto (`list_projects`) e qual período (semana, quinzena, mês). Se o relatório é de um cliente com vários projetos, confirme quais entram. **Use apenas dados do projeto pedido.**

### 2. Ler

- `get_project` e `list_milestones` (com `get_milestone`): metas, datas e progresso.
- `list_issues` com o `project`: o que foi **entregue** no período, o que está **em andamento** e o que vem **a seguir**.
- Datas alvo: o GoEkin tem o histórico de mudança de data pela API, mas ele ainda não está no MCP. Se a data mudou, **pergunte ao humano** o motivo, ou use o que estiver registrado nos comentários. Não invente.

### 3. Escrever em linguagem de cliente

Estrutura:

```markdown
# Status do projeto <nome> — <período>

## Resumo
<2 ou 3 frases: onde estamos, se o prazo se mantém>

## Entregue
- <o que o cliente consegue fazer agora, em palavras dele>

## Em andamento
- <o que está sendo feito e quando deve ficar pronto>

## Próximos passos
- <o que vem, com a data prevista>

## Riscos e decisões
- <o que pode afetar o prazo, e o que precisamos do cliente>
```

Regras de escrita:

- **Sem identificadores internos** (GOE-123), sem nome de agente, sem jargão técnico.
- **Sem informação interna:** custo, problemas de outro cliente, discussões do time.
- Atraso: diga a nova data e o motivo, com franqueza, e o que será feito.
- Não invente data. Sem base, escreva "a confirmar" e avise o humano.
- Não inclua horas gastas: o GoEkin ainda não controla tempo.

### 4. Entregar para revisão

Entregue o texto na conversa, marcado **RASCUNHO — revisar antes de enviar**, com uma lista curta do que o humano deve conferir (datas, pontos sensíveis, afirmações que dependem de contexto que a skill não tem).

Se a pessoa pedir para guardar o rascunho, grave onde ela indicar (por exemplo, um comentário numa issue interna). O envio ao cliente é **sempre** dela, e só depois do ok dela a skill pode ajustar a versão final.

## Não faça

- Enviar o relatório.
- Vazar informação interna ou de outro cliente.
- Prometer data que o projeto não sustenta.
