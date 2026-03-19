# 💳 German Credit Risk - Análise de Risco de Crédito

## 📋 Descrição do Projeto

Análise completa e modelagem de risco de crédito utilizando o dataset do kaggle **German Credit Data With Risk**. O projeto implementa um pipeline de machine learning comparando **4 algoritmos** para prever o risco de inadimplência de clientes.

## 🎯 Objetivo

Desenvolver um classificador ML que identifique com precisão quais clientes apresentam **alto risco de inadimplência (classe 0)** versus **bom pagamento (classe 1)**, utilizando dados demográficos, comportamentais e financeiros.

---

## 📊 Dataset

| Aspecto | Detalhe |
|---------|---------|
| **Nome** | German Credit Data |
| **Registros** | ~1.000 clientes |
| **Features** | 21 variáveis (demográficas, financeiras, comportamentais) |
| **Classes** | Binária (Risco: 0, Bom: 1) |
| **Desbalanceamento** | ~70% Bom Pagador, ~30% Risco |
| **Tipo** | Classificação Binária |

### Principais Variáveis

```
Demográficas:
├─ Age (Idade)
├─ Sex (Gênero)
└─ Housing (Tipo de Moradia)

Financeiras:
├─ Credit amount (Valor do Crédito)
├─ Duration (Duração do Contrato)
├─ Saving accounts (Poupança)
└─ Checking account (Conta Corrente)

Comportamentais:
├─ Purpose (Finalidade do Crédito)
└─ Employment (Status de Emprego)
```

---

## 🔬 Metodologia

### **Fase 1: Análise Exploratória (EDA)**

#### 1.1 Auditoria de Metadados
- Inspeção de tipos de dados
- Identificação de valores nulos
- Análise de cardinalidade

#### 1.2 Análise Visual
- Distribuição de idade (histograma com KDE)
- Tipo de moradia (gráfico de barras)
- Contas de poupança (contagem)
- Valor do crédito (boxplot)
- Duração média por finalidade
- Finalidades mais comuns

#### 1.3 Separação de Classes (Poder Discriminante)
- Boxplot de variáveis por classe de risco
- KDE (Kernel Density Estimation) para densidade por classe
- Média e mediana por grupo

#### 1.4 Análise de Multicolinearidade
- Matriz de correlação de Pearson
- Heatmap para visualizar relações
- Cálculo de VIF (Variance Inflation Factor)

---

### **Fase 2: Preparação dos Dados**

#### 2.1 Tratamento de Valores Ausentes
```python
# Estratégia: Preservar ausência como categoria
Saving accounts: NaN → 'unavailable'
Checking account: NaN → 'unavailable'
```

**Justificativa**: A ausência de informação é um sinal estatístico importante de risco.

#### 2.2 Codificação de Variáveis Categóricas
- **One-Hot Encoding** com `drop_first=True`
- Evita **Dummy Variable Trap** (multicolinearidade perfeita)
- Colunas transformadas:
  - Sex (1 dummy)
  - Housing (2 dummies)
  - Saving accounts (3 dummies)
  - Checking account (3 dummies)
  - Purpose (9 dummies)

#### 2.3 Split Train/Test
```python
Test Size: 30%
Train Size: 70%
Stratification: Sim (mantém proporção de classes)
Random State: 42 (reprodutibilidade)
```

---

### **Fase 3: Padronização**

```python
StandardScaler()
# Transforma todas as variáveis para:
# - Média = 0
# - Desvio Padrão = 1
```

**Necessária para**: Regressão Logística (modelo linear)  
**Opcional para**: Tree-based models (XGBoost, LightGBM)

---

### **Fase 4: Balanceamento de Classes**

Como as classes estão desproporcionais (70/30), aplicamos estratégias diferentes por modelo:

#### **Regressão Logística**
```python
class_weight='balanced'  # Aumenta peso dos casos raros
```

#### **Random Forest**
```python
class_weight='balanced'  # Ajusta pesos nas árvores
```

#### **XGBoost**
```python
scale_pos_weight=2.3  # Ajustado para proporção 70/30
# scale_pos_weight = (negativos / positivos) = 0.7/0.3 = 2.3
```

#### **LightGBM**
```python
class_weight='balanced'  # Balanceamento automático
```

---

### **Fase 5: Diagnóstico de Modelos Lineares**

#### 5.1 Teste de Multicolinearidade
```python
# VIF > 5: Indicativo de multicolinearidade
# VIF: 1-5: Aceitável
```

#### 5.2 Teste de Heterocedasticidade
```python
# Resíduos vs Probabilidades Preditas
# Verificação de padrões (não-aleatoriedade)
```

---

### **Fase 6: Modelagem Comparativa**

#### **4 Algoritmos Testados**

| Modelo | Tipo | Complexidade | Vantagem |
|--------|------|-------------|----------|
| **Regressão Logística** | Linear | Baixa | Interpretável, baseline |
| **Random Forest** | Ensemble | Média | Robusto, não-linear |
| **XGBoost** | Gradient Boosting | Alta | SOTA em tabular |
| **LightGBM** | Gradient Boosting | Alta | Rápido, eficiente |

#### **Avaliação em 2 Etapas**

```
FASE 1: Treino com 5-Fold Cross-Validation
└─ Estima performance em dados não vistos

FASE 2: Teste Final (Hold-out)
└─ Avalia performance final em teste set
```

#### **Métricas Coletadas**
```
- Acurácia
- Precisão (Classe 0 = Risco)
- Recall (Sensibilidade)
- F1-Score
- ROC-AUC
- Tempo de Execução
```

---

### **Fase 7: Visualizações de Avaliação**

