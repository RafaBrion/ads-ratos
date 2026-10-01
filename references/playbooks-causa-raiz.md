# Playbooks de Causa-Raiz — Google Ads

Portado de `google-ads-api-account-diagnostics` (repo [google/skills](https://github.com/google/skills/tree/main/skills/ads/google-ads-api-account-diagnostics)),
adaptado pra rodar via SDK Python já configurado em `google-ads-ratos-main`
(sem MCP server — usa o mesmo `run_query(customer_id, query)` de `scripts/lib`).

Testado contra `google-ads` SDK v30.0.0. Se os campos abaixo começarem a falhar,
checar primeiro a versão do SDK antes de assumir bug de conta.

Data de portagem: 2026-09-19.

---

## Quando usar

Estes playbooks respondem "**por quê**" caiu — complementam o Health Score e as
regras de `quality-gates.md`, que dizem "**o quê**" fazer. Rodar um playbook
sempre que um Kill Rule ou uma queda no dashboard disparar, ANTES de decidir a ação.

Não usar pra configurar campanha nova, subir conversão offline do zero, ou
qualquer coisa de setup — só diagnóstico de queda.

---

## 1. Queda de conversão (volume ou valor)

**Sintoma**: conversões ou valor de conversão caíram de repente.

**Query** (via `run_query`, resource `campaign`):
```sql
SELECT
  campaign.name,
  metrics.conversions,
  metrics.conversions_value,
  metrics.cost_micros,
  segments.date,
  segments.device,
  segments.conversion_action
FROM campaign
WHERE segments.date >= '{start_date}' AND segments.date <= '{end_date}'
```
*Gotcha*: `metrics.cost_micros` vem em micros — dividir por 1.000.000 pra virar R$.

**Leitura**: comparar o período de queda com o período anterior equivalente.
Isolar por `segments.device` e `segments.conversion_action` — a queda é geral
ou concentrada num device/ação específica?

**Se usa conversão offline** (store-purchase, venda por telefone), checar o
pipeline antes de culpar a campanha:
```sql
SELECT
  offline_conversion_upload_conversion_action_summary.conversion_action_name,
  offline_conversion_upload_conversion_action_summary.successful_event_count,
  offline_conversion_upload_conversion_action_summary.total_event_count,
  offline_conversion_upload_conversion_action_summary.status
FROM offline_conversion_upload_conversion_action_summary
```
Se vier vazio, a conta não tem upload offline configurado — seguir o
diagnóstico sem essa hipótese.

**Ação**: plugar no Kill Rule "Zero conversões" de `quality-gates.md` — só pausar
depois de confirmar aqui se é queda real de tráfego/conversão ou falha de
tracking/upload.

---

## 2. Oportunidade perdida (Impression Share)

**Sintoma**: Impression Share baixo (regra existente em `quality-gates.md`:
"Impression Share < 20% → Orçamento ou bid baixo"). Este playbook detalha
**qual dos dois** é a causa.

**Query**:
```sql
SELECT
  campaign.name,
  metrics.search_impression_share,
  metrics.search_rank_lost_impression_share,
  metrics.search_budget_lost_impression_share
FROM campaign
WHERE segments.date >= '{start_date}' AND segments.date <= '{end_date}'
```
*Gotcha*: os valores vêm como decimal (0.35 = 35%) ou string formatada
(`"< 0.10"`) — tratar os dois casos ao parsear.

**Leitura**:
| Campo alto | Causa | Ação |
|---|---|---|
| `search_budget_lost_impression_share` | Orçamento estourando | Aumentar orçamento (respeitar regra de "Escalar gradual" — nunca +30% de uma vez) |
| `search_rank_lost_impression_share` | Ad Rank ruim (lance ou Quality Score) | Revisar QS antes de subir lance — QS baixo com lance mais alto só queima dinheiro |

Isso substitui o diagnóstico genérico da linha "Impression Share < 20%" do
`quality-gates.md` por uma causa específica e acionável.

---

## 3. Queda de leads / volume

**Sintoma**: "por que meu volume de leads caiu esses dias?"

**Sequência**:
1. Confirmar a queda: conversões por dia, últimos dias vs. período anterior.
2. Isolar se caiu **tráfego** (cliques/impressões) ou **taxa de conversão**
   (conversões/cliques).
3. Se caiu tráfego → rodar o Playbook 2 (Impression Share) pra ver se é
   orçamento, rank, ou queda geral de volume de busca.
4. Se caiu taxa de conversão → quebrar por `segments.device` e
   `segments.conversion_action` pra achar onde está falhando.
5. Checar se alguém mexeu em lance/orçamento/targeting no período:

```sql
SELECT
  change_event.change_date_time,
  change_event.change_resource_name,
  change_event.resource_change_operation,
  change_event.changed_fields
FROM change_event
WHERE change_event.change_date_time >= '{start_date}' AND change_event.change_date_time <= '{end_date}'
LIMIT 10000
```
*Gotcha*: `change_event` exige `LIMIT <= 10000`, só aceita janela de até 30 dias,
e não aceita `metrics.*` — só atributos do próprio `change_event`.

**Ação**: se achar uma mudança manual coincidindo com a queda, é a causa mais
provável — reverter antes de mexer em mais nada.

---

## 4. Pipeline de conversão offline quebrado

**Sintoma**: uma ação de conversão offline (ex: venda por telefone, retirada
na loja) parou de aparecer ou está com taxa de sucesso baixa.

**Query**:
```sql
SELECT
  offline_conversion_upload_conversion_action_summary.conversion_action_name,
  offline_conversion_upload_conversion_action_summary.successful_event_count,
  offline_conversion_upload_conversion_action_summary.total_event_count,
  offline_conversion_upload_conversion_action_summary.status
FROM offline_conversion_upload_conversion_action_summary
```

**Leitura**: comparar `successful_event_count` com `total_event_count` e olhar
`status` pra achar o motivo da falha. Se vier vazio, não existe upload offline
configurado na conta — não é isso.

---

## Notas de portagem

- Removido tudo que dependia de tools do MCP server (`list_accessible_customers`,
  `customer_client` search, `get_resource_metadata`) — o SDK já resolve o
  `customer_id` via `.env`/`contas.yaml` em `google-ads-ratos-main`.
- As queries acima rodam direto com `run_query(customer_id, query)` de
  `google-ads-ratos-main/scripts/lib` — mesmo client, mesmo tratamento de
  resposta que os outros comandos de `read.py`.
- Não criar critério de "conta saudável" separado — os resultados destes
  playbooks alimentam o Health Score e os Kill Rules já existentes em
  `quality-gates.md`, nunca um score paralelo.
