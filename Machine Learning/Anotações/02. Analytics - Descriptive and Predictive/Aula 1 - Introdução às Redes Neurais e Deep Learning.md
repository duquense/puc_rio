
O cérebro tem neurônios para a tomada de decisões. Neurônios ligam e desligam na ordem de milissegundos.
Já um computador tem os transistores dentro dos microprocessadores que alternam em nanossegundos.

![[Pasted image 20240407205939.png]]

Sinapse: neurônio receber o sinal (sinapse) por uma ponta (dendrito). Quando o sinal é forte o suficiente (acima de 50mV), ele é propagado pelo corpo do neurônio (axônio) até sua outra ponta.

As sinapses podem ser **excitatórias** (elevam o sinal pro poximo neurônio) ou **inibitórias** (reduzem o sinal ao próximo neurônio)

Assim, a idéia das Redes Neurais é criar um modelo matemático para simular o comportamento de um neurônio (ou vários).

### Neurônio Artificial

O modelo básico propoe algo como: tem-se uma entrada X, essa entrada vai ser multiplicada por um peso W, que é o equivalente ao peso sináptico de excitação ou inibição do sinal. Soma-se, ainda, uma variável constante para calibrar o modelo. 

A saída Z é um valor que significa o resultado do input. Se o peso for excitatorio, logo Z > X. Do contrário, temos um sinal reduzido, Z < X.

Considerando múltiplos sinais de entrada (afinal, neurônios processam diversos impulsos ao mesmo tempo), exapande-se o modelo para matrizes, nas quais cada impulso tem seu peso correspondente e o resultado final é o somatório do produto entrada * peso sináptico:

![[Pasted image 20240407211509.png]]

Os pesos sinápticos são calculados durante o treinamento, em um processo de de aprendizado supervisionado (sabemos a resposta, calibramos os pesos para chegar perto dela).

Na sequencia temos o conceito do aprendizado hebbiano. Para a aplicação nas redes neurais, o que importa é calcular a diferença entre o valor que se espera e o valor que o modelo apresentou. Durante o treinamento do modelo, busca-se convergir esse delta até que a diverença se aproxime de 0.

![[Pasted image 20240407211944.png]]

É aí que começa a entrar o _backpropagation_. Pensa, você calcula o delta, a diferença entre a saída e a saída esperada. Daí esse delta volta somando para a matriz de peso W:

![[Pasted image 20240407212202.png]]

O n é a taxa de aprendizado, é um valor pequeno entre 0.1 e 0.0001 para definir a velocidade de convergência. Não se usa o delta (a diferença de erro) inteira pra não gerar instabilidade no modelo. É como se fosse atualizando os pesos aos poucos.

### Perceptrons

A parada com os perceptrons é que ele passa o modelo proposto acima pra função _sign_, que é a função de sinal: ou sai 0, ou 1 (ou então -1 e 1, a depender da aplicação).

A importância das funções de ativação é essa: decidir se o valor computado pelo neurônio é suficiente para dispará-lo ou não.

Mas os perceptrons não se provaram tão eficientes assim, pois só eram capazes de lidar com problemas lineares. Na vida real, busca-se resolver problemas não lineares.

Solução: inserir uma camada não linear no perceptron. Daí surge o MLP: Multi Layer Perceptron. Ele tem uma camada de entrada, camada de saída e a "camada oculta", onde está a parte não linear.

Por definição, as MLP são redes que contêm uma (ou mais) camada(s) oculta(s).

A partir deste ponto várias arquiteturas começaram a ser propostas para as redes neurais, começando pelas _feed-forward networks_.

### Feed Forward Networks

O nome já diz, alimentação direta. Entra o sinal em uma ponta, passa pelas camadas e sai com o resultado na outra.

![[Pasted image 20240407213140.png]]

As camadas são hiperconectadas: cada neurônio de uma camada se conecta aos demais da camada seguinte.

Recapitulando, a rede tem que ter:
	- Camada de entrada;
	- Ao menos uma camada intermediária não linear;
	- Função de ativação em cada camada (pode ser feitas combinações de ativações aqui)

Para as funções de ativação, tem a _sign_ mas tem várias outras também. Cada uma atende melhor para um tipo de problema específico (regressão, reconstrução de imagem, etc).

