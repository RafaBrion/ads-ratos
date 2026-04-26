# Métricas Meta Ads 2025-2026 (painel e fluxo de leitura)

Referência enxuta sobre quais métricas olhar no Gerenciador de Anúncios da Meta em 2025-2026 pós-Andromeda. Foco em decisão rápida, não em teoria.

**Carregar antes de:** diagnóstico de conta, relatório de performance, auditoria, análise de anúncio individual, troubleshooting de campanha.

**Complementa:** `andromeda-playbook.md` (contexto estratégico) e `benchmarks-br.md` (benchmarks por nicho).

---

## 1. Tabela de benchmarks Brasil 2025-2026

| Métrica | Ruim | Média | Bom | Topo |
|---|---|---|---|---|
| **CPM prospecting** | R$ 40+ | R$ 20-30 | R$ 14-20 | < R$ 14 |
| **Frequência 7d** | 3,5+ | 2,5-3,0 | 1,8-2,4 | 1,5-1,8 |
| **Hook rate** (3s plays ÷ impressões) | < 20% | 25-30% | 32-40% | 45%+ |
| **Hold rate** (thruplays ÷ 3s plays) | < 8% | 12-18% | 20-28% | 30%+ |
| **CTR link** | < 0,8% | 1,2-1,8% | 2,0-3,0% | 3,5%+ |
| **CPC link** | R$ 4+ | R$ 2-3 | R$ 1-1,8 | < R$ 1 |
| **Custo por LPV** | R$ 6+ | R$ 3-4,5 | R$ 1,8-2,8 | < R$ 1,5 |
| **LPV ÷ link click** | < 60% | 70-80% | 85-90% | 95%+ |
| **Conversion rate LPV → lead** | < 1% | 1,5-2,5% | 3-5% | 6%+ |
| **EMQ CAPI** | < 5 | 6-7 | 8 | 9+ |
| **ROAS infoproduto** | < 1,5 | 2,0-2,8 | 3,5-5,0 | 6+ |

Para benchmarks específicos por nicho (saúde, moda, infoproduto, local), consultar `benchmarks-br.md`.

---

## 2. Painel mínimo de colunas no Gerenciador

Customizar as colunas do Gerenciador nessa ordem exata:

1. Nome (campanha / conjunto / anúncio)
2. Entrega (status)
3. Valor usado
4. Impressões
5. CPM
6. Frequência
7. **Hook rate** (métrica custom: `video_plays_3s ÷ impressions`)
8. **Hold rate** (métrica custom: `video_thruplays ÷ video_plays_3s`)
9. CTR (link click through rate)
10. CPC (custo por link click)
11. Landing page views
12. Custo por LPV
13. Resultados (o objetivo da campanha)
14. Custo por resultado
15. ROAS de compra (quando e-commerce)
16. Ad Relevance Diagnostic (Quality + Engagement + Conversion Rate Ranking)

Salvar como preset de colunas no Gerenciador (botão "Colunas: Personalizar > Salvar como preset").

**Hook rate e Hold rate não existem nativas no Gerenciador.** Criar como métricas customizadas em "Métricas personalizadas":
- **Hook rate:** `Reproduções de 3 segundos do vídeo ÷ Impressões × 100`
- **Hold rate:** `ThruPlays ÷ Reproduções de 3 segundos × 100`

---

## 3. Fluxo diário de leitura (15 minutos)

1. Abrir Gerenciador, visão **últimos 3 dias**, nível **campanha**. Olhar **custo por resultado** e **ROAS**. Campanhas fora da meta vão pro próximo passo.
2. Descer pra **ad set** da campanha problemática. Olhar **frequência** e **CPM**. Frequência > 3 e CPM subindo = fadiga.
3. Descer pro **anúncio**. Olhar na ordem:
   - **Hook rate** (primeiro)
   - **Hold rate** (segundo)
   - **CTR link** (terceiro)
