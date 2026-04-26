# Andromeda Playbook (Meta Ads 2025-2026)

Playbook operacional pós-Andromeda pra diagnóstico, auditoria, estratégia e criação de campanha Meta Ads. Este arquivo é a fonte única de verdade estratégica da ads-ratos sobre o novo algoritmo de entrega da Meta.

**Carregar antes de:** diagnóstico de conta, auditoria, relatório estratégico, revisão de estrutura, recomendação de criativo, definição de targeting, revisão de orçamento.

---

## 1. O que é Andromeda (fundação técnica)

Andromeda é o novo motor de retrieval da Meta, anunciado oficialmente em 02/12/2024 no Engineering at Meta e com rollout global ampliado em outubro de 2025. Fica na primeira etapa do pipeline de seleção de anúncios, onde pinça alguns milhares de candidatos a partir de dezenas de milhões.

**Números oficiais da Meta:**
- 10.000x aumento de capacidade do modelo de retrieval vs geração anterior
- +6% recall no retrieval
- +8% qualidade dos anúncios em segmentos selecionados
- 10x eficiência de inferência
- 3x melhoria em QPS end-to-end
- 100x melhoria em latência de feature extraction vs versão CPU

**Hardware:** co-projetado com NVIDIA, rodando em Grace Hopper Superchip + MTIA (Meta Training and Inference Accelerator).

**Posição no pipeline de ads da Meta:**

1. **Retrieval (Andromeda)**: seleciona alguns milhares de candidatos entre dezenas de milhões
2. **Ranking (Lattice)**: ranqueia os candidatos. Lattice trouxe +12% qualidade e até +6% conversões
3. **Central brain (GEM, anunciado 10/11/2025)**: foundation model estilo LLM, +5% conversões no Instagram e +3% no Facebook Feed
4. **Leilão final**: decide qual anúncio entregar pra cada impressão

Advantage+ (Audience, Creative, Shopping) é o motivo funcional de Andromeda existir: multiplica a quantidade de anúncios elegíveis via automação, e Andromeda é quem consegue escolher bem nesse volume inflado.

**Citação oficial (Engineering at Meta, 02/12/2024):**
> "Retrieval é o primeiro passo do nosso sistema multi-estágio de recomendação de anúncios. Essa etapa tem a função de selecionar anúncios a partir de dezenas de milhões de candidatos e reduzir pra alguns milhares de anúncios relevantes."

> "O momentum positivo do pacote Advantage+ aumenta ainda mais o número de anúncios elegíveis através da automação de criação de público, alocação ótima de orçamento, posicionamento dinâmico e geração de criativos."

---

## 2. A inversão lógica que muda tudo

**Antes de Andromeda:** o gestor perguntava "qual público vê esse anúncio?". Targeting fino era alavanca central.

**Depois de Andromeda:** o sistema pergunta "qual anúncio mostrar pra cada pessoa?". Criativo é sinal central, targeting vira sugestão opcional.

**Implicações práticas:**

1. Targeting perdeu 80% da relevância. Criativo ganhou 80%.
2. Consolidação virou regra. Estrutura fragmentada queima verba.
3. Andromeda identifica near-duplicates e trata criativos parecidos como o mesmo anúncio, cortando entrega dos "clones". Diversidade conceitual virou obrigação.
4. CAPI com EMQ acima de 7 virou pré-requisito, sem isso o modelo não consegue matchear bem.
5. Delivery starvation (um criativo come o conjunto inteiro) deixou de ser bug e virou comportamento esperado do algoritmo. O gestor precisa forçar teste justo em estrutura paralela quando quer comparar.

---

## 3. Estrutura de campanha recomendada

Regra mestra: consolidar até doer.

**Por objetivo:**
- 1 a 2 campanhas ativas por objetivo (vendas, leads, tráfego)
- Mais que isso canibaliza leilão (auction overlap) e rouba sinal de aprendizado

