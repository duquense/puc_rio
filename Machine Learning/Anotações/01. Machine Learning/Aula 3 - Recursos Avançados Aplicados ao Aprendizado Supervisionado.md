
Ensembles (_comitês_): são métodos que combinam vários modelos de ML em um só. Com isso, se tem um modelo mais efetivo, com melhor desempenho em prever saídas e menor variância/viés/erro.

Podem ser de dois tipos:
* Métodos sequenciais: Os modelos base de ML são gerados e rodados em sequencia, e o _ensemble_ explora a dependência entre eles. Depois ele atribui pesos distintos às saídas com erros para calibrar o desempenho na ponta final da sequência.
* Métodos paralelos: Os modelos são executados ao mesmo tempo, e o _ensemble_ avalia a independência entre eles. A predição final é uma consolidação da predição de cada modelo em paralelo.


#### Métodos Mais Populares

1. ***Voting***: Constrói vários modelos (geralmente de tipos de algoritmos diferentes - _heterogêneo_) e calcula estatísticas como moda ou média para **combinar as predições**;
2. ***Bagging***: Constrói vários modelos (geralmente do mesmo tipo de algoritmo - _homogêneo_) **a partir de diferentes subamostras do conjunto de dados de treinamento;
3. ***Boosting***: Constrói vários modelos (geralmente do mesmo tipo de algoritmo - _homogêneo_) e **cada um deles aprende a corrigir os erros de predição do modelo anterior da sequência.


#### Voting

Voting, como o próprio nome indica, se refere a um sistema de votos.

O método pega 2 ou mais diferentes modelos de IA e os roda. Então, avalia a saída de cada modelo e aquela saída com mais "votos" é a saída do _ensemble_.

Se for um problema de regressão, o _voting_ faz a média entre as saídas dos modelos.

Tem como aplicar pesos ponderados ao voto de cada modelo, o que se chama _stacked aggregation, mas é algo de outro grau de complexidade.

Detalhe: dá pra fazer o _ensemble_ usando o mesmo algoritmo, mas usando diferentes hiperparâmetros para cada versão.

![[Pasted image 20240304223440.png]]

Recomenda-se usar _ensembles_ do tipo _voting_ quando todos os modelos do _ensemble_ tiverem, em geral, boa performance e quando a maioria dos modelos concordar entre si. 

Se qualquer modelo base do _ensemble_ tiver um desempenho melhor que o _ensemble_ em si, esse modelo provavelmente deverá ser usado em vez do _ensemble_.


#### Bagging

O funcionamento dos _ensembles_ do tipo _bagging_ pode ser sumarizado por:  

1. Crie várias subamostras (com aproximadamente 60% do _dataset_ de treinamento original) aleatórias do conjunto de dados de treino, com reposição.  

2. Escolha um algoritmo (por exemplo, árvore de decisão) e treine um modelo de _machine learning_ em cada subamostra.  

3. Dada uma nova instância de dados, calcule a predição do _ensemble_ (para problemas de classificação, a classe mais frequente; para problemas de regressão, a média das saídas de cada modelo individual).

O número de subamostras a serem geradas e, consequentemente, o número de modelos base a serem criados é um parâmetro do _ensemble_, que pode ser escolhido experimentalmente.

**Importante**: esta técnica de estimar uma quantidade a partir de uma amostra de dados pequena se chama **Bootstrap**.

Resumo do exemplo das médias (funcionamento do Bootstrap):
1. Tenho um conjunto grande de números, mas pego um dataset de treino de 100 registros apenas;
2. Ao invés de calcular a média desses 100 registros, vou calcular a média das médias de vários subconjuntos a partir deste de 100 registros;
3. Exemplo: a partir do conjunto de 100 registros, crio 1000 subconjuntos de 30 registros (com substituição - números podem se repetir);
4. Calculo a média de cada um desses 1000 conjuntos e depois faço a média das médias
5. Pronto. A média das médias tende a ser mais acurada que a média simples inicial.

Essa é a lógica por trás do *bagging*: pega um conjunto pequeno de dados, explode ele em um monte de novos subconjuntos, menores ainda, e depois calcula a média do resultado de cada um desses subconjuntos.

Dois exemplos famosos de _bagging_:

* ***Random Forest***: é um passo a mais no _bagging_ tradicional com árvores de decisão. O Random Forest, além de criar subconjuntos menores a partir do conjunto de treino, usa também um subconjunto aleatório dos atributos para cada nó.  Isso resulta em maior diversidade de árvores, geralmente produzindo um _ensemble_ melhor.
* ***Extra Trees*** : extensão do Random Forest. Mas no Extra Trees, não se utiliza as técnicas de subconjuntos/Bootstrap. Usa-se todo o conjunto de dados de treinamento, mas para cada decisão/nó se utiliza atributos e divisões aleatórias. Devido a essa grande aleatoriedade, é um modelo mais rápido computacionalmente do que o Random Forest, pois não é necessário avaliar onde realizar cada divisão.