4. Diagnosticar pelo padrão:
   - **Hook ruim:** trocar capa e 3 primeiros segundos
   - **Hold ruim:** refazer o meio do vídeo (segundo 3 ao 10)
   - **CTR ruim com hook e hold bons:** problema de oferta ou CTA, não de vídeo
5. Verificar **Ad Relevance Diagnostic**. Só agir quando tem 2+ eixos "below average" ao mesmo tempo. 1 eixo abaixo é normal.
6. **EMQ do CAPI** checar 1x por semana no Events Manager, não diário.

---

## 4. O que cada métrica realmente significa

### 4.1 Topo de funil (entrega e atenção)

**Impressões e alcance isolados não dizem nada.** Servem só como denominador.

**Frequência** é o que importa:
- Prospecting frio: 1,8 a 2,5 (saudável). Acima de 3 em 7 dias = saturação.
- Retarget quente (30 dias): 4 a 7 é aceitável. Acima de 10 irrita.
- Retarget morno (180 dias): teto em 3,5.

**CPM** lido em contexto:
- CPM subindo **sem** queda de CTR = competição de leilão (sazonalidade, BF, eleição). Não é problema do criativo.
- CPM subindo **com** CTR caindo = criativo morrendo. Refresh urgente.

**Hook rate** é o termômetro número 1 de criativo em vídeo. Se o hook tá ruim, nada mais importa (ninguém viu o anúncio de verdade).

**Hold rate** responde "o meio do vídeo segura depois do gancho?". Se hook é bom mas hold é ruim, o problema está no segundo 3 ao 10.

**Thumbstop ratio** é sinônimo de hook rate em ferramentas como Motion e Triple Whale. Mesma fórmula.

### 4.2 Engajamento e clique

**CTR link ≠ CTR all.** CTR link é o que importa. CTR all inclui cliques em perfil, expandir legenda, reações, métricas vaidade.

**Outbound click vs link click:**
- **Outbound click:** clique que sai do Meta (vai pra domínio externo). É o mais honesto pra tráfego.
- **Link click:** inclui cliques dentro do próprio Meta (forms nativos, Shop).
- Pra tráfego pro site, olhar **outbound**.

**Landing page views (LPV):** número mais honesto de tráfego real.

**LPV ÷ link click** (taxa de carregamento):
- < 60%: LP lenta ou quebrada no mobile. Rodar PageSpeed, cortar tudo acima de 3 segundos.
- 70-80%: normal
- 85-95%: ótimo

### 4.3 Conversão

**CPA (custo por resultado)** é a métrica final.
- Regra: **CPA máximo = 30 a 40% do LTV de 90 dias.**
- Não olhar só o ticket, olhar o LTV.

**ROAS em 2026 tem dois universos:**
- **ROAS atribuído Meta** (Gerenciador): inflado pela modelagem pós-iOS (infla 20 a 45%)
- **ROAS real** via CAPI + UTM + ferramenta de atribuição: o que paga a conta
- **Usar Meta pra otimizar criativo, usar atribuição real pra decidir budget.**

**Conversion rate LPV → compra** (e-commerce):
- < 1%: ruim
- 1,5-2,5%: médio
- 3-5%: bom
- 6%+: topo

**Events Match Quality (EMQ) no CAPI:**
- 0-5: quebrado, Andromeda não matcheia bem, performance cai
- 6-7: bom
- 8-9+: topo
- Pra subir: enviar email, telefone, fbp, fbc, user agent, IP, external_id

**Resultados vs conversões no Gerenciador:**
- **Resultados:** o evento que você escolheu como objetivo da campanha
- **Conversões:** qualquer evento trackeado no pixel
- Numa campanha de lead, o "resultado" é o lead. "Conversões" mostra compras como métrica secundária.

### 4.4 Qualidade e diagnóstico

**Ad Relevance Diagnostic** (3 eixos):
- **Quality Ranking:** quanto o criativo segura atenção
- **Engagement Rate Ranking:** cliques, reações, comentários
- **Conversion Rate Ranking:** conversão pós-clique