**Ad sets por campanha:**
- 1 a 3 conjuntos, máximo
- O padrão antigo de 5 a 10 conjuntos quebrou

**Criativos por ad set:**
- Piso: 8 a 15 anúncios únicos
- Top performers: 20 a 50 por conjunto
- Mínimo de 8 conceitos distintos (ângulo, formato ou mensagem diferente)

**Teste Meta interno (divulgado pela própria Meta):**
> 1 conjunto com 25 criativos entregou +17% conversões a -16% custo vs 5 conjuntos com 5 criativos cada. Advantage+ Creative entregou +22% ROAS.

**Framework ABO + CBO:**
- 1 campanha ABO pra testar (aprender quais criativos funcionam)
- 1 campanha CBO pra escalar (budget no que já provou)
- Critério de promoção: criativo que bate 10+ conversões dentro do CPA/ROAS alvo no ABO migra pro CBO

**Advantage+ Sales Campaign (ASC+) vs manual:**

| Situação | Usar |
|---|---|
| E-commerce com pixel maduro, 50+ conv/semana, CAPI OK | ASC+ |
| Retargeting de fundo de funil com exclusões | Manual |
| Conta nova sem histórico | Manual |
| High-ticket consultivo | Manual |
| Nicho local/regional pequeno | Manual |
| Maioria dos clientes (infoproduto, nutri, personal) | Híbrido: ASC+ topo + manual retarget |

Dado relevante: o nCAC de ASC+ mais que dobrou entre maio/2024 e maio/2025 em alguns verticais de varejo americano. Campanhas manuais de topo de funil ainda são mais baratas pra aquisição fria em nichos pequenos.

---

## 4. Orçamento e escala

**Sair do aprendizado (learning phase):** 50 conversões no evento otimizado em 7 dias. Andromeda não mudou esse piso mas ficou mais sensível a fragmentação.

**Budget mínimo por campanha:**
- Regra: CPA alvo x 50 conversões = budget semanal mínimo
- CPA R$ 20 → R$ 1.000/semana (R$ 143/dia)
- CPA R$ 50 → R$ 2.500/semana (R$ 357/dia)
- CPA R$ 100 → R$ 5.000/semana (R$ 714/dia)

**Budget mínimo por ad set (ABO de teste):**
- Nunca abaixo de R$ 30/dia por conjunto, não dá volume pro Andromeda decidir nada
- Distribuir igual entre os conjuntos em fase de teste

**CBO venceu ABO pra escala.** Andromeda precisa de pool grande de sinal pra decidir quem ganha delivery. ABO continua útil só na fase de teste inicial.

**Como escalar sem quebrar:**
1. Aumento máximo de 20% a cada 3 a 5 dias no mesmo conjunto. Acima disso reseta o aprendizado.
2. Pra escalar mais rápido: duplicar ad set/campanha vencedor em vez de editar o original.
3. Nunca mexer em budget, público e criativo no mesmo dia. Uma variável por vez, 72h de estabilização antes de avaliar.

---

## 5. Leilão e estratégias de bid

| Estratégia | Quando usar | Quando evitar |
|---|---|---|
| **Lowest Cost** (custo mais baixo) | Padrão em 90% dos casos. Contas maduras com CAPI e volume | Nunca evitar, é o default |
| **Cost Cap** | Quando já tem CPA de referência e cliente exige previsibilidade | Conta nova, fase de escala |
| **Bid Cap** | Praticamente aposentado. Só leilão competitivo caro com trava rígida | Maioria dos casos |
| **ROAS Goal** (mínimo ROAS) | E-commerce com CAPI funcionando e 50+ compras/semana. Relatado +22% ROAS e -30% CPA vs controle manual | Conta sem volume, conta sem CAPI maduro |

**Delivery starvation (entrega travada):**
- Sinal: conjunto gasta menos de 80% do orçamento, CPM baixo, impressões despencam
- Causas: targeting restrito demais, cost cap apertado, criativo fadigado, público saturado
- Solução: ampliar targeting, remover cost cap, injetar criativo novo, duplicar conjunto

