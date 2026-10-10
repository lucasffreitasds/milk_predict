# Previsão de Produção de Leite — Milk Predict

**Status: concluído — primeira versão de estudo com dados sintéticos.**

Desenvolvi este projeto para avaliar como um modelo de previsão de produção de leite pode apoiar a gestão de uma fazenda. A proposta combina Ciência de Dados e minha formação em Engenharia Agronômica para antecipar a produção esperada e explorar sua aplicação ao planejamento de insumos e custos.

A pergunta principal foi: **como prever a produção de leite para apoiar decisões de gestão com maior segurança?**

[Ver notebook](prod_milk_real.ipynb) · [Meu portfólio](https://lucasffreitasds.github.io/portfolio_projetos/)

## Dados

Utilizei uma base sintética com **16.560 registros, 180 animais e 92 dias consecutivos**, representando uma fazenda de manejo intensivo com vacas Holandesas na Zona da Mata de Minas Gerais.

Os dados incluem produção diária, dias em lactação, ordem de parto, peso, condição corporal, alimentação, comportamento, clima e saúde.

Criei a variável resposta `Target_7d`, correspondente à produção do mesmo animal sete dias depois. Também calculei a média dos sete dias anteriores e o desvio da produção atual em relação a essa média.

## Desenvolvimento

Organizei o trabalho nas seguintes etapas:

1. Inspeção dos dados e ordenação por animal e data.
2. Criação da resposta futura e dos atributos de histórico produtivo.
3. Separação cronológica em treino, validação e teste, com intervalos de sete dias entre os conjuntos.
4. Análise exploratória e avaliação de hipóteses no conjunto de treino.
5. Remoção de atributos constantes ou redundantes, codificação e padronização.
6. Exploração da seleção de variáveis com Random Forest, SelectFromModel e Boruta.
7. Treinamento da regressão Ridge, avaliação na validação e treinamento final com treino e validação reunidos.
8. Avaliação da versão final no teste e aplicação das previsões à simulação de consumo e custo.

A divisão inicial ficou com **9.000 registros de treino, 1.260 de validação e 1.260 de teste**. O treinamento final utilizou **10.260 registros**.

O modelo final utiliza **15 atributos**, com `alpha=0.1`. Random Forest e Boruta foram utilizados na exploração da seleção de variáveis; a previsão final foi realizada com Ridge. A avaliação apresentada utiliza holdout temporal.

## Resultados

| Métrica | Ridge final |
| --- | ---: |
| R² | 0,9176 |
| MAE | 1,25 L |
| RMSE | 1,74 L |

O modelo apresentou um **erro absoluto médio de 1,25 L por previsão**. Cada previsão corresponde à produção diária de um animal, sete dias à frente. O teste contém **1.260 previsões**, distribuídas entre 180 animais e sete dias.

Na semana avaliada, o modelo estimou **36.198,02 L**, contra **36.262,54 L** observados na base: uma diferença de **−0,18%**.

A diferença no total semanal complementa as métricas individuais, mas não as substitui: erros de superestimação e subestimação podem se compensar na soma.

## Aplicação ao negócio

Ao final de uma semana, somo as previsões geradas a partir dos sete dias mais recentes para estimar a produção da semana seguinte.

No teste, utilizei dados de **19 a 25 de março de 2024** para prever a produção de **26 de março a 1º de abril**. Todas as informações utilizadas estavam disponíveis no fechamento da semana de origem.

Explorei uma simulação de consumo de matéria seca e custo de alimentação, combinando produção prevista e peso histórico como aproximação para a semana seguinte.

Com as premissas adotadas, a estimativa foi de **85.957,14 kg de equivalente de silagem e R$ 17.191,43**, uma diferença de **−0,14%** em relação à referência calculada com peso e produção observados.

A simulação estima consumo e custo para apoiar o planejamento. A economia obtida com o uso da ferramenta, seu retorno financeiro e a adequação da dieta não foram avaliados neste estudo.

## Evolução do projeto

Esta primeira versão avalia a previsão com sete dias de antecedência. O objetivo é ampliar gradualmente o horizonte, chegando a **180 dias**, com um novo dataset, histórico mais longo e retreinamento do modelo.

Pretendo avaliar o desempenho em diferentes horizontes e quantificar a incerteza das previsões, buscando uma ferramenta que apoie o planejamento de curto, médio e longo prazo.

A confiança para uso na gestão dependerá de validação com dados reais e da confirmação de que os erros são aceitáveis para as decisões que a ferramenta pretende apoiar.

## Ferramentas utilizadas

Python, Pandas, NumPy, SciPy, Matplotlib, Seaborn, Scikit-learn, Boruta, OpenPyXL e Jupyter Notebook.

## Como executar

Clone o repositório:

```bash
git clone https://github.com/lucasffreitasds/milk_predict.git
cd milk_predict
```

Crie e ative um ambiente virtual.

**Windows — Prompt de Comando:**

```bat
py -3.13 -m venv .venv
.venv\Scripts\activate.bat
```

**Linux / macOS — com Python 3.13 instalado:**

```bash
python3.13 -m venv .venv
source .venv/bin/activate
```

Instale as dependências e abra o JupyterLab:

```bash
python -m pip install -r requirements.txt
python -m pip install boruta jupyterlab
python -m jupyterlab
```

Abra `prod_milk_real.ipynb` e execute as células em ordem, com o diretório de trabalho na raiz do repositório. O dataset está em `dataset/dataset_leite_realista.xlsx`.

Se utilizar Pyenv, ajuste `.python-version` para uma versão ou ambiente disponível na sua máquina.

## Observações e próximos passos

- **Dados sintéticos e histórico curto:** validar a abordagem com dados reais e períodos mais longos antes de utilizá-la em uma fazenda.

- **Horizonte de previsão:** ampliar gradualmente de sete para 180 dias, avaliando o desempenho e a incerteza em cada horizonte.

- **Generalização:** o teste cobre sete datas de origem dos mesmos animais presentes no treino. Ainda preciso avaliar outros períodos, novos animais e outras fazendas.

- **Comparação de modelos:** ampliar os experimentos e incorporar previsões simples, como a produção atual e a média histórica, à avaliação documentada.

- **Preparação dos dados:** organizar as transformações, a seleção de atributos e o modelo em uma Pipeline para facilitar a reprodução e a geração de novas previsões.

- **Aplicação alimentar:** fundamentar a equação de consumo, avaliar a aproximação dos pesos futuros e considerar separadamente silagem e concentrado na composição da dieta.

## Autor

**Lucas Ferreira de Freitas** — Cientista de Dados e Engenheiro Agrônomo.

[GitHub](https://github.com/lucasffreitasds) · [Portfólio](https://lucasffreitasds.github.io/portfolio_projetos/)

**Base utilizada:** [dataset sintético de produção leiteira](dataset/dataset_leite_realista.xlsx), com descrição do cenário e das variáveis na aba `Guia`.