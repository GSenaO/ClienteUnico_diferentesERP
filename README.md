# README - Análise Exploratória de Dados E-commerce

## 📋 Visão Geral do Projeto

Este script realiza uma análise exploratória completa de dados de um dataset de E-commerce, incluindo:
- Análises estatísticas e de negócio
- Análise RFM (Recência, Frequência, Monetário) para segmentação de clientes
- Visualizações gráficas dos resultados

---

## 📋Contexto da Modificação do Dataset
O dataset original do Olist não possui identificadores únicos de cliente entre diferentes pedidos (cada compra gera um customer_id novo). Para simular o cenário real de empresas multi-canal — como holdings com diferentes unidades de negócio — o dataset foi enriquecido com 5.000 CPFs sintéticos atribuídos aleatoriamente às compras da base original.
Isso permite treinar e demonstrar:

Construção da visão de cliente único (single customer view) a partir de múltiplas transações
Match de registros entre fontes de dados distintas via CPF como chave
Rastreio do comportamento do cliente ao longo do tempo e entre categorias
Análise RFM realista em um contexto onde um cliente tem múltiplos pedidos

Nota: os valores monetários e de frequência resultantes são superiores ao dataset original justamente por refletir o histórico consolidado por CPF, não por pedido.

## 🚀 Como Executar o Script

### Pré-requisitos
- Python 3.x instalado
- Bibliotecas necessárias: `pandas`, `matplotlib`

### Instalação das Dependências
```bash
pip install pandas matplotlib
```

### Executando o Script
1. Certifique-se de que o arquivo `df_ecommerce.csv` está no diretório correto
2. Execute o notebook no Jupyter ou rode o script Python diretamente:
```bash
python seu_script.py
```

---

## 📖 Descrição dos Passos Realizados

### 1. Carregamento dos Dados
```python
df_ecommerce = pd.read_csv('df_ecommerce.csv', sep=',')
```
- Carrega o dataset de E-commerce a partir do arquivo CSV
- Utiliza o separador padrão vírgula

### 2. Definição de Funções Exploratórias

#### 2.1 `check_cross_state_purchases(df)`
**Objetivo:** Verificar se existem clientes que compram de vendedores de outro estado.

**Retorna:**
- `exists`: Boolean indicando se existem compras interestaduais
- `count`: Número de compras interestaduais

**Lógica:** Compara `customer_state` com `seller_state` e conta as ocorrências diferentes.

---

#### 2.2 `top_selling_categories(df)`
**Objetivo:** Identificar as categorias de produtos que mais vendem.

**Retorna:** Série ordenada com valor total de vendas por categoria.

**Lógica:** Agrupa por `product_category_name` e soma `payment_value`, ordenando decrescente.

---

#### 2.3 `average_price_per_category(df)`
**Objetivo:** Calcular a média de preço de compra por categoria.

**Retorna:** Série ordenada com média de preço por categoria.

**Lógica:** Agrupa por `product_category_name` e calcula média de `payment_value`.

---

#### 2.4 `freight_payment_correlation(df)`
**Objetivo:** Verificar a correlação entre valor do frete e valor da compra.

**Retorna:** Coeficiente de correlação de Pearson.

**Lógica:** Calcula correlação entre `freight_value` e `payment_value`.

---

#### 2.5 `most_used_payment_types(df)`
**Objetivo:** Identificar os tipos de pagamento mais utilizados.

**Retorna:** Série com contagem de cada tipo de pagamento.

**Lógica:** Conta valores da coluna `payment_type`.

---

#### 2.6 `most_used_installments(df)`
**Objetivo:** Identificar o número de parcelas mais utilizado.

**Retorna:** Série com contagem de cada opção de parcelamento.

**Lógica:** Conta valores da coluna `payment_installments`.

---

#### 2.7 `promoters_detractors(df)`
**Objetivo:** Identificar clientes promotores e detratores baseados em reviews.

**Retorna:**
- `promoters`: Clientes com score 4-5
- `detractors`: Clientes com score 1-2
- `total`: Total de reviews

**Lógica:** Filtra por `review_score` e conta as ocorrências.

---

