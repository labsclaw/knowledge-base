---
title: "Sistema de Memória OpenClaw"
category: "conhecimentos"
tags: ["memória", "dreaming", "consolidação", "agents", "autoimprove"]
---

# Sistema de Memória OpenClaw para Agents

## Visão Geral

O sistema de memória do OpenClaw opera em três camadas:
1. **Curto prazo** - Sinais recém-gerados, limpeza frequente
2. **Dreaming** - Consolidação automática em 3 fases
3. **Longo prazo** - MEMORY.md e DREAMS.md

## Dreaming - Consolidação de Memória

### Três Fases

| Fase | Propósito | Saída |
|------|-----------|-------|
| Leve | Classificar e preparar material | Sinais de reforço |
| Profunda | Promover candidatos duráveis | MEMORY.md |
| REM | Refletir sobre padrões | DREAMS.md |

### Sinais de Ranqueamento (pesos)

1. **Frequência** (0.24) - Quantas vezes apareceu
2. **Relevância** (0.30) - Qualidade da recuperação
3. **Diversidade de consulta** (0.15) - Contextos distintos
4. **Recência** (0.15) - Pontuação de atualidade
5. **Consolidação** (0.10) - Recorrência em dias
6. **Riqueza conceitual** (0.06) - Densidade de tags

### Configuração

```json
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true,
            "frequency": "0 3 * * *",
            "timezone": "America/Sao_Paulo"
          }
        }
      }
    }
  }
}
```

## Integração para Agents

### Logging de Atividades

Agents devem registrar:
- Ações executadas (postagens, builds, etc.)
- Resultados obtidos
- Tags conceituais relevantes

Exemplo para x-poster:
```
## x-poster Activity
- Posted tweet ID 12345: "Hello world" - success
- Tags: #social-media #x-api
```

### Sinais para Dreaming

Atividades frequentes com alta relevância geram:
- Sinais de reforço nas fases leve/REM
- Candidatos para promoção profunda
- Entradas no Dream Diary

## Autoimprove Integration

Os sistemas de memória e autoimprove são complementares para agents autônomos:

### Loop de Melhoria Contínua

1. **Dreaming** consolida memórias e identifica padrões
2. **Autoimprove** otimiza baseado em metas mensuráveis
3. **Feedback loop**: Resultados de otimização viram novas memórias

### Formato improve.md para Agents

```markdown
# autoimprove: agent-performance

## Change
scope: agents/, skills/
exclude: secrets/, credentials/

## Check
test: npm test
run: node eval/performance.js
score: "SCORE: ([\\d.]+)"
goal: lower
guard: error_rate: ([\\d.]+) < 0.05

## Stop
budget: "24h"
rounds: 50
target: 0.50

## Agent
provider: openai
model: opencode/big-pickle

## Instructions
Optimize agent response time while maintaining quality.
Focus on memory access patterns and tool selection.
```

### Integração com Dreaming

- Experimentos bem-sucedidos são logados em `memory/YYYY-MM-DD.md`
- Melhorias de performance viram sinais de "consolidação"
- Padrões de otimização são refletidos na fase REM

### Gatilhos para Autoimprove

Use quando:
- Agent quer otimizar performance mensurável
- Identifica gargalos repetidos nas memórias
- Quer melhorar qualidade de respostas
- Precisa reduzir custo de operações

## Comandos CLI

```bash
# Verificar status
openclaw memory status

# Promover manualmente
openclaw memory promote --apply

# Verificar candidatos
openclaw memory promote-explain "query"

# Dream Diary
openclaw memory rem-harness
openclaw memory rem-backfill

# Autoimprove
/autoimprove
/autoimprove init
/autoimprove eval-init
```

## Estratégias para Agents

1. **Postagem de conteúdo**: Registrar IDs de tweets postados
2. **Builds/deploy**: Logs de sucesso/falha com timestamps
3. **Interações sociais**: Mentions, replies, engajamento
4. **Autoimprove**: Otimização contínua de performance
5. **Aprendizado contínuo**: Reflexões REM sobre padrões

## Arquivos Chave

- `memory/YYYY-MM-DD.md` - Diário diário
- `MEMORY.md` - Memória durável curada
- `DREAMS.md` - Diário de sonhos (narrativo)
- `memory/.dreams/` - Estado interno de consolidação
- `improve.md` - Configuração para autoimprove

## Boas Práticas

- Use tags conceituais consistentes
- Mantenha snippets curtos e focados
- Evite informações pessoais em memórias duráveis
- Atualize MEMORY.md com decisões importantes
- Integre autoimprove para otimização contínua