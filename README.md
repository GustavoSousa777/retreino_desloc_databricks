# Retreino Desloc: Ciclo Completo de Ciência de Dados no Databricks

Projeto prático desenvolvido a partir de um estudo de caso do curso **"Ciência de Dados na Plataforma do Databricks"**, simulando o trabalho de um Cientista de Dados na **Desloc**, uma empresa fictícia de mobilidade urbana (modelo similar ao Uber).

## 🎯 Destaque do projeto

Este repositório documenta um **ciclo completo de ciência de dados**, do dado bruto até o modelo em produção servindo previsões via API, e não apenas um notebook de modelagem isolado. O pipeline cobre:

**Ingestão, Estruturação, EDA, Preparação, Modelagem e Deploy via API (MLflow / Databricks Model Serving)**

O ponto alto do projeto é o **deploy real do modelo como endpoint REST no Databricks**, incluindo registro do experimento no MLflow, definição de assinatura do modelo (`ModelSignature`) e teste do endpoint em produção via requisição HTTP autenticada. Essa etapa fecha o ciclo que, na prática, separa um modelo treinado de um modelo efetivamente utilizável pelo negócio.

## 📋 Contexto de negócio

O projeto segue a metodologia **CRISP-DM**, a partir do ticket:

> **"Análise de viabilidade para re-treinamento do algoritmo de previsão de corridas"**

Objetivo: avaliar a necessidade de re-treinar o modelo de previsão do valor de corridas da Desloc, considerando possível degradação de performance (model drift) e mudanças nos dados.

## 🔄 Etapas do pipeline

| Notebook | Etapa (CRISP-DM) | Descrição |
|---|---|---|
| `create_delta_table.ipynb` | Data Understanding | Ingestão do CSV de transações e criação da tabela Delta |
| `00_struct_table.ipynb` | Data Understanding | Estruturação de features (data/hora, distância via haversine, período do dia) |
| `01_descriptive.ipynb` | Data Understanding | Estatística descritiva e inspeção inicial dos dados |
| `02_eda.ipynb` | Data Understanding | Análise exploratória univariada, bivariada e multivariada |
| `03_preparation.ipynb` | Data Preparation | Split treino/teste e pipeline de pré-processamento (`ColumnTransformer` + `MinMaxScaler`) |
| `04_ml.ipynb` | Modeling / Evaluation | Treinamento do modelo (KNN Regressor) e validação por reamostragem (100 iterações) |
| `05_deploy.ipynb` | Deployment | Registro no MLflow, definição de `ModelSignature` e deploy como endpoint REST, com teste real via API |

## 🛠️ Stack técnica

- **Databricks** (notebooks, Delta Lake, Model Serving)
- **PySpark**: processamento distribuído e criação de UDFs (ex: cálculo de distância via haversine)
- **Scikit-learn**: `Pipeline`, `ColumnTransformer`, `MinMaxScaler`, `KNeighborsRegressor`
- **MLflow**: tracking de experimentos, registro de modelo com assinatura, deploy do endpoint
- **Python** (`requests`, `json`): teste do endpoint de inferência em produção

## 🔒 Segurança

O token de acesso à API do Databricks utilizado nos testes de deploy foi **removido do notebook antes do commit**, por boa prática de segurança.

## 📈 Resultado obtido

O modelo (KNN, k=5) atingiu um erro médio (MAE) de aproximadamente **R$ 1,50** e um erro penalizado (RMSE) de aproximadamente **R$ 2,00** na previsão do valor das corridas.

## 🚧 Próximos passos (melhorias planejadas)

Este projeto nasceu como o case prático proposto pelo curso, seguindo a estrutura e as decisões técnicas definidas no material didático. Como evolução, pretendo:

- **Comparar o KNN com outros algoritmos** (Regressão Linear, Árvore de Decisão, Gradient Boosting) para justificar tecnicamente a escolha do modelo final, em vez de usar apenas o KNN sugerido no curso.
- **Revisar a feature `periodo`**, atualmente criada na etapa de estruturação mas descartada no pipeline de treino, decidindo explicitamente se deve ser mantida, substituída ou removida da criação de features.
- **Contextualizar as métricas de erro em relação ao negócio**, comparando o MAE/RMSE obtido com o ticket médio das corridas, para avaliar se a precisão do modelo é de fato adequada para uso em produção.

## 📁 Estrutura

```
├── create_delta_table.ipynb
├── 00_struct_table.ipynb
├── 01_descriptive.ipynb
├── 02_eda.ipynb
├── 03_preparation.ipynb
├── 04_ml.ipynb
└── 05_deploy.ipynb
```

---

Desenvolvido por [Gustavo Sousa](https://github.com/GustavoSousa777) como parte do curso *Ciência de Dados na Plataforma do Databricks*.