---

## 6. Targeting

**Broad é padrão em 2026.** Teste Lebesgue: broad +49% ROAS vs lookalike. Dados públicos: broad +17% conversões vs interesse.

Meta consolidou categorias de interesse em grupos amplos em junho/2025 e removeu exclusões de detailed targeting em 31/março/2026. Mesmo que você selecione um interesse, ele já é mais grosseiro do que era, e o algoritmo pode ignorar totalmente.

**Quando interesse fino ainda faz sentido:**
1. Conta nova sem 50 conversões semanais
2. Nicho ultra-vertical sem histórico (B2B de alto ticket)
3. Budget abaixo de R$ 150/dia onde o algoritmo não tem sinal pra aprender

**Fora disso, broad com CBO e 1 a 2 conjuntos é a estrutura que ganha.**

### Advantage+ Audience

Camada de targeting algorítmico. Só localização e idade mínima são restrições duras. Qualquer interesse, CA ou lookalike vira sugestão de seed, não filtro.

**Quando ativar:**
- 50+ conversões semanais
- Budget acima de R$ 250/dia
- E-commerce ou funil com pixel bem alimentado
- Benchmark interno Meta: CPA -32%, CTR +11-15%

**Quando performa pior:**
- Contas zeradas sem histórico de conversão
- Testes de oferta nova sem baseline
- Nichos B2B de alto ticket com pool pequeno de compradores

**Suggestions:** jogar 2 a 3 CAs quentes (compradores, top 25% site visitors, engagement IG 180 dias) como dica pra acelerar o aprendizado, sem limitar alcance.

### Custom Audiences (CAs)

CA virou combustível pro modelo, não público de exibição. Ranking prático de fontes:

1. Customer list de compradores com hash (email + telefone) → qualidade máxima
2. Website CA via pixel + CAPI (Purchase, AddToCart, ViewContent)
3. Engagement IG e FB 365 dias (first-party puro, ficou mais potente pós-Andromeda)
4. Video views 50% e 75%
5. Lead form opens e submits

Tamanho mínimo útil: 1.000 pessoas pra servir, 5.000+ pro algoritmo aprender. Abaixo disso, usar como suggestion dentro de Advantage+ Audience, nunca como público isolado.

### Lookalike Audiences

Ficou secundário. Broad com criativo forte bate lookalike na maioria dos testes de 2025-2026. Advantage+ Audience já faz "lookalike expansion" internamente a partir de qualquer CA, então LAL manual virou redundante.

**Quando ainda faz sentido:** histórico maduro + lista de clientes top LTV + objetivo de escalar além do alcance broad atual.

**Seed ideal:** 1.000 a 5.000 compradores reais (não visitantes genéricos). Qualidade esmaga volume.

**Tamanho:** 1% pra testar, 3% pra escalar. 5% e 10% viraram praticamente broad.

### Retargeting

Continua sendo a etapa mais lucrativa do funil (4x+ ROAS) mas só funciona com CAPI server-side rodando, porque pool de warm encolheu drasticamente pós-iOS.

**Estrutura de funil:**
- **Topo (prospecção):** 1 campanha broad com Advantage+ Audience, CBO, 10 a 20 criativos
- **Meio (aquecimento):** retarget 30 dias de engagement IG + FB + video view 50%
- **Fundo (conversão):** retarget 14 a 30 dias de site visitors + AddToCart + lead form open sem compra

**Janelas:**
- 1 dia: carrinho quente
- 7 dias: view content recente
- 30 dias: retarget padrão
- 180 dias: reativação de quem engajou e sumiu

**Exclusões:** necessárias em retarget (excluir compradores 180 dias). Desnecessárias em broad, atrapalham aprendizado.

### Geografia e demografia

