### Reconhecimento Facial

Identificar padrões e conformidades nas faces das pessoas de acordo com _features_ presentes em uma base de dados.

Normalmente, o reconhecimento facial não depende de um enorme banco de dados de fotos para determinar a identidade de um indivíduo – ele simplesmente identifica e reconhece uma pessoa como a única proprietária do dispositivo.

Além desse tipo de aplicação simples (desbloquear telefone), o reconhecimento facial pode ser usado em aplicações mais complexas, como serviços de segurança (identificar suspeitos). Porém, há de se observar aqui a questão ética da coisa (deve se um modelo muito preciso para evitar falsos positivos).

![[Pasted image 20240404231649.png]]![[Pasted image 20240404231715.png]]![[Pasted image 20240404231728.png]]
![[Pasted image 20240404231749.png]]

_Faceprint_: a "impressão digital" do nosso rosto.

Utilizações frequentes:
	- _Banking_: mais uma camada de segurança
	- Aeroportos: mais agilidade, mais segurança
	- Desbloqueio de telefones (atenção com privacidade dos dados)
	- Governo: autenticação de documentos (gov.br)
	- Redes sociais (tagging de pessoas)

### Questões éticas

Além das já faladas questões sobre excesso de vigilância e falsos positivos (um modelo sempre está sujeito a falhas), há outro ponto interessante:
- Para rodar esses modelos, é necessário um grande volume de dados. Muitas empresas não vão ter toda essa infra, apenas as gigantes. E aí, como faz? Fica dependente dessas grandes empresas? Como lidar com o armazenamento de tantos dados?


### FaceNet

Famoso sistema de reconhecimento facial. Proposto pela Google.

Esse sistema recebe um _batch_ de imagens e, após o processamento pela rede neural, gera um vetor de _features_ para os dados de entrada com uma normalização L2 aplicada sobre ele.

![[Pasted image 20240404233538.png]]
![[Pasted image 20240404233423.png]]


## Liveness Detection

Técnicas utilizadas para detectar que uma imagem é real, é de fato um humano (e não algo falso, como deepfake ou fotografias).

Existem técnicas ativas e passivas:
- Ativas: envolve que o usuário execute alguma ação a mais, ou dupla autenticação;
- Passivas: usa algoritmos de ML apenas para a detecção.

As tentativas de falsificação podem ser detectadas por meio de algoritmos de reconhecimento de padrões, que analisam artefatos como variações anormais de pixels na imagem, ou ainda com a ajuda de sensores de profundidade e modelos que conseguem realizar uma análise da geometria 3D da face.

Há trabalhos que propõem métodos de _liveness detection_ baseados na estrutura 3D da face. (iPhone tem sensor de profundidade, consegue detectar um ataque de apresentação 2D - fotografias).

### Deepfakes

_Deepfakes_ são imagens, vídeos ou áudio gerados artificialmente por computador com o uso de técnicas de _deep learning_, cujo objetivo é, da maneira mais realista possível, fingir ações de uma pessoa real.

Deepfakes são feitos geralmente de duas formas: **com o uso de _autoencoders_ e com redes neurais generativas adversárias**.

- Autoencoder:
![[Pasted image 20240404235154.png]]

1. Dois modelos são treinados;
2. O primeiro modelo usa imagens da pessoa que se quer fazer o fake;
3. o segundo modelo é com as suas próprias imagens, com as expressões e gestos que se quer imitar
4. Após treinados os modelos, passo o resultado do encoder da segunda rede (a com as minhas expressões) como entrada para o decoder da primeira rede
5. Isso resulta em uma imagem da primeira pessoa, mas com as expressões da segunda.

### Tipos de deepfakes em imagens

- Face swapping: usar as expressões faciais de uma pessoa B para fazê-la passar-se pela pessoa A.
- Body puppetry: transferir movimentos e gestos e todo o corpo de uma pessoa A para uma B, cujas imagens foram geradas artificialmente.

### Detecção de deepfakes

É um grande desafio, nem sempre é possível. Mas existem algumas _features_ típicas de _deepfake_, como, por exemplo, :

- um rosto perfeitamente simétrico, óculos tortos ou com distorções nas armações, dois brincos diferentes ou algo parecido.
- Os movimentos dos lábios parecem reais? Eles correspondem ao texto falado e estão em sincronia?
- A pessoa pisca com frequência incomum?


### Esteganografia

Esteganografia é a técnica de inserir mensagens escondidas em imagens, vídeos ou outros arquivos.

O objetivo é esconder mensagens em imagens, já que quase ninguém conhece ou imagina que exista essa técnica.

Em imagens, uma das técnicas clássicas é chamada de **representação pelo bit menos significativo.** No JPEG por exemplo é feita a transformação DCT de blocos de pixels em coeficientes. Os bits menos importantes desses coeficientes podem ocultar mensagens. Ou seja, a ideia é usar uma espécie de ruído estruturado a esses coeficientes para armazenar informação.

![[Pasted image 20240405001805.png]]

