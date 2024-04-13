### Tipos de Aprendizados de ML

* **Supervisionado**: Aprendizado onde eu sei de antemão a entrada e a saída para cada registro;
* **Não supervisionado:**  Eu não tenho a informação da saída. O modelo busca identificar regularidades e similaridades entre os dados para poder agrupá-los;
* **Semi supervisionado:** combina os dois acima;
* **Por reforço:** O modelo é capaz de perceber o estado do ambiente, realizar ações, avaliar novamente o estado (recompensas, _feedback_ ou reforço) e então realizar novas açoes, se necessário. 
### Aprendizados Supervisionados

 Será o foco da disciplina;

 Aprendizado Supervisionado = **CLASSIFICAÇÃO** e **REGRESSÃO**
	* **Classificação:** a variável de saída é categórica ("Sim/Não", classes, categorias)
	* **Regressão:** a variável de saída é numérica (pode ser contínuo ou discreto)

Modelos são construídos a partir dos dados de entrada (*dataset*), e são apresentados no formato de pares ordenados (entrada-saída desejada)

No aprendizado supervisionado, dizemos que os dados são **ROTULADOS**, pois já se sabe a saída esperada para cada entrada.

Normalmente divide-se o dataset em dois conjuntos: 
* Dados de Treinamento: geralmente 80% do dataset
* Dados de Teste: geralmente 20% do dataset

![[Pasted image 20240303195458.png]]

### Ciclo de um projeto de Dados

![[Pasted image 20240303195652.png]]

Dentro do ciclo de vida de projetos de ciência de dados, a etapa de **modelagem e inferência** – destacada na figura – consiste, de forma simplificada, em escolher o modelo mais adequado para resolver o problema em questão.

Esta etapa envolve 03 passos:

* 1. Elencar os modelos de ML possíveis para o problema em questão
* 2. Estimar os parâmetros para o modelo, com base nas instâncias do dataset e nas variáveis pré-processadas
* 3. Testar e avaliar os resultados de cada modelo, usando métricas de comparação*

Para o aprendizado supervisionado, esta etapa consiste em rodar os algoritmos com uma quantidade grande o suficiente até que ele aprenda uma regra geral de mapeamento entrada->saída. Isto se chama **treinar o modelo**.

### Problemas de Classificação

![[Pasted image 20240303200502.png]]

O ciclo de resolução para um problema de classificação pode ser resumido em 04 etapas:

* 1. Holdout: Divisão do dataset entre bases de treino e teste;
* 2. Treinamento: Submete-se o dataset de treino ao classificador (um algoritmo que trabalha a hipótese de classificação), que é calibrado de acordo com os dados apresentados*
* 3. Teste do modelo: Submete-se a base de teste ao classificador, que agora vai estimar as saídas da base de teste;
* 4. Acurácia: comparação e medição dos resultados apontados pelo modelo vs resultados esperados.

### Overfitting e Underfitting

* Overfitting: modelo se ajusta demais aos dados de treinamento e por isso erra muito nos dados de teste. 
  O modelo acaba "guardando" os padrões de treino e não consegue generalizar/identificar a regra para novos dados.
* Underfitting: é o inverso, quando o modelo não se ajusta bem aos dados de treinamento, logo fica incapaz de fazer previsões para os dados de teste.*

![[Pasted image 20240303201444.png]]

Deve-se equilibrar a escolha de parâmetros, não sendo complexo demais a ponto de gerar overfitting, tampouco simplório demais a ponto de gerar underfitting. Isso é conhecido como **dilema bias x variância**.

Solução para isto: **validação cruzada (*cross-fold validation*)**

![[Pasted image 20240303201903.png]]

### Métricas de Avaliação

Matriz de Confusão:

![[Pasted image 20240303202235.png]]

Acurácia: Valores previstos corretamente dividido por todos os valores previstos:

![[Pasted image 20240303202330.png]]

Precisão:Quantos % o modelo acertou dentro do que ele classificou como Positivo. 

![[Pasted image 20240303202524.png]]