#### Boosting

Boosting é um _ensemble_ do tipo sequêncial. A sua ideia é rodar um algoritmo, avaliar sua saída, ponderar seus erros e acertos e rodar o mesmo algoritmo novamente, mas desta vez com os dados ponderados.

Esta técnica é repetida em sequencia e vai reduzindo o erro do modelo original.

**AdaBoost** é o algoritmo de _boosting_ mais famoso até aqui. Ele funciona da seguinte maneira:

![[Pasted image 20240304230027.png]]

De forma ilustrativa, vemos como ele vai corrigindo a saída em cada passo:

![[Pasted image 20240304230103.png]]

Repare que inicialmente um modelo é treinado e utilizado para fazer predições no conjunto de treinamento. Em seguida, o peso relativo das instâncias de treinamento que tiveram a predição incorreta é aumentado. Um segundo modelo é treinado usando os pesos atualizados, e as predições são novamente realizadas no conjunto de treinamento. Os pesos das instâncias são atualizados, e assim sucessivamente.

Curiosidade: existe também o método **Gradient Boosting**, e a sua versão otimizada **XGBoost (Extreme Gradient Boosting)**. Se trata de uma variação do AdaBoost que usa uma técnica sofisticada envolvendo gradientes.

### Feature Selection

O conceito de Feature Selection já foi visto antes, mas é bom relembrar. Se trata justamente do método de eliminar atributos irrelevantes para o modelo, ou ainda, selecionar apenas os atributos mais relacionados com a variável target.

Atributos irrelevantes ou apenas parcialmente relevantes podem afetar negativamente o resultado do modelo, especialmente em algoritmos lineares como regressão linear e regressão logística.

Feature Selection é uma etapa de **pré processamento**, logo deve ocorrer **antes** da seleção do modelo.

Vale obserevar também que Feature Selection =/= Redução de Dimensionalidade.

Na Redução de Dimensionalidade o que acontece é a combinação de dois ou mais atributos em um novo atributo, reduzindo assim o número de colunas. São exemplos de métodos de redução de dimensionalidade o _Principal Component Analysis_ (PCA) e _Singular Value Decomposition_ (SVD).

Vantagens de Feature Selection:

1. Reduz Overfitting (menos ruído no dataset)
2. Tendência a um melhor resultado
3. Reduz tempo de treinamento (menos dados)
4. Melhor interpretabilidade do modelo

Os métodos de Feature Selection também podem ser divididos em Supervisionados ou Não Supervisionados, ou seja: se consideram ou não a variável target y para a remoção de atributos.

Abaixo temos 3 tipos de métodos FS:

![[Pasted image 20240305113206.png]]

### Normalização e Padronização

Outro conceito que já foi visto anteriormente.

Também é feito como pré processamento.

Normalização: Transforma os dados do atributo entre 0 e 1.

![[Pasted image 20240305113522.png]]

Padronização: transforma os dados em uma distribuição normal (média 0 e desvio padrão 1. Ex: de -1 a 1.)

![[Pasted image 20240305113533.png]]

Usa-se essas duas técnicas para melhorar a performance dos algortimos de ML, porém essa máxima não vale para todos os tipos de algoritmo. Por exemplo, geralmente os algoritmos baseados em árvores (como árvores de decisão ou Random Forest) não são afetados por diferença na escala dos atributos.

### Pipelines


Pipeline é o conceito de fazer todo o pré-processamento e treinamento do modelo usando apenas os dados de treinamento, sem misturar com os dados de teste (o que seria _data leakage_).

Existe a biblioteca ***pipeline*** no scikit-learn que ajuda a organizar isto.

O objetivo é garantir que todas as etapas do _pipeline_ sejam restritas ao conjunto de dados apropriado (como o conjunto de treino do _holdout_ ou cada _fold_ do procedimento de validação cruzada), permitindo obter uma estimativa justa do desempenho do modelo com dados não vistos.

![[Pasted image 20240305144026.png]]

### Otimização de Hiperparâmetros

Aqui falamos do _tuning_ de cada algoritmo de ML.

A variação dos seus hiperparâmetros pode trazer melhorias para o modelo. Eis os parâmetros que podem ser ajustados para alguns dos modelos estudados:

![[Pasted image 20240305144152.png]]

Além disso, vale lembrar que o scikit-learn também tem uma biblioteca para facilitar a variação dos hiperparâmetros.

No caso, é a funcionalidade ***grid search***, que permite especificar e avaliar de forma mais rápida o impacto dessas alterações.

Para cada combinação de hiperparêmetros especificada no _grid_, a biblioteca irá sistematicamente construir e avaliar um modelo utilizando validação cruzada. Isso nos permitirá escolher, ao final desse processo, a melhor combinação de hiperparâmetros para o algoritmo em questão.