- **Localização:** única trava dura que vale empilhar. Raio 5-10km urbano, 10-25km suburbano, 25-80km rural. Cidade inteira só em mercado com 500k+ habitantes. Usar "people living in", não "recently in".
- **Idade e gênero:** abertos por padrão. Travar só em produto legalmente segmentado (suplemento, estética, infantil) ou quando insight histórico mostrar 80%+ das conversões num recorte.
- **Idioma:** só em país com mistura relevante. Brasil raramente precisa.
- **Dispositivo:** all placements automático. Segmentar só em instalação de app.

---

## 7. Criativos (o campo de batalha real)

Andromeda tornou criativo a alavanca central de performance. Todas as outras decisões (público, budget, lance) são secundárias.

### 7.1 Volume e diversidade

**Padrões 2025-2026:**
- 8 a 15 criativos por conjunto como piso
- 20 a 50 em contas maduras
- 6 a 10 criativos novos/mês em conta pequena (<R$ 20k/mês)
- 8 a 20 criativos novos/mês em conta média (R$ 50k/mês)
- Refresh semanal em conta grande (R$ 100k+/mês)

**Diversidade conceitual vs cosmética:**

| Diversidade cosmética (Andromeda ignora) | Diversidade conceitual (Andromeda premia) |
|---|---|
| Trocar headline | Trocar ângulo (dor vs desejo vs objeção) |
| Trocar fundo ou cor | Trocar formato (UGC vs texto animado vs demo) |
| Trocar música | Trocar prova (depoimento vs antes/depois vs número) |
| Trocar ordem do slide | Trocar persona alvo |

**8 ângulos distintos pra cobrir:**
1. Dor (o que tá incomodando)
2. Desejo (o que a pessoa quer)
3. Objeção (por que ainda não comprou)
4. Autoridade (credibilidade, método, número)
5. Comparação (nós vs alternativas)
6. Antes/depois (transformação)
7. Tendência (o que tá rolando agora)
8. Contra-intuitivo (padrão interrompido)

### 7.2 Formato

- **Vídeo curto vertical domina.** Reels pra prospecting, Feed pra retarget, Stories complementa.
- **Carrossel** voltou a performar em ofertas com múltiplos benefícios, provas antes/depois, lista de produto.
- **Estático puro** sobrevive com copy forte, melhor em retargeting e prova social.
- **Duração:** 15-30s prospecting, 30-60s prova social.
- **Aspect ratio:** 9:16 (1080x1920) obrigatório.
- **Mobile-first, som off por padrão.** Legenda burn-in obrigatória (85% dos usuários começa mudo).
- **Dark post** ganha do orgânico promovido pra isolar teste. Exceção: peças com engajamento orgânico real viram prova social pesada.

### 7.3 Hooks (os 3 primeiros segundos)

Andromeda trata os 3 primeiros segundos como anúncio inteiro. Se 10 criativos têm intro parecida, ele classifica como 1 só. **Variar abertura visual + overlay de texto + fala + trilha, os 4 juntos.**

**Hooks vencedores em 2025-2026:**
1. Investimento perdido: "Gastei R$ 2.400 em academia antes de descobrir isso"
2. Pedido de tempo curto: "Me dá 20 segundos e você vai entender por que..."
3. Palavra gatilho "golpe": "Achei que era golpe até eu testar"
4. Pattern interrupt: "Tá fazendo errado" / "Esquece tudo que te falaram sobre..."
5. Dor específica: "Cansou de acordar inchada todo dia?"
6. Promessa clara no primeiro frame

**Regra prática:** escrever 5 a 10 variações de hook pra cada conceito antes de filmar, não depois.

### 7.4 Copy e CTA

- **Headline:** primeiros 80 a 125 caracteres. Depois do "Ver mais" atende só 1% dos usuários.
- **Copy curta ganha no topo de funil** (hook + oferta + CTA).
- **Copy longa ganha em retargeting e público morno** (história, prova, objeções).
- **CTA direto vence soft CTA** em 2026. "Compre hoje com 40% off", "Agende sua avaliação". Evitar "Saiba mais" genérico.

