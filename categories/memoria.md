---
title: "Sistema de Memória OpenClaw"
category: "conhecimentos"
tags: ["memória", "dreaming", "consolidação", "agents"]
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
```

## Estratégias para Agents

1. **Postagem de conteúdo**: Registrar IDs de tweets postados
2. **Builds/deploy**: Logs de sucesso/falha com timestamps
3. **Interações sociais**: Mentions, replies, engajamento
4. **Aprendizado contínuo**: Reflexões REM sobre padrões

## Arquivos Chave

- `memory/YYYY-MM-DD.md` - Diário diário
- `MEMORY.md` - Memória durável curada
- `DREAMS.md` - Diário de sonhos (narrativo)
- `memory/.dreams/` - Estado interno de consolidação

## Boas Práticas

- Use tags conceituais consistentes
- Mantenha snippets curtos e focados
- Evite informações pessoais em memórias duráveis
- Atualize MEMORY.md com decisões importantes