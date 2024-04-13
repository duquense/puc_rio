Relembrando: problemas de REGRESSÃO -> saída numérica, podendo ser contínuo ou discreto.

CLASSIFICAÇÃO -> "Conceder ou não crédito para o cliente?"
REGRESSÃO -> "Conceder qual valor de crédito para o cliente?"

Para a Regressão, aplica-se os mesmos conceitos de preparação da base de dados, separação entre datasets de treino e teste, definição dos critérios de parda do algoritmo e etapas de treinamento e teste.

A principal diferença entre Regressão e Classificação, além do tipo de saída, é que na Regressão, ao invés de calcular acurácia, estima-se a distância (erro) entre a saída do modelo e a saída esperada.

Exemplo visto de um problema de regressão: Em qual bairro abrir a nova loja do grupo?

* Tem-se o faturamento das lojas já abertas;
* Levanta-se dados (variáveis explicativas) sobre as lojas já abertas: população do bairro, IDH, renda per capta, número de concorrentes no bairro, etc;
* Levanta-se também as mesmas variáveis explicativas para os bairros candidatos (bairros que ainda não sabemos qual seria o faturamento, nossa variável de saída do problema);
* Treinamos o modelo com os dados levantados no 2º passo
* Aplicamos e testamos o modelo nos dados desconhecidos (3º passo)
* Ao final, teremos uma estimativa calculada para o faturamento de cada bairro candidato, e podemos então escolher pelo o que tem o maior valor previsto*

### Métricas de Avaliação

O x da questão aqui é sempre buscar quantificar o erro do modelo, isto é, a diferença entre a saída proposta pelo modelo vs a saída esperada:

![[Pasted image 20240303231753.png]]

Uma das métricas mais usadas é a MSE, _mean squared error_. Tem também a RMSE, _root mean squared error_, que passa uma raiz para manter o erro na mesma unidade do _target_.

![[Pasted image 20240303232002.png]]
![[Pasted image 20240303232013.png]]

Temos também o coeficiente de determinação R² (_R-square_), que mostra o quanto que _y_ é explicada por _x_. Quanto mais próxima de 1, melhor o modelo.

![[Pasted image 20240303232148.png]]

### Algoritmos de ML para Regressão


#### Árvore de Decisão

Árvore de decisão = termo genérico. Pode ser de classificação ou de regressão, quando voltadas para problemas deste último tipo.

Algoritmo CART = _classification and regression tree_

Muito semelhante às Árvores de Classificação (voltadas para problemas de Classificação). 
A diferença é que na árvore de regressão a predição é a média dos valores de cada nó-folha.

A construção é feita de forma similar: começa pelo nó raiz e vai dividindo em grupos mais homogêneos (divisão e conquista).

Para classificação, a homogeneidade se mede pela entropia; na regressão, usa-se estatísticas como variância, desvio padrão ou desvio absoluto da média.

Para regressão, um bastante utilizado é a redução do desvio padrão (SDR - _standard deviation reduction_). 

Um exemplo abaixo de como funciona a árvore de regressão:

![[Pasted image 20240303234203.png]]


#### KNN (_K-NEAREST NEIGHBOURS_)

O KNN também pode ser usado para a regressão.

Neste caso, ao invés de pegar a classe predominante dos K vizinhos mais próximos, faz-se a média aritmética dos seus valores _target_. 

#### SVM (_SUPPORT VECTOR MACHINE_)

O SVM para regressão também pode ser chamado de SVR (_support vector regression_). 

Apesar disso, ele é pouco utilizado para a regressão, pois existem modelos mais simples que apresentam resultados semelhantes ou melhores.

#### Regressão Linear

Regressão linear simples é o modelo clássico: traçar uma reta que melhor se ajuste aos pontos dispersos nos eixos x-y.

A melhor reta vai ser aquela que apresenta o menor SQE, ou seja, o menor somatório de erros. Lembrando que o somatório de erros vai ser a soma da distância (erro) de cada ponto até a reta traçada pelo modelo. Importante considerar aqui que a soma dos erros é quadrática, para desconsiderar valores positivos/negativos.

A regressão linear múltipla é quando se acrescenta mais uma variável ao plano. Tendo agora um cenário xyz, onde queremos prever z a partir de x e y, traçaremos um plano ao invés de uma reta. A medida do erro acontece de forma similar, verificando a distância dos pontos originais para o plano otimizado.

Para encontrar os coeficientes pra a reta de regressão, utiliza-se o Método dos Mínimos Quadrados (Ordinary Least Squares).

Curiosidade: Comparado ao modelo de regressão linear, o SVR tem a vantagem de utilizar uma grande variedade de funções que se adequam aos modelos. Na regressão linear, tentamos minimizar a taxa de erro, enquanto, no SVR, tentamos ajustar o erro dentro de um determinado limite, definido pela margem.

#### Métodos de regularização

São técnicas que se aplicam como complemento ao modelo linear, buscando reduzir o erro (SQE) e também reduzir a complexidade do modelo (uso de função de penalidade).

* Regularização Ridge: Busca minimizar a soma dos quadrados dos coeficientes (_aka_ regularização L2).
* Regularização Lasso: Busca minimizar a soma do valor absoluto dos coeficientes (_aka_ regularização L1).

O parâmetro de ajuste λ serve para controlar o **impacto da penalidade**.

Quando _λ_ = 0, o termo da penalidade não tem efeito e o resultado é similar ao método dos mínimos quadrados da regressão linear.

Quanto maior λ, maior o impacto da penalidade e maior a diminuição dos coeficientes.

A Ridge não permite zerar os coeficientes, mesmo com um λ alto. Assim, ele não consegue remover outliers, mas é capaz de aprender padrões complexos (mantém todas as variáveis preditoras).

Já a Lasso permite zerar coeficientes com um λ alto. Isto ajuda a simplificar o modelo e evitar o ruído de outliers, porém o modelo não consegue aprender padrões mais complexos.

#### Regressão Logística

Por fim, temos a regressão logística, que na verdade é utilizada para problemas de classificação.

É uma regressão que retorna saídas 0 ou 1. 

Internamente, a regressão logística **calcula a probabilidade de ocorrência (p) de um evento**, ou seja, seus valores de saída estão entre 0 e 1, e essa saída é mapeada na classe correspondente.

![[Pasted image 20240304004112.png]]

Ao invés do método de mínimos quadrados (regressão linear), a regressão logística usa o método de **estimação de máxima verossimilhança** para determinar os coeficientes.

Os melhores coeficientes vão ser aqueles que resultem num modelo em que as saídas vão ficar muito próximas de 1 para a classe padrão e muito próximas de 0 quando for para a outra classe.