Exemplo: Filtro de Spam (é spam ou não?) -> Preciso de uma precisão alta para não ter muitos casos errados de e-mails não spam como spam (falso positivo). Se isto acontece, eu aumento o falso positivo e diminuo a precisão. Se eu errar classificando um spam como não spam (falso negativo), isso não é tão grave. Não afeta a precisão pois não leva em consideração FN.

Recall: Quantos % o modelo acertou dentre tudo o que era esperado como Positivo.

![[Pasted image 20240303203103.png]]

Exemplo: classificar imagens de doença, como câncer de mama. Não queremos falsos negativos (situação onde há doença mas o modelo diz que não). Reduzir os FN significa aumentar o % de recall.

Na prática, quase sempre temos que escolher entre uma alta precisão ou um alto _recall_. Tipicamente, é impossível ter ambos.

Por fim, a **curva ROC** (_receiver operating characteristic_) contrasta os benefícios de uma classificação correta (TVP, sensibilidade ou _recall_) e o custo de uma classificação incorreta (TFP ou 1-especificidade)

Sensibilidade:

![[Pasted image 20240303204303.png]]

Especificidade:

![[Pasted image 20240303204318.png]]

Utilizamos a curva ROC para calcular a métrica **AUC** (_area under curve_), que varia entre 0 (predições 100% incorretas) e 1 (predições 100% corretas):

![[Pasted image 20240303204349.png]]

### Algoritmos de _machine learning_ para classificação

#### KNN (k-nearest neighbours)

O conceito do KNN é que os pontos vizinhos ao ponto que se quer estimar vai ter a mesma saída.

Assim, o KNN usa métricas de distância entre um ponto e outro para encontrar os pontos mais próximos e assim inferir qual será a sua categoria.

![[Pasted image 20240303204821.png]]

Quando se tem um novo ponto a ser classificado, o KNN compara este registro com todos os demais registros do dataset para identificar os k vizinhos mais próximos.

O valor de k é definido por parâmetro (pode ser 3, 5, 10...)

Limitações:
* Pode ser lento em grandes dataset;
* Sensível a características irrelavantes, já que ele usa todos os atributos para calcular a distância (importância do pré processamento aqui)
* Necessário testar diferentes valores de k e diferentes métricas de distância*

#### Árvores de Classificação

Há diferentes algoritmos para a elaboração de uma Árvore de Decisão, como:  
  
- ID3
- CTree;
- C4.5;
- C5.0;
- CART.

Todos os algoritmos são parecidos: em geral,; a principal distinção está nos processos de **seleção de variáveis, critério de particionamento e critério de parada para o crescimento da árvore**.

Embora sejam intuitivas e interpretáveis, as Árvores de Decisão não costumam ter o mesmo nível de precisão preditiva que alguns outros algoritmos populares de aprendizado de máquina.

Elas tendem a criar limites de decisão excessivamente complicados, resultando em maior variação do modelo, levando a _overfitting_. Para evitar esse problema, podemos usar **algoritmos de poda (_pruning_)** para reduzir a profundidade e a complexidade da árvore.

Exemplo de abordagem de árvore (C4.5), considerando o exemplo de bom-mau pagador:

1. Nó-raiz: Conjunto completo;
2. Divisão entre homens e mulheres;
3. Divisão entre acima/abaixo de 30 anos (ou em outros grupamentos de idade, gerando mais de 2 nós-filhos);
4. Divisão entre acima/abaixo de 1500 reais de renda 
5. Chega-se a um nó-folha cujos registros terão grande predominância da classe desejada (maus pagadores)

![[Pasted image 20240303210110.png]]

#### Naïve-Bayes

Um dos métodos mais utilizados para classificação, pois é rápido de se calcular e necessita de um pequeno conjunto de dados de treinamento.

O Naive vem do fato dele ser _ingênuo_, isto é, desconsidera a correlação dos atributos e trata cada um de forma independente.

O Bayes vem do teorema de Bayes (probabilidade) que é aplicado.

P (H|X) : probabilidade _a posteriori_ de H condicionada a X. "probabilidade de H acontecer, considerando X"

![[Pasted image 20240303210602.png]]

