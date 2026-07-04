# Análise de Negócios: Comportamento de Vendas & Eficiência Logística no E-Commerce (Olist)

## 📝 Descrição do Projeto

Este projeto foi desenvolvido para simular o papel de um Analista de Dados em uma operação real de e-commerce (utilizando a base pública da Olist). O objetivo central foi investigar como a eficiência logística e o comportamento do consumidor impactam diretamente o faturamento e a retenção de clientes (churn).

A partir de dados brutos e descentralizados, foi construído um ecossistema ponta a ponta que engloba:
1. **Diagnóstico Inicial (Excel):** Mapeamento do banco de dados relacional e levantamento de hipóteses de negócio.
2. **Engenharia & Tratamento de Dados (Python/Pandas):** Consolidação de tabelas fato através de joins estruturados, tratamento cirúrgico de dados omissos e criação de métricas temporais customizadas.
3. **Análise Visual & BI (Power BI):** Modelagem de dados em formato Estrela (*Star Schema*) conectada a uma dimensão calendário (`dCalendario`), resultando em um dashboard dinâmico com visuais de escala mista para suporte à tomada de decisão gerencial.

---

## 🗺️ Cronograma de Execução e Checklist

### 🟩 Fase 1: Exploração Inicial & Definição (Excel)
* [x] Baixar os arquivos .csv no Kaggle.
* [x] Abrir as tabelas no Excel para entender o dicionário de dados (colunas, IDs, tipos de variáveis).
* [x] Fazer uma filtragem rápida para identificar anomalias gritantes (valores zerados, colunas vazias).
* [x] Mapeamento prévio de dores de negócio e formulação de hipóteses de diagnóstico operacionais.

### 🟨 Fase 2: Limpeza e Manipulação de Dados (Python)
* [x] Preparação: Importar bibliotecas e remover linhas duplicadas das tabelas brutas.
* [x] Unificação (Merge): Cruzar as tabelas de pedidos, itens e pagamentos usando a chave `order_id` via Pandas.
* [x] Tratamento de Dados: Converter as strings de data para `datetime` e validar os valores nulos estruturais.
* [x] Engenharia de Recursos: Criar as métricas de Tempo de Entrega Real, Dias de Atraso e Ticket Médio.
* [x] Exportação: Gerar o arquivo final unificado `olist_consolidado_clean.csv`.

### 🟧 Fase 3: Análise Exploratória e Estatística (Python)
* [x] Visão Geral do Negócio: Calcular o Faturamento Total, Volume Total de Pedidos e a taxa de conversão de status.
* [x] Análise de Logística: Identificar a média de tempo de entrega e a taxa real de pedidos atrasados vs. antecipados.
* [x] Análise de Churn & Cancelamentos: Mapear o volume de pedidos cancelados e investigar se há padrões temporais ou de valor.

### 🟪 Fase 4: Dashboard Interativo (Power BI)
* [x] Modelagem e Carga: Importar o arquivo `olist_consolidado_clean.csv` e validar a tipagem dos dados no Power Query.
* [x] Design e Layout: Estruturar o visual do painel com um background limpo, organizando as seções de Finanças, Logística e Filtros.
* [x] Construção dos Cartões de KPI: Criar os blocos de destaque para Faturamento Total, Volume de Pedidos e Ticket Médio (conforme validados no Python).
* [x] Gráficos e Visuais de Suporte: Desenhar o gráfico de eficiência logística (Prazo vs. Atraso) com eixo secundário e o gráfico de impacto de cancelamentos.
* [x] Interatividade e Filtros: Configurar segmentadores por Estado e Ano para permitir análises dinâmicas pelo usuário final.

### 🟪 Fase 5: Documentação e Publicação (GitHub & LinkedIn)
* [x] Estruturar e documentar o arquivo `README.md` unificado.
* [x] Subir o código do Jupyter Notebook estruturado e comentado para o GitHub.
* [x] Escrever o post de impacto para o LinkedIn (Focando no problema resolvido e nos insights encontrados).

---

## 🧠 Respostas aos Insights de Negócio e Hipóteses da Fase 1

Durante a fase de exploração inicial no Excel, foram levantadas três grandes hipóteses de negócio. Abaixo está a validação estruturada de cada uma delas com base no que foi desenvolvido em Python e visualizado no Power BI:

