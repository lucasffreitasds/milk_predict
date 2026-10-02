# Previsão de Produção de Leite — Milk Predict

**Status: em desenvolvimento.**

Neste projeto, trabalho na previsão da produção futura de leite por animal, utilizando o histórico produtivo e informações de manejo, saúde e ambiente. A proposta é combinar Ciência de Dados e minha formação em Engenharia Agronômica para apoiar o planejamento de uma fazenda leiteira.

O objetivo é responder: **quanto cada animal deve produzir nos próximos dias e como essa previsão pode ajudar na gestão do rebanho?**

[Ver notebook principal](prod_milk_real.ipynb) · [Meu portfólio](https://lucasffreitasds.github.io/portfolio_projetos/)

## Dados

Na etapa principal, utilizo uma **base sintética**, construída para representar uma fazenda de manejo intensivo com vacas Holandesas na Zona da Mata de Minas Gerais.

A base contém **16.560 registros, 180 animais e 92 dias consecutivos**, com informações de todos os animais em cada data. Entre as variáveis estão:

- Produção diária de leite e histórico produtivo.
- Dias em lactação, ordem de parto e condição corporal.
- Consumo de matéria seca e água.
- Frequência de ordenha, ruminação e repouso.
- Temperatura, umidade e condição de saúde.

Atualmente, a preparação dos dados utiliza `Target_7d`, que representa a produção do mesmo animal **sete dias à frente**. Também criei a média dos sete dias anteriores e o desvio entre a produção atual e essa média.

Por ser uma base sintética, os resultados servem para estudo e desenvolvimento da abordagem. A aplicação em uma fazenda depende de validação com dados reais.

## Desenvolvimento

Organizei o trabalho nas seguintes etapas:

1. Ordenação dos registros por animal e data.
2. Criação da variável resposta futura e das variáveis de histórico produtivo.
3. Separação cronológica em treino, validação e teste, com intervalos entre os conjuntos para reduzir o risco de vazamento da resposta futura.
4. Análise exploratória das relações entre produção, manejo, saúde e ambiente.
5. Remoção de variáveis constantes ou redundantes.
6. Codificação de variáveis categóricas, representação cíclica das datas e padronização dos atributos numéricos.
7. Exploração da seleção de variáveis com **Random Forest Regressor, SelectFromModel e Boruta**.
8. Experimentos com **regressão Ridge**, ajuste de hiperparâmetros e avaliação dos erros de previsão.

O repositório também contém uma análise inicial em `prod_milk.ipynb`, realizada com outra base de produção leiteira.

## Resultados preliminares

Os resultados salvos da regressão Ridge correspondem a um **experimento anterior de previsão para 15 dias**, com **2.700 registros de teste**. A preparação atual foi alterada para sete dias, e ainda preciso atualizar as células de modelagem e executar novamente o fluxo completo.

| Métrica | Resultado registrado |
| --- | ---: |
| R² | 0,9179 |
| MAE | 1,25 L |
| RMSE | 1,73 L |
| MAPE | 4,61% |

Nesse experimento, o erro absoluto médio foi de aproximadamente **1,25 litro por previsão**. O RMSE de **1,73 litro** complementa a avaliação, dando maior peso aos erros mais elevados.

Esses números são uma referência inicial. Ainda preciso verificar o desempenho após consolidar o horizonte de previsão e revisar a validação temporal.

## Ferramentas utilizadas

Python, Pandas, NumPy, SciPy, Matplotlib, Seaborn, Scikit-learn, Boruta, OpenPyXL e Jupyter Notebook.

## Como executar

Utilizei **Python 3.13**. Com Python e Git instalados, clone o repositório:

```bash
git clone https://github.com/lucasffreitasds/milk_predict.git
cd milk_predict
```

Se usar Pyenv, ajuste `.python-version` para uma versão ou ambiente instalado na sua máquina. O arquivo atual referencia meu ambiente local, `milk_env`.

Crie e ative um ambiente virtual:

**Linux / macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows — Prompt de Comando:**

```bat
py -3.13 -m venv .venv
.venv\Scripts\activate.bat
```

Instale as dependências e abra o JupyterLab:

```bash
python -m pip install -r requirements.txt
python -m pip install boruta jupyterlab
python -m jupyterlab
```

Abra `prod_milk_real.ipynb` com o diretório de trabalho na raiz do repositório. O carregamento utiliza:

```python
df = pd.read_excel("dataset/dataset_leite_realista.xlsx")
```

As células finais de modelagem ainda precisam dos ajustes descritos abaixo para permitir uma execução completa a partir de um kernel reiniciado.

## Limitações e próximos passos

O projeto está em desenvolvimento. Os principais pontos que ainda preciso revisar são:

- **Integrar a preparação em uma Pipeline:** ajustar a padronização e a seleção de variáveis somente nos dados de treino de cada divisão da validação cruzada.

- **Comparar com previsões simples:** usar a produção atual e a média histórica como referências para avaliar quanto o modelo melhora a previsão.

- **Ampliar a avaliação:** testar outros modelos, analisar os erros por animal e estágio de lactação e validar a abordagem com históricos mais longos e dados reais.

- **Facilitar a reprodução:** completar as dependências e suas versões no `requirements.txt` e atualizar os resultados após uma nova execução completa.

## Autor

**Lucas Ferreira de Freitas** — Cientista de Dados e Engenheiro Agrônomo.

[GitHub](https://github.com/lucasffreitasds) · [Portfólio](https://lucasffreitasds.github.io/portfolio_projetos/)

**Base utilizada:** [dataset sintético de produção leiteira](dataset/dataset_leite_realista.xlsx), com descrição das variáveis e do cenário na aba `Guia`.
