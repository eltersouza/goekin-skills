---
name: goekin-standup
description: Gera o standup ou resumo diário de um time do GoEkin a partir do que realmente mudou (concluído, em andamento, bloqueado), com links para as issues. Use quando pedirem "o que rolou ontem?", "standup de hoje", "resumo do período" ou em rotina diária.
---

# goekin-standup: resumo do que andou

Ninguém precisa escrever status à mão: o resumo sai do que mudou no GoEkin. A skill só **lê**. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md) e não muda status de issue.

## Passo a passo

### 1. Definir o período e o recorte

Padrão: desde o último dia útil. Se a pessoa pedir ("os 3 dias que fiquei fora"), use o que ela disse. Pergunte (ou leia) o **recorte**: time, projeto ou cliente. **Respeite o filtro**: um resumo para o cliente A não menciona o cliente B.

### 2. Ler

- `list_activity` do período, para o que mudou.
- `list_issues` com `updatedAt` desde o início do período, do recorte.
- `list_comments` das issues **bloqueadas**, para explicar o bloqueio com a palavra de quem o registrou.
- `list_users` para os nomes, quando precisar.

### 3. Agrupar

Por pessoa ou agente, em três grupos: **concluiu**, **está fazendo** e **bloqueado**. Entre os bloqueios, diga o que falta e de quem depende. Destaque o que **entrou em revisão** e está esperando alguém.

### 4. Escrever curto

```markdown
**Resumo de <data ou período> — <time ou projeto>**

**Concluído**
- GOE-12 <título> (<quem>)

**Em andamento**
- GOE-15 <título> (<quem>) — <uma frase se houver novidade>

**Em revisão, esperando**
- GOE-18 <título> — PR aberto desde <dia>

**Bloqueado**
- GOE-20 <título> — falta <o quê>, depende de <quem ou o quê>
```

Poucas linhas por pessoa. Sem atividade em um grupo, omita o grupo.

### 5. Entregar

Entregue na conversa. Postar em outro lugar (comentário numa issue, canal externo) **só se a pessoa pedir e indicar o destino**. Se houver issue sem movimento que mereça cobrança, **sugira** o comentário, um por issue por semana, no máximo.

## Não faça

- Postar em canal externo sem permissão.
- Misturar clientes ou projetos num resumo filtrado.
- Resumo longo que ninguém lê.
