# CP05 — Sistemas de Energia Renovável e Sustentabilidade

## Objetivo

Este projeto tem como objetivo aplicar técnicas de **Machine Learning** na análise de dados relacionados a sistemas de energia renovável.

---

# Tarefa 1 — Classificação da fonte de energia

## Objetivo

A primeira tarefa consiste em classificar a fonte de geração de um empreendimento como:

- **Eólica**
- **Hidráulica**
- **Solar**

Para isso, foram utilizadas as seguintes variáveis de entrada:

- `potencia_kw`
- `latitude`
- `longitude`

A variável que o modelo deve prever é `fonte`.

## Origem dos dados

Os dados utilizados nesta tarefa estão no arquivo:

`aneel_classificacao_orange.csv`

O conjunto de dados tem como origem informações relacionadas aos empreendimentos de geração de energia elétrica disponibilizados pela **ANEEL — Agência Nacional de Energia Elétrica**.

As fontes hidráulicas consideradas no conjunto incluem diferentes categorias de empreendimentos, como UHE, PCH e CGH, agrupadas na classe **Hidráulica**.

## Metodologia

Inicialmente, os dados foram explorados para verificar sua estrutura, valores ausentes, distribuição das classes e estatísticas descritivas.

As variáveis `potencia_kw`, `latitude` e `longitude` foram utilizadas como características (`X`), enquanto `fonte` foi utilizada como variável-alvo (`y`).

Os dados foram divididos em:

- **80% para treinamento**
- **20% para teste**

Foi utilizada uma divisão **estratificada**, preservando a proporção das classes nos conjuntos de treinamento e teste.

Foram avaliados três algoritmos de classificação:

- K-Nearest Neighbors (KNN)
- Regressão Logística
- Random Forest

Para KNN e Regressão Logística foi utilizada padronização das variáveis por meio de `StandardScaler`. O Random Forest não necessita dessa padronização.

## Avaliação

Os modelos foram avaliados utilizando:

- Acurácia
- Precisão
- Recall
- F1-score

Para Precisão, Recall e F1-score foi utilizada a média **macro**, atribuindo o mesmo peso a cada classe.

Também foram utilizadas **matrizes de confusão** para analisar os erros de classificação entre Eólica, Hidráulica e Solar.

## Conclusões

Os resultados mostram que é possível realizar a classificação da fonte de geração utilizando somente potência instalada e localização, porém existem limitações importantes.

Na matriz de confusão da Regressão Logística, por exemplo, a maior confusão ocorreu entre as classes **Solar e Eólica**, com 55 empreendimentos solares classificados como eólicos.

Isso demonstra que potência, latitude e longitude, isoladamente, não fornecem informações suficientes para distinguir perfeitamente as diferentes tecnologias de geração.

A inclusão de outras características, como condições ambientais, disponibilidade de recursos naturais, características do terreno e informações técnicas dos empreendimentos, poderia melhorar o desempenho da classificação.

---

# Tarefa 2 — Regressão da radiação solar

## Objetivo

A segunda tarefa consiste em prever a **radiação solar global horizontal**, medida em `W/m²`, utilizando variáveis meteorológicas e a hora do dia.

A variável-alvo é:

`radiacao_w_m2`

Foram utilizadas como características:

- `temperatura_c`
- `umidade_pct`
- `nuvens_pct`
- `vento_kmh`
- `hora`

## Origem dos dados

Os dados utilizados nesta tarefa estão no arquivo:

`meteo_regressao_orange.csv`

Os dados meteorológicos são provenientes do **Open-Meteo**, referentes à região de Petrolina, Pernambuco.

## Período dos dados

O conjunto de dados utilizado corresponde ao período de 01/04/2025 a 30/06/2025, no fuso `America/Recife`.

## Metodologia

Inicialmente, os dados foram organizados cronologicamente e analisados quanto à estrutura, valores ausentes e estatísticas descritivas.

Também foi realizada uma visualização do comportamento médio da radiação solar de acordo com a hora do dia.

As variáveis meteorológicas e a hora foram utilizadas como características (`X`), enquanto `radiacao_w_m2` foi utilizada como variável-alvo (`y`).

Para evitar vazamento de informações temporais, os dados foram divididos respeitando sua ordem cronológica:

- **Primeiros 80%:** treinamento
- **Últimos 20%:** teste

Dessa forma, o modelo é treinado com dados anteriores e avaliado em dados posteriores.

Foram utilizados três modelos de regressão:

- Regressão Linear
- Árvore de Decisão
- Random Forest

## Avaliação

Os modelos foram avaliados utilizando:

- MAE (Mean Absolute Error)
- MSE (Mean Squared Error)
- R² (Coeficiente de Determinação)

Além das métricas, foi realizada uma comparação entre os valores reais e os valores previstos pelo modelo de melhor desempenho.

## Conclusões

Os modelos conseguiram representar a relação entre as variáveis meteorológicas, a hora do dia e a radiação solar.

A variável `hora` possui papel importante porque a radiação solar apresenta um comportamento diretamente relacionado à posição do Sol. No período analisado, a radiação tende a aumentar durante a manhã, atingir valores mais elevados próximo ao meio do dia e diminuir durante a tarde.

Entre os modelos avaliados, o **Random Forest apresentou o melhor desempenho geral**, considerando as métricas obtidas no conjunto de teste.

É importante destacar que **radiação solar não é o mesmo que geração de energia elétrica fotovoltaica**. A radiação representa a quantidade de energia solar incidente sobre uma superfície, enquanto a geração elétrica depende também de fatores como:

- orientação e inclinação dos painéis;
- eficiência dos módulos fotovoltaicos;
- temperatura dos módulos;
- sombreamento;
- sujeira;
- perdas em cabos e inversores;
- potência instalada do sistema.

Portanto, o modelo desenvolvido nesta tarefa realiza a previsão de **radiação solar**, e não diretamente da quantidade de energia elétrica produzida.

---

# Como executar o projeto

## Requisitos

É necessário possuir Python instalado ou utilizar o Google Colab, e instalar (ou só importar, no Colab) as seguintes bibliotecas:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