### 7.5 UGC e prova social

UGC domina em 2025-2026. Casos da Meta apontam UGC superando criativo de estúdio em +35-38% CTR e -25-50% CPA.

**Hierarquia de autenticidade que Andromeda premia:**
1. Depoimento real de cliente filmado com celular (maior peso)
2. Dono/fundadora falando na câmera, sem estúdio
3. Creator pago que entrega no tom nativo
4. UGC atuado por ator
5. Vídeo de produção profissional (pior desempenho em prospecting)

**Prova social dentro do vídeo** (print de comentário, áudio de cliente, número de vendas, zap real) é o elemento que mais levanta hold rate depois do hook.

### 7.6 IA generativa e Advantage+ Creative

Advantage+ Creative é ligado por default e gera variações automáticas (enhancements visuais, legenda, expansão de tela, música, substituição de imagem).

**Ligar quando:** o criativo-base é forte e você quer volume de variação sem esforço extra.

**Desligar substituição de imagem quando:** marca com guideline rígido, depoimento autoral, nicho sensível (saúde, infoproduto sério, pessoa pública).

Controle manual fica em: Ad Account Settings > Advertising Settings > Creative Tools. Opt out por feature, não global.

**Criativo 100% gerado por IA** (Runway, Sora, HeyGen avatar) já está sendo distribuído mas performa pior que UGC real em hook rate. Tendência: usar IA pra B-roll, variação de fundo, locução secundária, e manter rosto humano real no primeiro plano.

### 7.7 Matriz de criativos recomendada (por conjunto consolidado)

| Formato | Quantidade | Objetivo | Exemplo |
|---|---|---|---|
| UGC falado dono/cliente | 4 a 6 | Hook + prova, ângulos distintos | Fundadora falando resultado em 25s |
| Depoimento real bruto | 2 a 3 | Autoridade social | Cliente gravando no celular pós-resultado |
| Estático com copy forte | 2 a 3 | Retargeting, escala | Card antes/depois com headline direta |
| Carrossel benefício | 1 a 2 | Objeção e detalhe | 6 telas: problema, método, prova, oferta, bônus, CTA |
| Demo de produto/serviço | 1 a 2 | Conversão morna | Tela de produto, unboxing, uso |
| Hook pattern interrupt | 2 a 3 | Teste de ângulo novo | "Tá fazendo isso errado" em 10s |
| **Total** | **12 a 19** | **Cobertura Andromeda** | **1 conjunto, 1 CBO** |

---

## 8. CAPI e qualidade de sinal

**CAPI virou pré-requisito, não diferencial.** Sem Conversions API com Event Match Quality (EMQ) acima de 6, Andromeda entrega com sinal ruim e queima verba.

- **Eventos ideais pra otimizar** (ordem de preferência): Purchase > Add to Cart > Lead > ViewContent. Se não bate 50 purchases semanais, otimiza em AddToCart até chegar lá.
- **Pixel + CAPI com deduplicação:** obrigatório. Recupera 20 a 30% de conversões perdidas pelo iOS 14.5+.
- **Conversão padrão vs customizada:** preferir eventos padrão (Purchase, Lead) sempre que possível, Andromeda entende melhor. Custom conversions só quando o evento padrão não descreve bem.
- **Contas com CAPI bem implementado:** +10 a +25% ROAS vs setup só com pixel.

**Checklist mínimo de CAPI:**
- Servidor rodando
- Dedup configurada
- EMQ ≥ 6 (ideal 7-9+)
- Pelo menos 5 parâmetros: email, phone, fbp, fbc, external_id
- 7 dias de dados estáveis antes de julgar

---

## 9. Testes e experimentação

**A/B test nativo da Meta** funciona mas precisa de disciplina:

