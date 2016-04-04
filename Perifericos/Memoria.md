# Memória

#### Mapeamento de memória
----------

E implementada através do sistema [**MMAP**](https://pt.wikipedia.org/wiki/Mmap), de modo sucinto, ele **mapeai  todos os arquivos em um espaço endereçável na memória virtual**; Sendo assim, **não acessa** esses dados na **memória física** e **sim** os dados que estão **no disco**.
 Dessa forma o MongoDB, por sua vez, acessa esses dados **como se fosse a memória física**, conforme sua necessidade, **consequentemente, carregando e acessa esses dados em memória**.
Resumidamente o mapeamento de memória é realizado dentro da memória virtual.

**MMAP**: 

![mmap](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/mmap.png)



Obviamente essa rotina apenas acontece até a memória física ficar cheia (full)
Quando isso ocorre, temos um novo conceito aqui.



#### Substituição de página
----------
Quando **a memória atingir seu limite**, **e necessário  encontra espaço**, para que o próximo conteúdo ([página](https://pt.wikipedia.org/wiki/Mem%C3%B3ria_paginada)) seja alocado na memoria.
Dessa forma, **e preciso encontra um página para descarte e escreve esse conteúdo de descarte, no disco** para que a nova página seja alocada na memória.

A decisão para escolha da página de descarte e realizada com base no algoritmo **LRU**  (Least Recently Used) , como o nome sugere, e a parte da memoria menos acessada.

#### Memória residente
----------
Basicamente é a parte da memória que não podem ser substituídas e ficam sempre na memória.
O mongoDB trabalha esse conceito através dos seguintes pontos.

**Conjunto de trabalho (Working Set)**:

E a porção dos dados de maior acesso, de uso frequente, pelo cliente [mongod](https://docs.mongodb.org/manual/reference/program/mongod/) ou [mongos](https://docs.mongodb.org/manual/reference/program/mongos/), ou seja, **como parte do nosso working set teremos índices e  um subconjunto de seus dados mediante a frequência de uso**. 
Com isso, esse conjunto de trabalho deve ficar na memória para melhor desempenho, caso contrario, a utilização desses dados no disco, a menos que estivermos usando [SSD](https://pt.wikipedia.org/wiki/SSD), pode causar lentidão.

> A partir da versão 2.4, **podemos configurar o tamaho do nosso working set**, mas você deve se pergunta. 

Então, podemos nos perguntar.
**Qual tamanho devemos definir**, um vezes que, **não pode existe diferença entre o tamanho da memoria residente e do conjunto de trabalho?**

Ter bom conhecimento sobre a base, pelo tamanho médio dos documentos utilizados e índices utilizados, assim como chaves, são os meios nos quais devemos nos basear para definir seu tamanho.

 E não é uma tarefa simples, pois precisamos entender o que estamos realmente usando e o que pode ser atribuído como residente para o processo mongod.
Além disse , estatisticamente, a memória residente pode ser maior do que working set ou menor do que o working set.

O motivo para o qual a memória residente pode vi a ser menor do que o working set e o funcionamento interno do journal.

**Journal** como sabemos tem a função dar durabilidade aos dados no MongoDB e da forma na qual ele funciona **pode impactar** sobre o que e considerado residente na memória para o processo mongod, **pois com journal a quantidade de memória virtual em uso e duplicada.**

Neste caso quando estatisticamente você percebe uma ligeira quebra de memória residente  em relação ao working set, isso e por conta do remapeamento realizado pelo journal, para garantir a durabilidade dos dados.