## Funções de Ativação

![[Pasted image 20240407213957.png]]
![[Pasted image 20240407214010.png]]



![[Pasted image 20240407214033.png]]
![[Pasted image 20240407214050.png]]

Além dessas, tem a famosa função ReLU, que é uma família de **funções retificadoras**.
Na ReLU clássica, temos uma função linear se x>0, e nulo quando x<0. 

![[Pasted image 20240407214254.png]]

Daí se tem algumas variações, que buscam suavizar a ReLU tradicional:

![[Pasted image 20240407214532.png]]
![[Pasted image 20240407214544.png]]


E tem também a função SOFTMAX, presente na camada de saída de quase todas as redes de classificação, pois representa uma distribuição de probabilidade discreta.

![[Pasted image 20240407214820.png]]


## Backpropagation

Backpropagation é o conceito de pegar o erro da saída e voltar o delta pra dentro do modelo, corrigindo os pesos da entrada.

Usa o modelo de descida do gradiente (gradiente descendente). 

![[Pasted image 20240407215021.png]]

O backpropagation não é um algoritmo otimizado, o que significa que nem sempre garante a melhor resposta. Isto se deve ao fato dele começar com pesos aleatórios, o que pode 'travar' a descida do gradiente em algum ponto.

Daí existem outros algoritmos que também fazem essa redistribuição de pesos, como o ADAM: adaptivem moment estimation.

## Deep Learning

Redes Neurais Profundas, ou Deep Learning, nada mais são que as redes MLP vistas anteriormente, mas com mais camadas ocultas. Isto está em alta agora devido à maior capacidade de processamento.

![[Pasted image 20240407220325.png]]

Um ponto abordado aqui é sobre como trabalhar imagens dentro de uma rede neural. Até então, se tratou de redes neurais _horizontais_, ou seja, trabalhadas no plano x e y (pontos no plano). 

Mas quando se trata de imagens, temos altura, largura e cor. Se a imagem for preto e branco beleza, pois a parte de cor vira apenas um parâmetro de tons de cinza, entre preto e branco (0 e 1).

Mas e para imagens coloridas? Aí entram tres canais de cores, o RGB. 
Pensando assim, as DNNs são redes cujas camadas são estruturas tridimensionais (larg. × alt. × canais), diferentemente das MLPs clássicas, que são apenas camadas de uma única dimensão horizontal totalmente conectadas.


## CNNs - Redes Neurais Convolucionais

São um tipo de rede neural profunda _feedforward_ com camadas que não são hiperconectadas (nem todos os neurônios de uma camada se conectam com os da seguinte).

As CNNs são baseadas em camadas que aplicam a operação de convolução (quadradinho que varre a figura). 
As camadas convolucionais são normalmente aplicadas a entradas bidimensionais (matrizes que representam imagens -> vai varrendo a imagem).
Esse 'quadradinho' é o filtro, ou _kernel_.

![[Pasted image 20240407221229.png]]

![[Pasted image 20240407221332.png]]

Resulta em 1 porque:
1x0 + 2x1 + 1x0 = 2
0x1 + 1x(-4) + 1x2 = -2
2x0 + 1x1 +1x0 = 1

Somatório Final: 2 + (-2) +1 = 1

Daí algumas características da convolução:

- Padding: Quando um _kernel_ é deslocado sobre uma imagem e chega ao final de uma dimensão, existem três possibilidades: _valid padding, same padding_ ou zero _padding_.
![[Pasted image 20240407221851.png]]

- Stride: é como o kernel se desliza pela imagem/matriz.
![[Pasted image 20240407221925.png]]

- Pooling: O _pooling_ também é chamado de camada de “subamostragem”. É uma camada não treinável que só serve para **reduzir a complexidade quando o número de convoluções é muito alto**. -> Redução dos pontos de entrada (pixels em uma imagem)
![[Pasted image 20240407222108.png]]
Veja que reduz cada matriz 4x4 em um ponto único na matriz da direita.

Arquiteturas de Convolução Famosas:

- LeNet
- AlexNet
- GoogLenet/Inception
- ReNet (Microsoft)