- **Duração mínima:** 7 dias corridos (captura variação dia da semana)
- **Volume mínimo:** 50 a 100 conversões por variação
- **Budget mínimo:** R$ 100/dia por variação pra atingir significância de 95% em 7-14 dias
- **Duração por tipo:**
  - Criativo: 7-14 dias
  - Público: 14-21 dias
  - Posicionamento: 14 dias
  - Budget/bid: 21-28 dias
- **Isolar variável:** mudar só uma coisa por teste
- **Quando matar:** após 7 dias e 50 conversões, se o teste está 30%+ pior em CPA e a barra de confiança da Meta não cruza 90%, encerra. Nunca matar antes de 3 dias.

**Pra teste de criativo em 2026:** o padrão novo é 1 conjunto consolidado com 8-15 criativos e deixar Andromeda distribuir. A/B test nativo serve pra testes estruturais (posicionamento, público, otimização), não pra criativo.

---

## 10. Checklist operacional

### Diário (15 minutos)

1. Conferir alertas no Gerenciador (rejeições, limites, pixel offline)
2. Gasto e CPA/ROAS de cada campanha nas últimas 24h contra meta
3. EMQ do CAPI (precisa estar ≥ 6)
4. Algum ad set saiu de "Learning" pra "Learning Limited"?

### Semanal

1. Injetar 3 a 5 criativos novos por ad set ativo
2. Pausar criativos com frequência > 4 e CTR < 0,8%
3. Escalar vencedores com +20% a cada 3 dias, sem tocar em mais nada
4. Duplicar ad set vencedor (não editar o original)
5. 1 teste A/B novo por semana (variável isolada, 7 dias mínimo)

### Mensal

1. Auditar estrutura: mais de 2 campanhas ativas por objetivo? Consolidar
2. Ad set com menos de 10 criativos? Injetar
3. Ad set em Learning Limited há +7 dias? Matar ou duplicar
4. CAPI EMQ médio < 6 no mês? Abrir chamado técnico
5. Revisar benchmark por nicho no `benchmarks-br.md`

### Nunca fazer

- Mexer em budget, público e criativo no mesmo dia
- Aumentar budget mais de 20% de uma vez
- Rodar menos de 8 criativos por ad set
- Teste A/B com menos de 7 dias
- Confiar só em pixel sem CAPI
- Usar bid cap em campanha de escala
- Rodar 5+ ad sets com R$ 20/dia cada (fragmentação pura)

---

## 11. Timeline Andromeda

- **19/11/2024**: Meta publica "Sequence learning" abrindo terreno conceitual
- **02/12/2024**: Engineering at Meta lança o post oficial do Andromeda
- **2025**: rollout ampliado integrado ao Advantage+ e aos avanços do GEM
- **Outubro/2025**: rollout global ampliado
- **10/11/2025**: post oficial do GEM posiciona o foundation model de ads como camada superior, com Andromeda mantido como retrieval engine
- **Próximos passos anunciados**: migração pra loss function autoregressiva, entrega de "um conjunto mais diverso de candidatos de anúncio", integração crescente com MTIA e novas GPUs

---

## 12. Fontes

### Material oficial Meta
- Meta Andromeda - Engineering at Meta (02/12/2024)
- AI Innovation in Meta's Ads Ranking - Meta for Business
- Meta Lattice - Meta AI Blog
- Meta GEM - Engineering at Meta (10/11/2025)
- Sequence learning - Engineering at Meta (19/11/2024)
- Meta Business Help - About Advantage+ Audience

### Análises e boas práticas
- Jon Loomer - Meta Andromeda What It Means for Your Ad Strategy
- Jon Loomer - 83 Changes to Meta Advertising in 2025
- Jon Loomer - Guide to Meta Ads Targeting in 2026
- Foxwell Digital - Meta Andromeda Practical Guide
- Foxwell Digital - Creative Diversity The Golden Ticket
- Motion - Andromeda Impact on BFCM
- Lebesgue - Multiple Ads per Ad Set vs One Ad per Ad Set
