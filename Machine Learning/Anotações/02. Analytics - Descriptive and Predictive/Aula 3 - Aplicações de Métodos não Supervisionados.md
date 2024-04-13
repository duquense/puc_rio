
Relembrando: Modelos não supervisionados são aqueles em que não se tem dados rotulados, isto é: não se sabe de antemão qual deve ser a saída.

Os problemas clássicos de métodos não supervisionados envolvem a clusterização (agrupamento de dados próximos).

Quanto mais próximos (semelhantes) forem um par de dados, maior a chance deles receberem o mesmo rótulo.

Para definir isso, usa-se métricas de similaridade ou dissimilaridade. Quanto mais parecidos dois objetos, maior a similaridade e menor a dissimilaridade.

![[Pasted image 20240408181427.png]]

Daí se tem também as etapas do proccesso de aprendizagem não supervisionada. Por exemplo:
- Seleção de atributos: deve-se selecionar adequadamente, para evitar ruídos e informações não relevantes;
- Medida de proximidade: importante usar dados normalizados pra que um não pese sobre os demais;
- Critério de agrupamento (cluster compacto? ou alongado?)
- Algoritmo de agrupamento
- Verificação dos resultados
- Interpretação dos resultados

## Dimensionalidade

Um dos aspectos mais importantes. Através dela é possível reduzir a complexidade das amostras e melhorar a performance dos modelos (+ resultados com - esforços).

- Seleção de atributos: criar um subconjunto dentre as colunas disponíveis. É realmente escolher as que importam mais. Mantém os dados originais.
- Redução de dimensionalidade: reduz o número de colunas através de uma operação que leva em consideração os valores de cada coluna. Faz um bem bolado, ao invés de simplesmente descartar. Não mantém os dados originais.

![[Pasted image 20240408185606.png]]

Exemplo de redução: objeto sai de 3D (x,y,z) e a gora é representado no plano 2D. São agora criados novos pontos (x,y).

A redução é importante, dentre outros, porque facilita o aprendizado do modelo. Muitos atributos podem levar a overfitting, à dificuldade de aprendizado. É a chamada maldição da dimensionalidade.

## Remoção de atributos

O nome já indica, é a remoção simples e direta. Pode ser feito em casos onde o atributo não é relevante para o modelo, ou é redundante.

Ex: dígitos finais do CEP.

## Redução de atributos

Aqui já entra a complexidade de reduzir colunas considerando a contribuição de todas as originais.

Deve-se manter um bom equilíbrio entre a redução do tamanho, que cria uma representação mais compacta, e uma alta simplificação, que gera perda de informação relevante.

### Análise de Componentes Principais - PCA

Um dos principais métodos de redução de dimensionalidade.

A idéia é traçar componentes (vetores) que consigam explicar o comportamento dos dados no espaço. No exemplo abaixo, os 2 componentes reduzem a dim de 3 pra 2:

![[Pasted image 20240408190328.png]]

- O eixo 1 é o principal e contém a maior variancia entre pontos;
- Depois vem o eixo 2, com a 2a maior variância, daí o eixo 3... e assim por diante.
- A covariância entre pares de eixos é zero - eles não são correlacionados.

### MDS - Escalonamento Multidimensional

O escalonamento multidimensional (MDS – _multidimensional scaling_) representa as dissimilaridades como distâncias entre pontos em um espaço de baixa dimensão (normalmente, duas dimensões), de modo que as distâncias correspondam o mais próximo possível às dissimilaridades.

Equivalente à PCA com dois componentes principais, quando usamos a distância euclidiana.

É comum utilizarmos o MDS como método de análise e visualização para identificarmos características em comum entre os dados. Assim, **identificamos visualmente as dissimilaridades entre pares de objetos**.

### Agrupamento

É a clusterização. Algoritmo mais famoso é o _k-means_. Busca desvendar padrões existentes nos dados e os classifica de acordo com sua similiaridade.

Existem três categorias principais de algoritmos de clusterização:  
  
- Sequenciais (baseados em centroides).
- Hierárquicos.
- Baseados em densidade.

### K-means

Baseado em centróides. O algoritmo do _k-means_ é bem simples e segue o exemplo abaixo:

(1) Selecione **k** centroides iniciais.

(2) Forme **k** _clusters_, associando cada exemplo ao seu centroide mais próximo.

(3) Recalcule a posição dos centroides com base no centro de gravidade do _cluster_.

(4) Repita os passos 2 e 3 até que os centroides não sejam mais movimentados.

![[Pasted image 20240408191118.png]]
![[Pasted image 20240408191131.png]]
![[Pasted image 20240408191141.png]]![[Pasted image 20240408191151.png]]

### Clusterização hierárquica - HCA

Pode ser feita de duas formas: Aglomerativo e Divisivo.

- Aglomerativo: começa de um único dado e o transforma em cluster; daí a cada rodada ele vai agrupando novos dados adjacentes e aumentando o cluster.
- Divisivo: é o contrário. Começa considerando todo o conjunto de dados como um único cluster, daí vai quebrando em novos clusters.

o HCA é útil para fazer clusters dentro de clusters, numa estrutura de árvore. É onde se tem os dendogramas.

![[Pasted image 20240408191637.png]]

![[Pasted image 20240408191659.png]]

![[Pasted image 20240408191706.png]]


### DBSCAN

_Density-based spatial clustering of applications with noise_. É um método baseado em densidade.

Métodos baseados em densidade muito resistentes à presença de ruídos e baseados no conceito de _ε_-vizinhança. 

Seja um espaço bidimensional, a _ε_-vizinhança de um ponto _p_ é o conjunto de pontos contidos em um círculo de raio _ε_, centrado em _p_.

Algortimo do DBSCAN: Selecione um ponto aleatoriamente que não tenha sido atribuído a nenhum _cluster_ e que não seja um _outlier_:

1. Calcule sua _ε_-vizinhança e determine se ele é um _core point_.
	- Se sim, comece um _cluster_ ao redor desse ponto.
	- Se não, rotule como _outlier_.
1. Depois de ter encontrado o _core point_, expanda-o adicionando todos os pontos alcançáveis.
2. Repita os dois passos acima, até que todos os pontos sejam atribuídos a um _cluster_ ou rotulados como _outliers_.