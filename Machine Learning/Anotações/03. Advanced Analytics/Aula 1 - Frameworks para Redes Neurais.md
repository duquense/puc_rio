## TensorFlow x PyTorch

Dois dos frameworks mais famosos para a aplicação de redes neurais e _machine learning_. 

O TensorFlow tem uma curva de aprendizado maior e é um pouco mais complexo, mas permite ajustes mais finos, maior parametrização.

O PyTorch é um pouco mais amigável para iniciantes, devido à familiaridade com Python. Porém, oferece menos customização dos modelos.

Mas hoje o TensorFlow já conta oficialmente com a API / lib do Keras, que por ser de alto nível de linguagem entrega um pouco mais de facilidade para a construção dos modelos.

## Arquitetura

A Arquitetura de ambos é baseada em grafos. 

No TF (TensorFlow), os dados de entrada são transformados em **tensores**, que são matrizes de altas dimensões. Daí esses tensores fluem por entre os nós (camadas) da rede neural.

No início, os grafos do TF eram estáticos. Isso significa que todas as variáveis da rede eram definidas ao início da execução e não eram mais alteradas, mesmo com as iterações do treinamento.

QUando se fala em grafos dinâmicos, acontece o inverso: as variáveis são redefinidas a cada iteração. 

A importância disso se dá por exemplo no caso de processamento de linguagem natural: os dados de entrada serão de tamanhos variados, logo se a variável de entrada for estática, a aplicação não funcionará bem.

Contudo, grafos dinâmicos vão exigir mais poder de processamento (_tradeoff_ de perfomance)

## Debugging

O PyTorch permite ferramentas tradicionais de debugging, algumas utilizadas também em Python, como pdb ou ipdb.

No TF isso é feito de outra forma, com uma ferramenta específica chamada tfdbg. O TF não aceita ferramentas de debugging tradicionais.

## Deploy

O TF apresenta uma ferramenta mais robusta de deploy, que é o TensorFlow Serving.

É uma ferramenta já validada pro mercado, voltada para ambientes de produção industrial. Tem recursos para otimizar desempenho e gerenciar custos associados ao deploy. Além disso, pode se integrar a outras plataformas como Kubernetes e Docker.

Já o PyTorch usa o TorchServe para deploy. É uma ferramenta mais simples e ainda em estágio experimental, ao contrário do TensorFlow Serving.

Ambos os frameworks permitem deploy em ambientes mobile também.

No caso do TF, é preciso usar o TF Lite, que é um outro framework. Deve-se tomar cuidado pois nem todos os modelos e funções do TensorFLow podem ser convertidos ou existem no TF Lite.

Já o PyTorch tem um _workflow mobile_ que usa a mesma base de código do PyTorch. Trocando em miúdos, você consegue desenvolver um modelo no PyTorch normalmente e depois salvar e carregar com o PyTorch Mobile Lite Interpreter.


## Redes Neurais de Convolução (CNN)

CNNs são as redes neurais que usam convolução no seu modelo. É aquela ideia do quadradinho 16x16 que vai varrendo (fazendo a convolução) uma imagem maior de 1024x1024, por exemplo.

CNNs estão muito associadas a classificação e detecção de objetos (Visão COmputacional).

### Detecção de Objetos

A detecção de objetos funciona combinando dois conceitos:

- Classificação: identifica a classe de um objeto na imagem. 
- Localização: Identifica se há um objeto na imagem.

![[Pasted image 20240404221203.png]]
![[Pasted image 20240404221214.png]]![[Pasted image 20240404221225.png]]

## Principais Modelos de Detecção de Imagens

### YOLO

Acrônimo para You Only Look Once. Baseada na LeNet, um modelo mais pesado. 

São 24 cadas de onvolução + 2 camadas totalmente conectadas.

Forma de detecção:
1. Divisão da imagem num grid S x S;
2. cada celula do grid define pelo menos uma bounding box e um score, que é o que de fato vai falar se ali tem ou não um objeto. **Esse _score_ de confiança reflete o quão confiante o modelo está de que a _bounding box_ contém um objeto.**
3. Se não há um objeto nessa box, o score é zero;
4. cada bounding box tem 5 atributos: x e y (centros da box); altura e largura; e score (representa a confiança/probabilidade comparado ao dado treinado. Ex: 78% de ser um cachorro)

![[Pasted image 20240404222656.png]]
### MobileNet

A MobileNet é voltada para dispositivos móveis, logo é um modelo de rede neural mais simples (menos parâmetros) que outrso modelos, tipo LeNet. Demanda tb menos processamento.

O custo computacional pode ser reduzido de 8 a 9 vezes.

Para detecção de objetos, a MobileNet utiliza uma arquitetura chamada SSD (_Single Shot Detection_). Ao usar a SSD, precisamos de apenas uma única imagem para detectar vários objetos e não particionar a imagem em múltiplas partes menores, enquanto abordagens clássicas, como a R-CNN, precisam de duas imagens pelo menos, uma para gerar probabilidades de uma região conter objetos e outra para detectar o objeto de cada região proposta.

## Segmentação de Objetos

A detecção apenas gera a bounding box em torno da classe dos objetos, mas não traz informação sobre a forma de cada um.

Aí que entra a segmentação: ela cria máscaras dos objetos. É a sombrinha de cada animal nos exemplos abaixo:

![[Pasted image 20240404223420.png]]
![[Pasted image 20240404223435.png]]

A segmentação é útil para casos na vida real, como por exemplo:
	- Detecção facial
	- Análise de raiox X
	- Carros automatizados (detecção de faixa, por ex)

![[Pasted image 20240404223540.png]]
![[Pasted image 20240404223602.png]]
![[Pasted image 20240404223614.png]]


### Redes de convolução para segmentação: UNET

A rede neural de convolução UNET é uma rede totalmente convolucional, isto é, não apresenta em sua saída camadas totalmente conectadas. Normalmente, a sua saída nos dá um mapa de “calor” para a posição de cada objeto detectado na imagem, e após calculamos de fato a _label_ de cada pixel. A UNET é chamada assim devido à sua arquitetura, realizando sucessivas operações de convolução em duas partes.  
  

- A primeira etapa é chamada de _encoder_ (também chamado de “contração”), que é usada para capturar o contexto na imagem. O _encoder_ é apenas uma pilha tradicional de camadas de convolução seguidas de _maxpooling_.

- A segunda etapa é chamada de _decoder_ (também chamada de “camada de expansão”), que é usada para permitir a localização precisa usando convoluções transpostas. Ela é uma das arquiteturas mais usadas para segmentação semântica, em especial para análise de imagens médicas.

### Vision Transformers

Em linhas gerais, se trata de um modelo novo desenvolvido e bastante eficaz para a detecção de imagens. 

Pode ser considerado o estado da arte.

Porém, essa arquitetura é muito mais difícil de treinar, e seu desempenho depende muito dos hiperparâmetros escolhidos, do conjunto de dados e da profundidade da rede.

Em comparação com demais modelos já consolidados, o Vision Transforme até supera, mas por uma margem pequena. Logo, em algumas situações a complexidade do modelo pode não valer a performance.

