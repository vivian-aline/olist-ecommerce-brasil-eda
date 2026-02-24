# Análise E-commerce Olist Brasil: Performance e Oportunidades

Análise exploratória completa de 100.000 pedidos reais da Olist (marketplace brasileiro, 2016-2018), focada em identificar padrões de receita, crescimento, categorias de produtos, geografia, logística e experiência do cliente.

Projeto desenvolvido individualmente como parte do meu portfólio para Analista de Dados Pleno, utilizando PySpark para big data e pipeline completo de ETL + EDA.

---

## 1. Problema de Negócio

**Marketplace brasileiro precisa otimizar operações e crescimento com base em dados reais anonimizados:**
- Receita cresceu 25% em 2018 vs 2017, mas AOV estável → foco em upsell/cross-sell.
- São Paulo concentra 41% dos clientes mas tempo de entrega 20% maior que média.
- Beleza e saúde lidera receita (R$ 1,4M), mas categorias como portateis para casa têm AOV de R$ 669 (oportunidade).

**Perguntas centrais:**
> 1. Quais categorias geram mais receita e onde investir marketing?  
> 2. São Paulo domina vendas, mas logística é eficiente?  
> 3. Como evolui receita, AOV e satisfação ao longo do tempo?

---

## 2. Dados Utilizados

**Dataset público Olist (Kaggle)**: 9 tabelas com ~100k pedidos reais (2016-2018).

| Tabela | Registros | Descrição |
|--------|-----------|-----------|
| `olist_customers_dataset.csv` | 99.441 | Clientes únicos (customer_unique_id) |
| `olist_orders_dataset.csv` | 99.441 | Ciclo completo do pedido (datas, status) |
| `olist_order_items_dataset.csv` | 112.650 | Itens por pedido (preço, frete, seller) |
| `olist_order_payments_dataset.csv` | 103.886 | Pagamentos (tipo, parcelas, valor) |
| `olist_order_reviews_dataset.csv` | 104.162 | Avaliações (score 1-5, comentários) |
| `olist_products_dataset.csv` | 32.951 | Produtos (categoria, peso, dimensões) |
| `olist_sellers_dataset.csv` | 3.095 | Vendedores (localização) |

**Qualidade inicial:** Dataset limpo (poucos nulos/duplicatas), mas com ~610 produtos sem categoria e reviews com 63k comentários nulos (normal).

**Stack técnica:**
- **Processamento:** PySpark (Google Colab)
- **Bibliotecas:** `pyspark.sql`, `Window`, funções de agregação
- **Visualização:** Matplotlib/Seaborn (notebooks) + exportação para BI

---

## 3. Metodologia (Pipeline ETL + EDA)

### **Fase 1: Exploração Inicial** (01_Olist_DataExploration.ipynb)
- Schema, nulos (produto: 610 sem categoria), duplicatas (reviews: 85).
- Relacionamentos: 100% íntegros (exceto ~3k pedidos cancelados sem itens).

### **Fase 2: Limpeza** (02_Olist_DataCleaning.ipynb)
- Criação de variáveis: `dias_entrega`, `status_valido`, `regiao`.
- Padronização: Categorias, estados (SP: 41k clientes), reviews (classificação Ruim/Média/Boa).
- Tratamento: Remoção de ~610 produtos sem categoria; nulos em datas de entrega (cancelados).

### **Fase 3: EDA + 9 KPIs Estratégicos** (03_Olist_EDA_Analise.ipynb)
- Joins entre tabelas (pedido → cliente → item → pagamento → review).
- KPIs calculados: Receita mensal/anual, AOV, crescimento YoY, performance por categoria/cidade.

### **Fase 4: Exportação**
- CSVs limpos e KPIs exportados para Power BI/Looker.

**Estrutura de pastas no repositório:**
.
├── data/
│   ├── raw/              # Originais Olist
│   ├── cleaned/          # Tratados (7 CSVs)
│   └── kpis/             # 9 CSVs com KPIs
├── notebooks/
│   ├── 01_DataExploration.ipynb
│   ├── 02_DataCleaning.ipynb
│   ├── 03_EDA_Analise.ipynb
│   ├── 04_juntar_arquivo.ipynb
│   └── 05_exportando_CSV.ipynb
├── dashboard/
│   └── dashboard_olist.pbix
└── README.md

---

## 4. Principais Insights

### **4.1. Receita e Crescimento**
| Ano | Receita Total | Crescimento YoY |
|-----|---------------|-----------------|
| 2016 | R$ 57k | - |
| **2017** | **R$ 7M** | **+12.142%** |
| **2018** | **R$ 8,7M** | **+25%** |

- **Tendência mensal:** Pico em nov/dez (Black Friday/Natal).
- **AOV mensal:** Estável ~R$ 160 (2017-18), mas subiu 15% nos últimos 6 meses.

### **4.2. Performance por Categoria**
| Top 5 Categorias | Receita | Volume | AOV |
|------------------|---------|--------|-----|
| **belezasaude** | R$ 1,4M | 5.967 | - |
| **relogiospresentes** | R$ 1,3M | 3.599 | - |
| **camamesabanho** | R$ 1,2M | 11.115 | **Alto crescimento (+95%)** |
| **esportelazer** | R$ 1,1M | 7.864 | - |
| **portateiscasa** | - | 75 | **R$ 669 (alto AOV)** |

### **4.3. Insights Geográficos**
| Cidade | % Receita | % Pedidos | Tempo Entrega (dias) |
|--------|-----------|-----------|----------------------|
| **São Paulo** | **13,7%** | **18%** | **8,7** (20% > média) |
| Rio de Janeiro | 7,3% | 7,9% | - |
| Belo Horizonte | 2,6% | 3,2% | - |

### **4.4. Logística e Satisfação**
- **Tempo médio entrega:** SP (8,7 dias), PR/MG (~12 dias).
- **Reviews:** 57k nota 5 (boa), mas 96k entregues → oportunidade de coletar mais feedback.

---

## 5. Conclusões e Recomendações

✅ **Crescimento sólido** (25% em 2018), mas **AOV estagnado** → investir em upsell.  
✅ **SP domina** (41% clientes), mas logística ineficiente → estoque local ou parceiros regionais.  
✅ **Oportunidades:** Categorias alto AOV/baixo volume (portateis: R$669); crescimento rápido (camas: +95%).

**Ações prioritárias:**
1. Campanhas em **portateiscasa** e **moveisquarto** (alto AOV).
2. Otimizar frete SP (maior volume, maior atraso).
3. Programa de reviews para NPS (só 50% dos entregues avaliam).

---

## 6. Próximos Passos Sugeridos
- **Dashboard Power BI** com KPIs interativos (receita mensal, mapa cidades, AOV categoria).
- **RFM Analysis** para segmentação clientes.
- **Previsão demanda** (Prophet/Time Series).

**Notebook Colab principal:** (03_Olist_EDA_Analise.ipynb)

---
*Desenvolvido por Vivian Aline Inoue | Dez/2025*