#### 2.8 `rfm_analysis(df)`
**Objetivo:** Realizar análise RFM completa para segmentação de clientes.

**Métricas calculadas:**
- **Recency:** Dias desde a última compra
- **Frequency:** Número de pedidos únicos por cliente
- **Monetary:** Soma total dos valores de pagamento por cliente

**Métricas adicionais:**
- `First_Purchase`: Data da primeira compra
- `Last_Purchase`: Data da última compra
- `Avg_Review_Score`: Média das avaliações
- `Avg_Purchase_Interval`: Média de dias entre compras
- `Top_Category`: Categoria mais comprada
- `Top_Category_Value`: Valor gasto na categoria principal

**Scoring:**
- R_Score: 1-4 (menor recência = melhor)
- F_Score: 1-4 (maior frequência = melhor)
- M_Score: 1-4 (maior monetary = melhor)

**Segmentação:**
- **Campeões:** R=4, F=4, M=4
- **Clientes Valiosos:** R≥3, F≥3, M≥3
- **Clientes Recentes:** R≥3, F≥1, M≥1
- **Clientes Fiéis:** R≥2, F≥2, M≥2
- **Clientes Perdidos:** R=1
- **Clientes de Baixa Frequência:** F=1
- **Outros:** Demais combinações

---

### 3. Execução das Análises

O script executa as seguintes perguntas de negócio:

1. **Compras interestaduais:** Verifica se existem clientes que compram de vendedores de outro estado
2. **Top categorias:** Lista as 10 categorias que mais vendem
3. **Média de preço:** Calcula média de preço por categoria
4. **Correlação frete:** Verifica correlação entre valor do frete e valor da compra
5. **Tipos de pagamento:** Mostra os tipos de pagamento mais utilizados
6. **Parcelas:** Mostra o número de parcelas mais utilizado
7. **Promotores/Detratores:** Identifica clientes promoters e detratores + calcula NPS
8. **Análise RFM:** Executa análise RFM completa
9. **Segmentos:** Mostra distribuição dos segmentos de clientes
10. **Estatísticas RFM:** Exibe estatísticas descritivas das métricas RFM

---

### 4. Visualizações Gráficas

O script gera os seguintes gráficos:

1. **Compras por Estado:** Gráfico de barras comparando compras no mesmo estado vs outro estado
2. **Top 10 Categorias:** Gráfico de barras das categorias que mais vendem
3. **Média de Preço por Categoria:** Gráfico de barras das médias de preço
4. **Correlação Frete vs Pagamento:** Scatter plot mostrando a correlação
5. **Tipos de Pagamento:** Gráfico de barras dos tipos de pagamento
6. **Número de Parcelas:** Gráfico de barras das opções de parcelamento
7. **Promotores/Detratores:** Gráfico de barras da distribuição
8. **Distribuição de Segmentos:** Gráfico de barras da distribuição percentual

---

## 📊 Principais Conclusões

### 1. Compras Interestaduais
- Existem **73.871 compras interestaduais** onde o cliente e o vendedor estão em estados diferentes
- Isso indica que a logística interestadual é relevante para o negócio
- Clientes aceitam comprar de vendedores de outros estados, demonstrando alcance nacional da operação

### 2. Categorias Mais Vendidas (Volume Total)
- **cama_mesa_banho** (R$ 1.725.465,67), **beleza_saude** (R$ 1.646.292,53) e **informatica_acessorios** (R$ 1.592.611,66) lideram o faturamento total
- Essas categorias de uso diário garantem o fluxo de caixa e a recorrência diária da operação
- Recomenda-se manter o foco de marketing nessas categorias

### 3. Ticket Médio por Categoria
- **pcs** (R$ 1.244,45), **telefonia_fixa** (R$ 767,30) e **portateis_casa_forno_e_cafe** (R$ 656,79) possuem os maiores tickets médios
- Categorias de alto valor agregado justificam fretes mais elevados

### 4. Correlação Frete vs Pagamento
- **Coeficiente de correlação: 0.3740** (positiva)
- O frete não é um impeditivo para alto valor agregado: compras mais caras suportam fretes mais caros
- Categorias pesadas ou tecnológicas (como PCs e eletrodomésticos) naturalmente custam mais para transportar
- O cliente aceita pagar esse custo logístico porque o bem justifica

