
Agora vamos além das redes feed forward. Começamos pelas Redes Neurais Recorrentes (RNN), que introduzem o resultado da saída de volta no modelo. Uma espécie de feedback/retroalimentação.

![[Pasted image 20240407232326.png]]
Nas feed forward, as saídas dependem apenas da amostra de entrada e do processamento feito pelos _perceptrons_. Elas são adequadas à maioria dos problemas em que a entrada é pontualmente processada pela rede, resultando em uma saída. 

Porém, existem problemas em que o processamento do sinal deve considerar o histórico dos valores de entrada.
Veja as séries temporais: cada dado é ordenado em função do tempo e sofre influência de eventos anteriores. Exemplos: temperatura, áudio, vídeo, etc.

Nos _perceptrons_ das RNNs, existe uma sinapse que recebe a previsão da rodada anterior e a adiciona à nova saída linear. O valor resultante é transformado pela função de ativação, para produzir a nova saída real.

![[Pasted image 20240407232503.png]]


Quando se trata de séries temporais como dados de entrada, consideramos as entradas da RNN como janelas de tempo. 
A entrada que recebe essas janelas se chama desdobramento no tempo e é uma sequência de valores, um em cada passo de tempo (Desdobramento ou unfolding).

Nesse caso, usa-se o Backpropagation pelo tempo.
O **algoritmo BPTT** é a extensão do _backpropagation_-padrão para redes com desdobramento. Ele visa **computar as saídas** da RNN e **determinar o valor** da função de custo para cada rede.


## Redes LSTM

A rede LSTM (_long short term memory_) é análoga ao comportamento das memórias de curto e longo prazo cerebrais.

Aqui se introduz os conceitos de adicionar ao modelo portas que simulam a memória de uma informação. A porta pode ficar aberta ou fechada (0 ou 1), o que indica se aquela info fica na memória ou não.
Neste exemplo, a porta é o elemento, e o 0 ou 1 é o estado da porta.

- Memória de curto prazo: contexto do trabalho;
- Memória de longo prazo: aprendizado adquirido;

A LSTM é adequada quando há intervalos muito longos de tamanho desconhecido entre eventos importantes.

Durante o treinamento, uma rede LSTM pode aprender quando deve manter ou descartar informações.

 Os portões são gerenciados por funções sigmoides, enquanto as ativações são baseadas em tangentes hiperbólicas, cuja simetria garante melhores desempenhos.
![[Pasted image 20240407233730.png]]

Há três portões em um bloco LSTM:  
  

- Forget gate (Esquecimento)
	- O portão de esquecimento é responsável pela **persistência dos elementos** de memória existentes ou pela sua eliminação.
	- Se o portão de esquecimento emitir um valor próximo a 1, o elemento correspondente ainda é considerado válido.
	- Valores mais baixos -> célula vai levando o valor até zero, quando ela é removida completamente

- Memory gate (input gate - memória)
	- Define o quanto (%) da amostra de entrada deve ser usada para atualizar o estado (saída).
	- É tipo... vou usar muito da memória já adquirida ou não?
	- Redefine o contexto C do aprendizado;
	- O novo contexto é calculado pela soma do resultado do portão de esquecimento com o resultado do portão de memória

	Logo, a LSTM se trata de um equilibrio entre as saídas anteriores e sua reavaliação, considerando o quanto se deve esquecer e o quanto se deve adicionar ao contexto atual. 

- Output gate (Saída)
	- **controla as informações** que devem transitar do estado para a unidade de saída;
	- O portão de saída é controlado pelo contexto atual, fornecido pela combinação dos dois portões anteriores.

Estas características conferem às LSTM uma espécie de operações de leitura, gravação e reset de dados.

![[Pasted image 20240407235123.png]]

## Autoencoders

Muito usados em problemas de redução de dimensionalidade. Isto é importante pois menos dimensões: menor complexidade, menor ruído -> mais eficácia.

![[Pasted image 20240407235213.png]]

Um _autoencoder_ genérico é um modelo dividido em dois componentes separados: _encoder_ e _decoder_. Eles não funcionam completamente autônomos e são treinados juntos.

- Encoder (codificador): transforma a entrada em um vetor codificado. Reduz a info (Compressão).
- Decoder (decodificador): reconstroi a info original. Restaura o tamanho da info (Descompressão).  

![[Pasted image 20240407235605.png]]

- Denoising autoencoders: Uma caracteristica importante é que os autoencoders podem trabalhar com ruídos ou dados incompletos. Pode-se inclusive treina-los adicionando erros manualmente (ruído gaussiano).
  
  Dessa forma, o codificador se torna robusto e aprende a codificar as amostras, mesmo quando a informação foi incompleta ou ruidosa.

- Variational autoencoders: estado da arte de autoencoders. Seu objetivo não é encontrar uma representação codificada de um conjunto de dados, mas determinar os parâmetros de um processo gerador que seja capaz de produzir todas as saídas possíveis a partir dos dados de entrada.

### GANs - Redes Adversárias Generativas

Arquitetura de duas redes neurais que competem entre si. Um gerador e um discriminador. 

O gerador tenta enganar o discriminador gerando amostras o mais fiéis possíveis aos dados reais; o discriminador tem que acertar o que é real ou fake.

Uma rede puxa o desenvolvimento da outra.

![[Pasted image 20240408000352.png]]

Primeiro se treina o gerador, e o discriminador vai avaliando a probabilidade das infos serem reais ou não, dá notas por exemplo de 1 a 100;
A cada feedback, o gerador busca corrigir seus parametros e aumentar sua nota;
Quando ele atinge uma probabilidade bem alta pelo discriminador, significa que está bom o bastante;
Daí vira a vez de treinar o discriminador. Vamos colocando os dados reais pra ele entender melhor e melhorar a percepção do que é fake ou não.

Pra redes convolucionais (tipo as de treino de imagens), tem-se a DCGAN, Deep Convolutional GAN.