**Regra de leitura:**
- 1 eixo "below average" sozinho = normal, não mexer
- 2+ eixos "below average" juntos = agir
- Quality baixo = problema de criativo
- Conversion Rate baixo = problema de LP ou oferta
- Engagement baixo = problema de hook ou público

**Delivery status:**
- **Learning:** campanha nova, não mexer por 7 dias ou 50 conversões
- **Learning Limited:** não bateu 50 conversões em 7 dias. Consolidar conjuntos, subir budget, ou otimizar em evento mais alto no funil
- **Active:** otimização estável

**Auction overlap:** quando 2+ ad sets seus competem no mesmo leilão. Acima de 30% de overlap = consolidar.

### 4.5 Métricas por formato

**Vídeo:**
- **ThruPlay** (15s ou final): métrica de retenção
- **Average video play time:** meta > 8s em Reels
- **Video plays 25/50/75/100%:** curva de drop-off. Queda entre 25 e 50% é onde o meio do vídeo falha

**Carrossel:**
- **Card views por card** (breakdown): o card 1 carrega 60-70% das impressões
- Se o card 3 tem mais clique proporcional, ele deveria ser o card 1

**Estático:** hook rate não existe (sem vídeo). Sinal é impressão, CTR link e custo por LPV.

---

## 5. Métricas pra IGNORAR (ruído e vaidade)

Pular todas essas colunas:

- **CTR all:** mistura clique vaidade com clique útil. Usar só CTR link.
- **Reach isolado:** sem frequência e CPM, não diz nada.
- **Engagement geral** (reações + comentários + compartilhamentos somados): vaidade pura, não correlaciona com venda.
- **Video plays** (sem qualificar 3s): Meta conta play de 1 segundo, inflado.
- **Post reactions** isolado: vaidade.
- **Page likes:** vaidade absoluta.
- **Cost per 1000 accounts reached:** derivado de CPM, redundante.
- **Quality Score isolado** (sem os outros 2 rankings do Ad Relevance).

**Regra:** se a métrica não responde uma pergunta de decisão ("devo pausar? devo escalar? devo refazer?"), é ruído. Tira do painel.

---

## 6. Breakdowns (quando usar)

Breakdown = segmentar os dados por alguma variável.

| Breakdown | Quando usar |
|---|---|
| **Posicionamento** | Quando suspeitar que Reels está comendo tudo ou que Feed morreu |
| **Idade** | Depois de 100+ conversões, pra decidir lookalike ou refinar criativo |
| **Gênero** | Idem idade, só quando a conta tem volume |
| **Região** | Em campanha nacional com performance desigual por estado |
| **Dispositivo** | Só quando problema de LP no mobile/desktop |
| **Hora do dia** | Raramente. Só quando suspeita de dayparting |

**Nunca** usar breakdown como padrão de análise. Só em troubleshooting específico.

---

## 7. Janelas de atribuição

**Padrão recomendado em 2025-2026:** 7-day click + 1-day view.

- **7-day click:** captura quem clicou e comprou em até 7 dias
- **1-day view:** captura quem viu (sem clicar) e comprou no mesmo dia
- Mais amplo que isso (28-day click, 7-day view) infla demais pós-iOS
- Mais restrito (1-day click) deixa conversão na mesa

**Realidade pós-iOS:** Meta subestima 20 a 40% das conversões reais mesmo com janela correta. CAPI recupera parte. Ferramenta de atribuição (Triple Whale, Northbeam) fecha a lacuna.

---

## 8. Checklist de setup de painel

- [ ] Preset de colunas criado e salvo no Gerenciador
- [ ] Hook rate e Hold rate criadas como métricas customizadas
- [ ] Janela de atribuição ajustada pra 7d click + 1d view
- [ ] CAPI rodando com EMQ ≥ 6 verificado no Events Manager
- [ ] Colunas de vaidade removidas (CTR all, reach isolado, page likes, video plays)
- [ ] Filtros salvos pra "learning limited" e "frequência > 3"
