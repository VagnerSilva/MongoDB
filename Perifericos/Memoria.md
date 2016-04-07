# Memória

#### Mapeamento de memória
----------

E implementada através do sistema [**MMAP**](https://pt.wikipedia.org/wiki/Mmap), de modo sucinto, ele **mapeai  todos os arquivos em um espaço endereçável na memória virtual**; Sendo assim, **não acessa** esses dados na **memória física** e **sim** os dados que estão **no disco** **como se fosse a memória física**. Conforme sua necessidade, **consequentemente, carrega e acessa esses dados na memória**.
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
Basicamente é a parte da memória que não pode ser substituída.
O mongoDB trabalha esse conceito através dos seguintes pontos.

**Conjunto de trabalho (Working Set)**:

E a porção dos dados de maior acesso, de uso frequente pelo cliente [mongod](https://docs.mongodb.org/manual/reference/program/mongod/) ou [mongos](https://docs.mongodb.org/manual/reference/program/mongos/), ou seja, **como parte do nosso working set teremos índices e o subconjunto de seus dados mediante a frequência de uso**. 


Onde esse conjunto de trabalho fica ou deve ficar na memória para melhor desempenho, caso contrario, a utilização desses dados no disco, a menos que estivermos usando [SSD](https://pt.wikipedia.org/wiki/SSD), pode causar lentidão.

Utilizando o comando  [db.stats()](https://docs.mongodb.org/manual/reference/method/db.stats/), podemos chegar ao valor máximo do working set de uma  determinada base somando os seguintes parâmetros

+ **datasize** - Tamanho total da base
+ **IndexSize** - Tamanho total dos índices criados nessa base

Porém, não significa que esse total seja ou esteja de fato ativo.
Além disse , estatisticamente, a memória residente pode ser maior do que working set ou menor do que o working set.

O motivo para o qual a memória residente pode vi a ser menor do que o working set e o funcionamento interno do journal, **pois com journal a quantidade de memória virtual em uso e duplicada.**

Neste caso quando estatisticamente você percebe uma ligeira queda de memória residente em relação ao working set, isso e por conta do remapeamento realizado pelo journal, para garantir a durabilidade dos dados mediante a atualizações e inserções.

Por isso, bom conhecimento sobre a base, índices utilizados e chaves,  são os meios nos quais devemos nos basear para identificarmos possíveis impactos no desempenho.
Assim como, deduzir o quanto de memória e necessário para o bom desempenho da aplicação.

Lembrando que, além da estimativa de uso da memória pelo working set, também, devemos considera as conexões , onde para cada conexão temos o consumo de 1MB podemos ser no máximo 20.000 conexões por instância e, também, quantas instâncias do MongoDB estão sendo executadas no sistema, pois para cada instância o consumo do working set pode variar. 

Ou seja, o conjunto de todos esses fatores nos dará embasamento para que possamos determina se a quantidade de memória física disponível atenderá ou não a necessidade de uma determinada aplicação.


**Processo de restart e limpeza de memória**:

