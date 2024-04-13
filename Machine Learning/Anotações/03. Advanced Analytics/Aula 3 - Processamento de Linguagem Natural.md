PLN (ou NLP em inglês, _Natural Language Processing_) é a área de IA que analisa e tenta compreender a forma (padrões) como os humanos se expressam (texto e fala).

- Lingaugem natural: inglês, português. Evoluem com o tempo e são difíceis de descrever por regras.
- Linguagem artificial: linguagens de programação, notações matemáticas.

Exemplos de PLN:
- Previsão de textos;
- Traduções;

O Processamento de Linguagem Natural já passou por 3 vertentes:
- **Racionalista**: Tentativa de estruturar tudo em regras bem definidas. Porém, como a linguagem é mutável, fica impossível de colocar tudo em regras;
- **Empírica**: Já usa um conjunto de dados para tentar extrair _features_. Necessita de um especialista humano para validar essas _features_ extraídas. Era um modelo mais utilizado para sistemas nichados, de domínio próprio, e não generalistas;
- **Redes Neurais**: É a vertente moderna de PLN. Aprendizado com base de grandes volumes de dados. Não vai precisar de humanos para fazer a extração de _features_, mas precisa ainda da validação humana.

-> Lib de Python para PLN: **Natural Languange Toolkit (NLTK)**

## Limpeza de Dados

Cita a importância de remover caracteres especiais, como símbolos, vírgulas, e outros caracteres de pontuação.

Mas frisa também que, a depender do contexto, é importante manter, como no caso fianceiro ($ e , ).

Uso de REGEX para a limpeza.

## Pré Processamento de Dados

Cita a importância de jogar tudo para minúsculo: isto porque senão os modelos interpretam Brasil, brasil e BRASIL como 3 palavras diferentes;

Faz a _tokenização_: quebra o texto em _tokens_, ou seja uma lista de frases/palavras (ao invés de um único bloco de texto).

- Stop-words: palavras irrelevantes. "E", "ou", "o". Algumas libs já tem isso pronto, mas cada caso é um caso. Podemos acrescentar outras palavras à essa lista para diminuir o ruído.

![[Pasted image 20240405181355.png]]

- Stemming: redução de palavras para a sua raiz. "Ajudando, ajudaram" -> "Ajuda".

![[Pasted image 20240405181531.png]]

- FreqDist: function que conta a frequencia de cada palavra:

![[Pasted image 20240405181639.png]]

- N-grams: combinação de palavras usadas juntas. N-gram de N=1 é unigrama; N=2 é bigrama, N=3 é trigrama e assim por diante.

## Bag of Words

Após feitas as etapas de pré processamento, é preciso transformar isso nas entradas para os algoritmos. Cria-se então a Bag of Words.

É feita uma codificação de palavras para números (vetorização), e se conta quantas vezes a palavra aparece numa frase. 

-> Term Frequency: quantas vezes a palavra aparece num documento / total de palavras do documento

-> Inverse term frequency: O log do número de documentos dividido pelo número de documentos que contêm a palavra “w”. A frequência de dados inversa determina o peso de palavras raras em todos os documentos do _corpus_.

-> POS tagging: classificação da classe gramatical do termo. Importante para tradução automática, por ex.

-> Dependency parsing: método para descobrir as dependências entre termos de uma sentença. Constrói-se uma árvore com as relações entre palavras de uma sentença, que pode ser usada depois em algoritmos.

![[Pasted image 20240405184925.png]]

Setas são as dependências; caixas em azul são as tags.

![[Pasted image 20240405185016.png]]


## Word Embedding

**_Word embeddings_ é uma técnica em que palavras são transformadas em uma representação na forma de um vetor numérico**.

Isso é necessário pq algoritmos funcionam melhor com números do que texto bruto.

Técnica utilizada: ***one hot encoding***

![[Pasted image 20240405185222.png]]

Colocando as palavras em vetores também dá pra fazer operações algébricas.

O Word2Vec faz isso para descobrir palavras semanticamente proximas, como REI e RAINHA.

![[Pasted image 20240405185428.png]]

Na sequencia ele explica de forma complexa como o Word2vec funciona. Isto pq, como visto acima, os vetores não estão como one hot encoding.

Mas na prática, o que acontece é assim: treina-se o modelo com um vetor onde só a palavra desejada recebe 1, as demais 0. Então imagina que se tem uma base de 10000 palavras e quer saber qual a próxima palavra para 'carro'. Tem também o passo da janelinha que corre o texto vendo as palavras antes e depois de cada termo.

Daí roda essa rede neural que vai entregar um novo vetor com a probabilidade de ser cada uma das 10000 palavras do modelo. Provavelmente 'rápido' vai ter maior probabilidade que 'colchão', por exemplo.

### Doc2Vec

Semelhante ao modelo anterior, mas agora a gente representa não só algumas frases como número, mas documentos (independente do tamanho).

![[Pasted image 20240405190411.png]]

O modelo do Doc2Vec tem duas variantes, mas a que normalmente tem melhores resultados é uma extensão do **Continuous Bag of Words (CBOW)**. Esse algoritmo tenta essencialmente prever uma palavra a partir de uma lista de palavras de contexto. Isto é, dada uma amostra de palavras, ele tenta prever qual palavra do nosso _corpus_ tem a maior probabilidade de estar associada a essa lista.

A intuição por trás desse modelo é simples: dada uma frase 'eu adoro _data science_', escolheremos nossa palavra-alvo como “adoro” e uma lista de palavras relacionadas, como [“eu”, _“data_”, “_science_”]. O que esse modelo fará é pegar as representações distribuídas das palavras de contexto para tentar prever a palavra-alvo “adoro”.

No Doc2Vec, em vez de usarmos apenas palavras para prever a próxima palavra, também adicionamos outro vetor de _features_, que é exclusivo para cada documento. Assim, ao treinar os vetores de palavras, o vetor de documento D também é treinado, e, ao final do treinamento, temos da mesma forma uma representação densa do documento.

- O Doc2Vec pode ter aplicações práticas no campo jurídico:
	- A comparação de documentos pode identificar/apontar documentos similares;
	- Isto pode por exemplo sugerir para os profissionais da área processos com histórico semelhante (mesmo contexto, mesmas palavras presentes);
	- Daí ajuda na análise da jurisprudência (o como proceder)

## RNNs - Redes Neurais Recorrentes


Uma forma usual para processamento de texto.

![[Pasted image 20240405191300.png]]

Algumas limitações das RNN:
- Só captam dependências em uma direção; na RNN, a palavra que vem depois não tem efeito sobre o significado da palavra atual(o que nem sempre é verdade);
- Não captura dependência longa; Frases que estão muito distantes, ainda que no mesmo contexto, por exemplo.
- Vanishing gradient: quando o cálculo do gradiente se torna muito pequeno e trava a rede, impedindo a alteraçõs dos pesos e travando o aprendizado da rede. Para contornar isso, ele apresenta o conceito de GRU - gated recurrent unit




