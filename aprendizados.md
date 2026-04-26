# Aprendizados — Ads Ratos

Regras gerais aprendidas durante o uso. O Claude DEVE ler este arquivo no início de qualquer comando.
Regras específicas de plataforma ficam no `aprendizados.md` de cada skill de execução.

---

### 2026-04-11, Andromeda é a fundação de qualquer análise Meta Ads 2025-2026

**Regra:** Antes de QUALQUER diagnóstico, auditoria, relatório ou recomendação de estrutura/criativo/targeting em conta Meta Ads, carregar `references/andromeda-playbook.md`. Aplicar regras antigas (fragmentar ad sets, testar muitos públicos, rodar 3-6 criativos por conjunto) leva a diagnósticos errados porque a lógica de entrega da Meta mudou completamente após outubro/2025.
**Contexto:** Caso real em conta de nutricionista (abril/2026). Campanha com 4 criativos por ad set, AD04 pegou momentum e "asfixiou" o AD03 mesmo o AD03 tendo CPA 2,5x melhor. Diagnóstico antigo culparia budget mal distribuído, quando o motivo real é o Andromeda tratando os criativos como near-duplicates e concentrando entrega.

### 2026-04-11, Criativo é a alavanca central, targeting é secundário

**Regra:** Quando a performance da conta não bate meta, o primeiro suspeito é o criativo (volume, diversidade, fadiga), não o público. Diagnosticar nessa ordem: (1) tem 8+ criativos conceitualmente distintos no conjunto? (2) hook rate ≥ 25%? (3) hold rate ≥ 12%? (4) frequência < 3? Só depois olhar targeting e budget.
**Contexto:** Pós-Andromeda, targeting fino virou sugestão opcional. Broad com criativo forte bate lookalike em +49% ROAS (Lebesgue). A pergunta que o algoritmo faz mudou de "quem vê esse anúncio?" pra "qual anúncio mostrar pra essa pessoa?". Aplicar framework antigo de "público quente vs frio" gera recomendações erradas.

### 2026-04-11, Consolidar ad sets, não fragmentar

**Regra:** Em qualquer auditoria, se a conta tem mais de 2 campanhas por objetivo ou mais de 3 ad sets por campanha, a primeira recomendação é consolidar. Meta teste interno: 1 conjunto com 25 criativos = +17% conversões a -16% custo vs 5 conjuntos com 5 criativos cada.
**Contexto:** Estrutura fragmentada fragmenta o sinal de aprendizado, gera auction overlap e impede Andromeda de decidir bem. Recomendação contra-intuitiva pra gestor formado na era "teste de público", mas é o padrão vencedor em 2025-2026.

### 2026-04-11, Benchmarks Brasil antes de julgar performance

**Regra:** Nunca julgar CPA, CPM, CTR ou ROAS sem comparar com benchmark brasileiro do nicho. Consultar `benchmarks-br.md` e `metricas-painel.md` antes de classificar uma métrica como "boa" ou "ruim". Benchmark americano não vale, mercado brasileiro tem CPMs menores e CPAs diferentes por nicho.
**Contexto:** Erro comum é aplicar ranges americanos (Wordstream, Databox global) em conta brasileira e assustar o cliente sem motivo, ou elogiar quando a métrica está na verdade ruim pro nicho.

### 2026-04-11, Hook rate é a métrica de entrada pra diagnosticar criativo

**Regra:** Em análise de anúncio individual em vídeo, olhar hook rate (3s plays ÷ impressões) ANTES de qualquer outra métrica. Abaixo de 20% = criativo invisível, nada mais importa. Só depois olhar hold rate, CTR e CPA. Essa é a ordem correta de troubleshooting.
**Contexto:** Hook rate e hold rate não vêm por padrão no Gerenciador, precisam ser criados como métricas customizadas. Instruções no `metricas-painel.md`.

### 2026-04-11, CAPI com EMQ ≥ 7 é pré-requisito, não diferencial

**Regra:** Na primeira auditoria de qualquer conta Meta Ads, verificar CAPI e EMQ no Events Manager. Se EMQ < 6, nada mais vale auditar até o CAPI ser corrigido, porque o modelo não está aprendendo com sinal limpo e qualquer otimização vai rodar em terreno instável. Contas com CAPI bem implementado: +10 a +25% ROAS.
**Contexto:** Pós-Andromeda, qualidade do sinal vira o gargalo real. Diagnóstico que ignora EMQ é diagnóstico cego.

### 2026-04-12, Pietro Embalagens — campanhas sazonais começam 60–90 dias antes da data

**Regra:** Para Pietro Embalagens, NUNCA planejar campanha sazonal com menos de 60 dias de antecedência. O cliente final (confeiteira/artesã) precisa comprar a embalagem, produzir os doces/presentes e vender ANTES da data comemorativa — não na véspera. Planejar como mercado geral (1–2 semanas antes) leva a campanha atrasada e venda perdida.
**Datas principais e quando rodar:** Páscoa (abril) → início em janeiro/fevereiro | Dia das Mães (2º domingo de maio) → início em fevereiro/março | Natal (25/12) → início em outubro | Dia dos Namorados (12/06) → início em março/abril.
**Como aplicar:** Ao sugerir datas de lançamento de campanha sazonal para Pietro Embalagens, calcular 75 dias antes da data como ponto de partida ideal. Se já estivermos dentro dos 30 dias antes, alertar que a campanha está atrasada e ajustar copy/estratégia para urgência (últimas unidades, entrega rápida, etc.).

---