#### 7.1 Curvas ROC
```
Treino: 5-Fold Cross-Validation
Teste: Hold-out final

Interpretação:
- Curva mais próxima do canto superior = melhor
- AUC próximo de 1.0 = excelente
```

#### 7.2 Curvas de Aprendizado
```
Mostra:
- Performance em treino vs validação
- Gap entre treino e validação = overfitting?
- Necessidade de mais dados?
```

#### 7.3 Matrizes de Confusão
```
Comparativo:
- Treino (5-Folds)
- Teste (Hold-out)

Identifica:
- TP (Verdadeiros Positivos)
- TN (Verdadeiros Negativos)
- FP (Falsos Positivos)
- FN (Falsos Negativos)
```

---

## 📈 Resultados Principais

### **Comparativo de Modelos (Teste)**

| Modelo | ROC-AUC | Recall (Risco) | Precisão (Risco) | F1-Score |
|--------|---------|----------------|------------------|----------|
| **Regressão Logística** | ~0.72 | ~0.45 | ~0.65 | ~0.53 |
| **Random Forest** | ~0.78 | ~0.55 | ~0.68 | ~0.61 |
| **XGBoost** ⭐ | ~0.80+ | ~0.60+ | ~0.70+ | ~0.65+ |
| **LightGBM** | ~0.79 | ~0.58 | ~0.69 | ~0.63 |

**Melhor Modelo: XGBoost** (maior ROC-AUC e F1-Score)

### **Interpretação**

✅ **Detecção de Risco**: Modelos conseguem identificar ~60% dos verdadeiros riscos  
✅ **Confiabilidade**: Quando preveem "risco", estão corretos ~70% das vezes  
⚠️ **Trade-off**: Precisão vs Recall (típico em problemas de crédito)

---

## 💡 Insights Principais

### **1. Idade vs Risco**
<img width="531" height="404" alt="image" src="https://github.com/user-attachments/assets/9fd35e72-9f1c-44ee-a61c-6640181323f0" />

- Clientes mais jovens: Risco MAIOR
- Clientes mais velhos: Risco MENOR


### **2. Valor do Crédito vs Risco**
<img width="549" height="415" alt="image" src="https://github.com/user-attachments/assets/d81e6b9e-523a-40b1-994e-bd16a2598c86" />

- Créditos maiores: Risco MENOR (seleção mais rigorosa)
- Créditos menores: Variabilidade MAIOR


### **3. Duração do Contrato vs Risco**
<img width="534" height="403" alt="image" src="https://github.com/user-attachments/assets/7ec040ec-22e2-4f45-bc6b-19826e7e54e7" />

- Contratos mais curtos: Risco MAIOR
- Contratos mais longos: Risco MENOR (clientes consolidados)


### **4. Finalidade do Crédito**

<img width="1403" height="768" alt="image" src="https://github.com/user-attachments/assets/b908d110-7ce2-4a9f-882e-a4fc3faa16ea" />

---

## 🛠️ Tecnologias Utilizadas

```
Python 3.x
├─ pandas          → Manipulação de dados
├─ numpy           → Computação numérica
├─ scikit-learn    → Modelos ML, preprocessing
├─ xgboost         → Gradient Boosting
├─ lightgbm        → Light Gradient Boosting
├─ statsmodels     → Análise estatística
├─ matplotlib      → Visualizações básicas
├─ seaborn         → Visualizações avançadas
└─ imblearn        → Desbalanceamento

Técnicas:
├─ EDA (Análise Exploratória)
├─ Feature Engineering
├─ One-Hot Encoding
├─ Padronização (StandardScaler)
├─ Balanceamento de Classes
├─ Validação Cruzada (5-Fold)
├─ ROC-AUC
└─ Cross-Validation com K-Folds
```

---

## 🎓 O Que Aprendi

✅ **Pipelines de ML Completos**: Do dado bruto ao modelo em produção  
✅ **Desbalanceamento de Classes**: Como lidar com proporções 70/30  
✅ **Comparação de Algoritmos**: Quando usar qual modelo  
✅ **Validação Robusta**: 5-Fold CV vs Hold-out  
✅ **Diagnóstico de Modelos**: Detecção de overfitting e underfitting  
✅ **Interpretabilidade**: Feature importance e SHAP values  
✅ **Visualizações Avançadas**: ROC, learning curves, confusion matrices  

---

## 🚀 Como Usar o Código

### **Estrutura do Notebook**

```
1. Importações de Bibliotecas
2. Carregamento dos Dados
3. Análise Exploratória (EDA)
   ├─ Metadados
   ├─ Distribuições
   ├─ Correlações
   └─ Multicolinearidade
4. Preparação dos Dados
   ├─ Tratamento de NaN
   ├─ Encoding
   └─ Split Train/Test
5. Padronização
6. Diagnóstico de Modelos Lineares
7. Modelagem Comparativa
   ├─ Treino com CV
   ├─ Teste Final
   └─ Ranking
8. Visualizações
   ├─ Curvas ROC
   ├─ Curvas de Aprendizado
   └─ Matrizes de Confusão
```

### **Executar Localmente**

```bash
# 1. Instalar dependências
pip install pandas numpy scikit-learn xgboost lightgbm statsmodels matplotlib seaborn imbalanced-learn

# 2. Executar o notebook
jupyter notebook german_credit_risk.py

# 3. Ou converteu para script Python
python german_credit_risk.py
```

---


## 🔗 Repositório

Arquivo: [german_credit_risk.py](https://www.kaggle.com/datasets/kabure/german-credit-data-with-risk)
Tamanho: ~600 linhas
Tema: Risco de Crédito / Classificação Binária