P (H) : probabilidade _a priori_ de H. Não considera X, ou seja: olha pra probabilidade do conjunto como um todo.

![[Pasted image 20240303210656.png]]

P (X|H) : probabilidade _a posteriori_ de X condicionada a H. "Probabilidade de X acontecer, considerando H".

![[Pasted image 20240303210821.png]]

P (X) : probabilidade _a priori_ de X. Novamente, olha para todo o conjunto, independente de H.

![[Pasted image 20240303210909.png]]


Tendo todos estes valores, calcula-se a probabilidade através da fórmula:

![[Pasted image 20240303210954.png]]

Relembrando o exemplo do vídeo:

* Quer se definir a probabilidade de alguém com 35 anos ser mau pagador (MP), isto é: P (MP | 35 anos) *
* P (MP): faz o cálculo da frequencia dos registros entre MP e BP
* P (35 anos): faz o calculo da frequencia de idades através de um histograma
* P (35 anos | MP): **função de densidade** dado o histograma da distribuição idade x maus pagadores; 
* Para a função de densidade, aplica-se uma distribuição normal (mais usual) à curva de idade x maus pagadores, calculando no processo a média e a variância dos dados (tiradas do histograma)
* Por fim, para considerar novos parâmetros no Naive-Bayes, como por exemplo P(MP | 35 anos, 1000 de renda) -> quebra-se os parâmetros em fatores de multiplicação: 

*![[Pasted image 20240303212710.png]]


#### SVM (Support Vector Machine)

Um dos algoritmos mais efetivos para classificação.

O seu treinamento é lento, mas apresentam boa acurácia e baixo overfitting, se comparado aos demais métodos.

A ideia é criar um hiperplano que consiga dividir os dados entre as classes desejadas. Quando se trata de duas dimensões, o hiperplano vira um plano, ou reta:

![[Pasted image 20240303213029.png]]

O SVM trabalha com margem na definição do hiperplano:

![[Pasted image 20240303213117.png]]

Cada possível reta/plano = classificador.

Pontos que tocam a margem = vetores de suporte.

Por construção, todos os vetores de suporte têm a mesma distância em relação à reta do classificador linear (a metade do comprimento da margem).

O classificador associado ao valor máximo de margem é denominado **classificador linear de margem máxima**.

É importante ressaltar que, apesar de o classificador linear de margem máxima ser geralmente bom, pode haver _overfitting_ se o número de dimensões for grande.

Na prática, não dá pra separar 100% com um hiperplano, logo é preciso flexibilizar (classificar soft-margin, que permite alguns registros violarem a linha de separação). 

![[Pasted image 20240303213538.png]]

Pontos destacados = ruído.

Esta rigidez é definida pelo parâmetro de custo C.

Quanto maior for o valor de _C_, mais pontos podem ficar dentro da margem e maior será o erro de classificação, mas menor é a chance de _overfitting_. Se _C = 0_, a margem é rígida e temos o classificador de margem máxima. Na prática, esse classificador não produz bons resultados.

OBS: no scikit-learn o parâmetro C ocorre de forma contrária (C menor, margem maior, mais flexível).

**Quanto maior a folga permitida, maior o número de vetores de suporte e mais lenta a classificação dos dados de teste, pois a complexidade computacional do SVM está relacionada com o número de vetores de suporte.*

O SVM não se limita a modelos de apenas duas classes (Sim/Não, por exemplo). Ele pode ser aplicado em problemas de classificação de múltiplas classes.
Neste cenário, criam-se vários modelos, no qual o primeiro modelo avalia se o registro é da classe desejada (região positiva) ou se é de outras classes (região negativa).

Exemplo: classificar um registro segundo sua cor, sendo que temos 5 cores disponíveis (azul, amarelo, verde, vermelho, branco).

Serão feitos 4 modelos (p-1), no qual o primeiro avalia se o registro é azul ou não. O segundo avalia se é amarelo ou não e assim por diante.

Por fim, uma ilustração da função de kernel do SVM, responsável por subir a dimensão dos dados para conseguir traçar um plano separador (no caso, sai de uma reta em 1D para um plano em 2D):

![[Pasted image 20240303214348.png]]