### 1. O Fenômeno do "Cancelamento Tardio" e o Tratamento de Dados Omissos
* **O que foi observado na Fase 1:** Pedidos com status `canceled` ou em processamento que apresentavam valores nulos (`null`) nas colunas de data (como `order_delivered_customer_date`), sugerindo desistências antes da conclusão do fluxo de entrega ou na etapa de pré-compra.
* **Decisão de Engenharia no Power BI (Documentada):** 
  * Para garantir a consistência visual e a ordem cronológica do eixo temporal nos gráficos do Power BI, optou-se por aplicar um **filtro a nível de visual** nos gráficos de linha e de barras, ocultando os registros cujas datas estavam em branco.
  * *Justificativa Técnica:* Como esses registros representam cancelamentos e abandonos de fluxo pré-compra (sem data de fechamento associada), mantê-los no eixo X geraria uma categoria desordenada que comprometeria a estética e a leitura cronológica do dashboard. A volumetria total, contudo, permanece preservada no banco de dados subjacente para futuras auditorias de funil.

### 2. Eficiência Operacional: Onde está o real gargalo do Churn?
* **O que foi observado na Fase 1:** Hipótese de que o gargalo de cancelamentos estava na logística externa (transportadoras) e não na separação interna.
* **Resolução & Resposta no Projeto:** 
  * A hipótese foi **confirmada e quantificada** através do gráfico de *Eficiência Logística*. 
  * Ao ajustarmos o eixo secundário para refletir valores de atraso e antecipação, ficou evidente que a operação trabalha majoritariamente com entregas antecipadas (valores de dias de atraso negativos no gráfico). 
  * Os picos de cancelamento e oscilação visualizados no Power BI coincidem com os períodos onde a linha de dias de entrega tendeu a se aproximar ou estourar a meta de prazo, provando que a previsibilidade de entrega afeta diretamente a retenção do cliente.

### 3. Sazonalidade de Novembro (Efeito Black Friday)
* **O que foi observado na Fase 1:** O volume absoluto de cancelamentos disparava em novembro. Hipótese de que era apenas um efeito de base pelo aumento de vendas da Black Friday.
* **Resolução & Resposta no Projeto:** 
  * No Python, realizamos a normalização dos dados criando métricas volumétricas agregadas. 
  * Ao plotar o *Total de Cancelamentos por Mês/Ano* no Power BI ao lado da *Evolução do Faturamento Mensal*, confirmamos que o aumento de cancelamentos em novembro acompanha proporcionalmente o pico de faturamento de \$1 Mi (novembro de 2017). Logo, a eficiência percentual da operação se manteve estável, validando a hipótese de "Efeito de Base".

* **Caso de Estudo Adicional (Fevereiro/Março de 2018):** Identificou-se um pico anômalo de cancelamentos em escala nacional em fevereiro de 2018, antecedendo o pico histórico de atrasos de entrega observados em março de 2018. 
  * *Conclusão de Negócio:* O comportamento indica um efeito cascata onde o estresse logístico nas transportadoras gerou uma onda de cancelamentos por quebra de expectativa (desistência preventiva) antes mesmo do sistema computar os atrasos reais das entregas represadas.
---

## 📊 Estrutura do Dashboard e KPIs Principais

O painel final foi estruturado estrategicamente nas seguintes seções:
1. **Blocos de KPI (High-Level):**
   * **Faturamento Total:** \$13,59 Mi
   * **Volume de Pedidos:** 99 Mil
   * **Ticket Médio:** \$136,68
2. **Evolução do Faturamento Mensal:** Gráfico de linha de tendência demonstrando a sazonalidade e histórico de receita histórica.
3. **Eficiência Logística (Prazo vs Atraso):** Gráfico misto que expõe visualmente o impacto do volume de vendas sobre os prazos de entrega reais e metas de antecipação (tratando o eixo secundário de forma a suportar valores de dias negativos para adiantamentos).
4. **Análise de Cancelamentos por Mês/Ano:** Gráfico de barras horizontais focado em monitorar o comportamento temporal de quebras de funil.
5. **Filtros Dinâmicos:** Segmentação interativa por Ano e Estado do consumidor para análises granulares.

---

## 🚀 Como Executar o Projeto

1. Clone o repositório:
   ```bash
   git clone 