### 5. Estrutura de Pagamentos
- **Cartão de crédito** domina de forma esmagadora (85.291 transações)
- **Boleto** (22.517), **voucher** (6.163) e **débito** (1.663) complementam
- **Parcelamento:** 57.616 clientes pagam à vista (1x), mas há um salto estratégico em 10x (6.786)
- Isso indica duas jornadas de compra: conveniência/baixo ticket (1x) e compra planejada de bens duráveis (10x)

### 6. Análise de Sentimento (NPS)
- **Promotores (score 4-5):** 87.338 clientes
- **Detratores (score 1-2):** 18.575 clientes
- **NPS aproximado: 59.47%** (zona de qualidade)
- O volume de detratores pode estar atrelado à alta taxa de clientes perdidos
- Problemas com compras interestaduais (logística/prazo) costumam ser os maiores ofensores de NPS no Brasil

### 7. Segmentação RFM - Saúde da Base

| Segmento | Proporção | Descrição |
|----------|-----------|-----------|
| Clientes Recentes | 26.14% | Nova aquisição |
| Clientes Perdidos | 23.18% | Evasão/Churn |
| Clientes Fiéis | 20.42% | Compras recorrentes |
| Clientes Valiosos | 18.42% | Alto valor |
| Outros | 5.94% | Diversos |
| Campeões | 5.9% | Melhores clientes |

**Estatísticas RFM:**
- Recência média: 26,3 dias
- Frequência média: 23 pedidos
- Valor monetário médio: R$ 3.987,21

### 8. Diagnóstico Principal

**Aquisição forte, mas Retenção frágil:**
- A operação é muito boa em atrair novos usuários (Clientes Recentes = 26,14%)
- Porém, a base tem um alto índice de Churn (Clientes Perdidos = 23,18%)
- Apenas 5,9% se tornam Campeões

---

## 💡 Recomendações Estratégicas

### Para Campeões (5,9%) e Clientes Fiéis (20,42%)
- **Não** oferecer descontos agressivos — eles já compram de você
- Oferecer exclusividade: lançamentos antecipados, programas de indicação
- Cross-sell de categorias de alto ticket (ex: empurrar PCs para quem já compra Informática)

### Para Clientes Recentes (26,14%)
- Foco total na **segunda compra**
-many acabando se perdendo após a primeira transação
- Enviar automação com voucher semanas após a primeira compra para garantir conversão em "Clientes Valiosos"

### Para Clientes Perdidos (23,18%)
- Rodar campanhas de **Win-back** (recuperação)
- Como o ticket médio geral é alto (~R$ 3.987), vale a pena sacrificar margem oferecendo "Frete Grátis" agressivo para reativar capital adormecido

### Para Clientes de Baixa Frequência
- Identificar padrões de abandono
- Criar sequências de reengajamento personalizadas
- A recência média indica risco de churn (clientes não compram há algum tempo)
- **Segmentos mais comuns:** Clientes Recentes e Clientes de Baixa Frequência
- A empresa tem uma base grande, mas com baixo engajamento

### 7. Recomendações Estratégicas
1. **Programas de fidelização** para aumentar frequência de compra
2. **Campanhas de reativação** para clientes perdidos
3. **Ofertas personalizadas** baseadas na categoria preferida
4. **Melhoria no atendimento** para reduzir detratores
5. **Análise de carrinhos abandonados** para aumentar conversão

---

## 📁 Arquivos de Entrada

- `df_ecommerce.csv`: Dataset principal com dados de pedidos, clientes, vendedores e produtos

---

## 📝 Observações

- O script assume que o arquivo CSV está no mesmo diretório ou o caminho está correto
- Algumas funções podem necessitar de ajustes dependendo da estrutura do dataset
- Recomenda-se verificar os nomes das colunas antes de executar

---

## 🔧 Personalização

Para adaptar o script a outros datasets:
1. Atualize o caminho do arquivo CSV na função `read_csv`
2. Verifique se os nomes das colunas batem com as do seu dataset
3. Ajuste os parâmetros de segmentação RFM conforme necessidade do negócio
