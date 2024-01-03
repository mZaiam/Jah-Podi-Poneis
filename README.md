# Jah'Podi Pôneis - Predicting Bang Gap

## Introdução

O "band gap" é uma propriedade que representa a diferença de energia entre o estado de mais energia da banda de valência e o estado de menos energia da banda de condução em um material isolante, ou semicondutor. Esse valor de energia está relacionado a condutividade do material: quanto menor o valor do band gap, maior é a condutividade do mesmo.

O objetivo deste trabalho é utilizar modelos e estratégias de aprendizado de máquina para prever o band gap de materiais de duas formas distintas:

1. Utilizando como features apenas a proporção molar dos materiais.
2. Utilizando como features a proporção molar multiplicada pelos valores de eletronegatividade na escala de Pauling dos átomos que compõem os materiais.

Esse trabalho utiliza o dataset `expt_gap` da biblioteca `matminer`, que contém os valores experimentais de band gap de diversos materiais semicondutores, sendo essa a variável que desejamos prever. Para isso, treinamos quatro modelos para cada caso: regressão linear e floresta aleatória apenas com normalização padrão, e regressão linear e floresta aleatória com normalização padrão e redução de dimensionalidade com PCA.

## Descrição Geral do Projeto

Inicialmente, queríamos relacionar a fórmula química do material com o seu valor de band gap, uma vez que imaginávamos que os átomos, e a sua proporção, influenciariam nesse valor. Porém, tivémos a ideia de adicionar mais uma informação nessa predição: o valor de eletronegatividade de cada átomo. Essa intuição vem do fato de que átomos mais eletronegativos atraem mais os seus elétrons, e portanto, dificultariam na sua promoção da banda de valência para a banda de condução. Dessa forma, imaginamos que o valor da eletronegatividade dos átomos também influencie na predição do valor do band gap de um material. Para testarmos essa hipótese, criamos dois conjuntos de features: um contendo apenas a proporção molar do material, e outro contendo a proporção molar multiplicado pelo valor de eletronegatividade de cada átomo, e treinamos os mesmos modelos com os dois conjuntos de features, para compararmos os resultados. Existem várias escalas de eletronegatividade diferentes, porém no nosso caso utilizamos a definição de eletronegatividade de Pauling.

Iniciando o processo de tratamento de dados, realizamos um parsing das fórmulas químicas de cada material, para que pudéssemos extrair a proporção molar de cada material das strings que estavam no dataset `expt_gap`. Para isso, utilizamos a função `Composition`, da biblioteca `pymatgen`, e para adicionar posteriormente os valores de eletronegatividade, utilizamos a biblioteca `mendeleev`.

Os modelos selecionados para a predição foram: regressão linear e floresta aleatória. A regressão linear foi escolhida pela hipótese de que talvez houvesse uma relação linear entre os features e o valor de band gap. Já a floresta aleatória foi escolhida por conta de ser um modelo extremamente robusto, por conta de ser formado por diversas árvores de decisão. Talvez o valor de band gap se comporte como regiões mais separadas no espaço dos features, característica que poderia ser captada pelo modelo de floresta aleatória. Em ambos os modelos testamos a redução de dimensionalidade com o PCA, utilizando 90% da variância dos features iniciais.

Todos os modelos utilizaram os features após uma normalização padrão dos features. As florestas aleatórias foram submetidas à otimização de hiperparâmetros por busca aleatória em um espaço de busca pré-definido, por conta do custo computacional de treinamento desses modelos. Para termos um modelo de base para comparação, treinamos um modelo baseline para cada caso.

De forma sintética, o notebook `main.ipynb` está estruturado da seguinte forma:

1. **Importações:** Inicialmente, são importadas as bibliotecas e funções necessárias para o desenvolvimento do trabalho, dentre elas: `matplotlib`, `numpy`, `pymatgen`, `mendeleev`, `matminer` e  `sklearn`.

2. **Tratamento do Dataset:** O dataset inicial é carregado, e linhas com valores `NaN` são descartadas. Em seguida, é realizada uma manipulação nos dados para criar colunas que representam a proporção molar dos elementos presentes em cada material. Isso é feito usando a biblioteca `pymatgen`. Posteriormente, são criadas colunas adicionais multiplicando a proporção molar pelo valor da eletronegatividade dos átomos, utilizando dados da biblioteca `mendeleev`.

3. **Divisão em Casos 1 e 2:** O notebook trabalha com dois conjuntos de features para treinar os modelos de machine learning. O "Caso 1" utiliza apenas a proporção molar dos materiais como features, enquanto o "Caso 2" utiliza a proporção molar multiplicada pelo valor de eletronegatividade.

4. **Discussão dos Resultados:** Ao final do notebook, há uma discussão sobre os resultados obtidos, incluindo uma análise do desempenho de cada modelo nos dois casos e uma reavaliação das hipóteses iniciais.


## Créditos
- O projeto utiliza o dataset `expt_gap`, disponível em ([https://next-gen.materialsproject.org](https://hackingmaterials.lbl.gov/matminer/dataset_summary.html#expt-gap)).


## Agradecimentos

Agradecemos ao professor Daniel Roberto Cassar pela disciplina de Machine Learning. 

## Observations

This was a group project, and this is my edited version. I only translated the notebook to english and changed some minor things. If you're interested in seeing the original project, just check the 
