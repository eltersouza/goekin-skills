---
name: goekin-retro
description: Conduz a retrospectiva de um ciclo do GoEkin com dados (planejado contra entregue, escopo adicionado, estimativa contra real, reprovações de QA, bloqueios) e transforma os aprendizados em ações. Use no fim do ciclo, ou quando pedirem "faz a retro da sprint" ou "o que aprendemos?".
---

# goekin-retro: retrospectiva com dados

A retro olha o **processo**, não as pessoas. Segue a [`goekin-workflow`](../goekin-workflow/SKILL.md). As ações viram issues em **Backlog**, depois do ok do humano.

## Passo a passo

### 1. Ler

- `get_cycle` do ciclo encerrado (ou do atual, se a pessoa quiser uma retro no meio) e `list_issues` do ciclo.
- `list_comments` e `list_activity` para achar padrões: bloqueios repetidos, mesmos tipos de bug, issues que voltaram da revisão.
- Os vereditos de QA nos comentários (`goekin-qa`).

### 2. Calcular

| Métrica | Como |
|---|---|
| Entregue contra planejado | Concluídas no ciclo sobre o que estava no início |
| Escopo adicionado | O que entrou depois do início |
| Estimativa contra real | Estimativa e tempo entre começar e concluir, por issue; destaque as maiores diferenças |
| Retrabalho | Reprovações de QA e issues de correção abertas |
| Bloqueios | Quantos e por quê |

Diga os limites dos números: o GoEkin ainda não guarda o histórico do ciclo, então o cálculo é aproximado.

### 3. Propor

```markdown
**Retrospectiva — <ciclo> (<datas>)**

Números: <3 ou 4 linhas>

O que funcionou
- <dado ou fato que sustenta>

O que mudar
- <padrão visto, com as issues que o mostram>

Ações propostas
1. <ação concreta> — responsável sugerido: <pessoa>
```

Pergunte ao time o que os dados **não** mostram (como foi o ritmo, o que atrapalhou, o que não ficou registrado). A retro não termina antes de ouvir.

### 4. Ações viram issues

Cada ação aceita vira uma issue em **Backlog**, com responsável sugerido na descrição, no mesmo time. Pergunte o projeto e o milestone, como no workflow. **Ação que ninguém pega não é ação:** se não houver responsável sugerido, diga.

Decisões do time (regras novas) são registradas como decisão, com data, no lugar que o time escolher.

### 5. Registrar

Pergunte onde fica a retro. O GoEkin ainda não tem documentos próprios. Padrão: um comentário assinado na issue que a pessoa indicar (por exemplo, o épico do ciclo), com o texto acima.

### 6. Aprendizados

Os aprendizados úteis para outras skills (por exemplo, "UI leva o dobro do estimado") ficam registrados na retro. Quem usa a [`goekin-estimate`](../goekin-estimate/SKILL.md) pode citá-los. Não guarde nada fora do GoEkin sem pedir.

## Não faça

- Apontar culpados. Fale de processo.
- Criar issues sem ok, ou fora do Backlog.
- Encerrar o ciclo (`complete_cycle`): é do humano.