![[Pasted image 20240405002045.png]]

Técnicas como o StegaStamp trazem a oportunidade de expandir aplicações de esteganografia para imagens e documentos impressos. Um dos possíveis desdobramentos de técnicas como esta é a sua utilização para validação documentos, em que a mensagem escondida pode vir a ser um dado criptografado ou um _hash_.

Para validar a veracidade do documento, podemos utilizar um decodificador, que, a partir de uma fotografia tirada com qualquer dispositivo, seja um telefone, câmera ou _scanner_, pode recuperar essa informação e fazer o _matching_ com uma base de dados. Esta é apenas uma de muitas implicações dessa tecnologia, que está em pleno desenvolvimento.


## _Pose estimation_

Outro grande desafio para a visão computacional.
Normalmente, o problema a ser abordado envolve estimar a pose humana 2D, ou seja, os _keypoints_ anatômicos ou “partes” do corpo de pessoas em imagens ou vídeos.

Hoje, os modelos de processamento de imagem mais poderosos são baseados em redes neurais convolucionais (CNNs) e _vision transformers_. Assim, praticamente todos os projetos de _pose estimation_ são baseados em uma ou em uma mistura dessas duas arquiteturas.

![[Pasted image 20240405002704.png]]
![[Pasted image 20240405002820.png]]


- Abordagem Top Down: define os pontos das articulações, daí agrupa as imagens. É mais custoso, mas mais eficiente tb.
![[Pasted image 20240405003141.png]]
- Bottom Up: Faz uma boundin box primeiro, define a pessoa. Depois estima as articulações contidas ali.
![[Pasted image 20240405003238.png]]

 Alguns métodos populares de estimativa de pose humana 2D incluem OpenPose, CPN, AlphaPose e HRNe.

A estimativa de pose humana 3D pode ser realizada em imagens ou vídeos monoculares (câmera normal). Usando múltiplas câmeras para estimar a profundidade ou sensores adicionais (IMU ou LiDAR), a estimativa de pose 3D pode ser aplicada com técnicas de fusão de informações. Embora os conjuntos de dados humanos 2D possam ser facilmente obtidos, é demorado coletar anotações precisas de imagens de pose 3D, e a anotação manual não é prática.

Uma biblioteca popular que usa redes neurais para estimativa de pose humana 3D em tempo real, mesmo para casos de uso de várias pessoas, é o OpenPose.

### _Tracking_ de pose em vídeos

Difícil de fazer, devido a motivos como:
	- Oclusão (uma pessoa na frente da outra, ou objeto tampando);
	- Víde blur;
	- coerência temporal (garantir que o esqueleto se mantenha correto ao longo dos frames)

Usa-se redes neurais recorrentes (RNN) para mitigar isso, além de CNNs (convolução).

### Estimativa da Pose

Busca-se obter os keypoints da pessoa ou objeto. No caso, as articulações (cotovelos, joelhos, pulsos, etc). 

Pode-se fazer estimativas Multipose (detecta poses para várias pessoas de uma vez) ou Pose única (só um indivíduo).

Ex: um modelo retorna 17 _keypoints_ de um individuo. Cada _keypoint_ é anotado com três números (x, y, v), em que x e y marcam as coordenadas, e v indica se o _keypoint_ está visível ou sofre de oclusão.

![[Pasted image 20240405004310.png]]

## Modelos para estimativa de pose

- OpenPose
Bottom-up. Uma vantagem do OpenPose é que ele é uma API que oferece aos usuários a flexibilidade de selecionar a origem das imagens a serem analisadas, sejam vídeos, imagens ou _streaming_ de câmeras.

- AlphaPose
Concorrente do OpenPose. Arquitetura eficiente para tempo real. Usa top-down, ao invés de bottom-up

- PoseNet
Bottm-Up. Feito para dispositivos leves (navegadores) ou móveis.

- DensePose
Modelo feito pelo Facebook. Gera modelos 3D para filmes, animações e realidade aumentada.


### Aplicações em Retail

Em varejo, a análise do comportamento do cliente é crucial para melhorar a experiência e a eficiência operacional. 
Uma aplicação é usar visão computacional para estimar o fluxo de clientes, criar mapas de calor na loja e detectar áreas de maior interesse. Isso é feito com câmeras simples e algoritmos de detecção de pessoas, armazenando dados para análise estatística.

Resumo Chat GPT:
  
Entender o que os clientes fazem nas lojas é importante para os vendedores. Eles podem usar câmeras e computadores para ver quantas pessoas estão lá e onde elas estão indo. Isso ajuda a decidir onde colocar os produtos para que as pessoas os vejam mais facilmente.

Com câmeras, os vendedores podem ver as áreas movimentadas da loja. Isso os ajuda a entender o que as pessoas gostam e onde elas gostam de ir. Dessa forma, podem organizar a loja de maneira mais inteligente.

Então, os vendedores podem colocar os produtos nos lugares certos para atrair mais pessoas. Isso faz com que mais pessoas comprem, o que é bom para os negócios e para quem está comprando.

