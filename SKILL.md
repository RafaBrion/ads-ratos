---
name: ads-ratos
description: Inteligência de tráfego pago — diagnóstico, relatório, auditoria e estratégia para Meta Ads e Google Ads. Usa benchmarks brasileiros, Quality Gates e Health Score. Use quando o usuário mencionar diagnóstico de conta, relatório de ads, auditoria de tráfego, performance de campanha, análise de conta, health score, benchmark, quality gate, dashboard de ads. Também dispara com /ads-ratos.
---

# Ads Ratos

Camada de inteligência para gestão de tráfego pago. Diagnostica contas,
gera relatórios visuais, audita campanhas e aplica Quality Gates com
benchmarks do mercado brasileiro.

Não executa ações na API diretamente — delega para skills de execução:
- **Meta Ads**: skill `meta-ads-ratos` (SDK oficial facebook-business)
- **Google Ads**: skill `google-ads-ratos` (SDK oficial google-ads)
- **Google Analytics**: skill `ga4-ratos` (GA4 Data API)

Se a skill de execução não estiver instalada, orientar o usuário a instalar.

## Setup

Na primeira vez, rodar:
```
/ads-ratos setup
```

O setup:
1. Detecta se o CC-OS RATOS está instalado (lê `_contexto/empresa.md` se existir)
2. Verifica quais skills de execução estão disponíveis (meta-ads-ratos, google-ads-ratos, ga4-ratos)
3. Cadastra contas do cliente no `contas.yaml`
4. Testa conexões

## Comandos

| Comando | O que faz | Quando usar |
|---|---|---|
| `/ads-ratos setup` | Configura contas e testa conexões | Primeira vez |
| `/ads-ratos diagnostico` | Health Score + KPIs + alertas automáticos | Check diário (5 min) |
| `/ads-ratos relatorio` | Dashboard HTML com benchmarks BR | Entrega pro cliente (semanal/mensal) |
| `/ads-ratos auditoria` | Análise profunda com Quality Gates | Revisão mensal |
| `/ads-ratos historico` | Registra e consulta otimizações e hipóteses | Após cada ação |

## Cadastro de contas (contas.yaml)

**Arquivo:** `contas.yaml` (na raiz da skill)

Antes de executar qualquer comando, o Claude DEVE ler este arquivo para resolver
nomes de clientes para IDs de conta.

Se não houver contas cadastradas, guiar o setup.

## Referências (carregar sob demanda)

| Arquivo | Quando carregar |
|---|---|
| `references/benchmarks-br.md` | Diagnóstico, relatório e auditoria |
| `references/quality-gates.md` | Auditoria e diagnóstico |

O Claude DEVE ler o arquivo de referência relevante ANTES de executar o comando.

## Regras gerais

1. **NUNCA usar MCPs**: toda execução DEVE ser via scripts Python das skills Ratos (meta-ads-ratos, google-ads-ratos, ga4-ratos). Nunca usar fb-ads-mcp-server, adloop ou qualquer outro MCP de terceiro. Isso garante consistência e independência.
2. **Benchmarks BR**: sempre usar benchmarks do mercado brasileiro (não americano)
2. **Terminologia PT-BR**: nunca usar termos em inglês no output (spend → gasto, reach → alcance, etc)
3. **Números sempre**: alertas e recomendações devem ter números específicos, nunca vagos
4. **Comparativo**: sempre comparar com período anterior quando possível
5. **Priorizar**: ordenar alertas e recomendações por impacto financeiro (maior economia primeiro)

## Detecção de skills de execução

Antes de executar, verificar quais skills estão disponíveis:

```bash
# Meta Ads
ls ~/.claude/skills/meta-ads-ratos/SKILL.md 2>/dev/null && echo "META_OK"

# Google Ads
ls ~/.claude/skills/google-ads-ratos/SKILL.md 2>/dev/null && echo "GOOGLE_OK"

# GA4
ls ~/.claude/skills/ga4-ratos/SKILL.md 2>/dev/null && echo "GA4_OK"
```

Se nenhuma skill estiver instalada, orientar:
- Meta Ads: `git clone https://github.com/duduesh/meta-ads-ratos ~/.claude/skills/meta-ads-ratos`
- Google Ads: (em breve)
- GA4: (em breve)

## Tabela de terminologia PT-BR

| Inglês | Português |
|---|---|
| spend | gasto |
| reach | alcance |
| impressions | impressões |
| clicks | cliques |
| conversions | conversões |
| cost per lead | custo por lead (CPL) |
| click-through rate | taxa de cliques (CTR) |
| cost per click | custo por clique (CPC) |
| cost per mille | custo por mil (CPM) |
| frequency | frequência |
| return on ad spend | retorno sobre investimento (ROAS) |
| budget | orçamento |
| ad set | conjunto de anúncios |
| ad creative | criativo |
| landing page | página de destino |
| conversion rate | taxa de conversão |
| quality score | índice de qualidade |
| search terms | termos de busca |
| negative keywords | palavras-chave negativas |
| audience | público |
| placement | posicionamento |
| daily budget | orçamento diário |
| lifetime budget | orçamento vitalício |
