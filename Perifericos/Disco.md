# Disco
As de falarmos sobre como planejar a utilização do disco, falaremos de como o MongoDB efetua o alocamento dos arquivos e o pre-alocamento;

####Alocamento
----------

**Pré-alocação**

Por padrão quando inserimos algo  em um banco  de dados e gerado um arquivo de no minimo **64MB**.

Esse  tamanho vai se duplicando até atingir seu tamanho máximo que e de **2048MB** (2 GigaBytes).

![alocação](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/aloca.png)


Porém, esse arquivo de **2GB** sempre está vazio, ou seja, esta pré-alocando, garantindo que esse espaço esteja disponível para quando os dados atingirem essa capacidade.

Assim sendo, quando inserimos dados num banco e atingindo um determinado valor de alocação um arquivo de pré-alocação e gerado.

![pre-alocado](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/pre-aloca.png)

**Journal**
Cujo a função básica e garantir a durabilidade dos dados, também, se utiliza da pré-alocação.
No caso do mecanismo de armazenamento **MMPAV1**, são **3** arquivos de **1GB** , no máximo, e no **WiredTiger** **128MB**. 
>>(falaremos mais sobre MMPAV1 e WiredTiger no topico de configurações)

**Oplog**
Basicamente utilizado para recuperação dos dados, ao utilizarmos um conjuntos de **replica set**, um arquivo de **oplog** e gerado, onde por padrão consome **5% do espaço livre** do disco, porém, pode ter o valor especificado.

####Conceitos formais
----------
Com o entendimento de como o banco trabalha sua alocação de dados, vamos ao seguinte exemplo.

Considerando que temos  uma unidade de **100GB**, onde usaremos um conjunto de replicas.

 Sendo assim, sabemos que  o oplog será gerado e consumira **5% do espaço livre.**
Considerando que temos **100GB  de espaço livre**, desta forma, teríamos **5GB**   de espaço sendo pré-alocado.
Vejamos na image o consumo do disco numa suposta inicialização, onde optamos pela engine **MMPAv1**.

![ex1](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/ex1.png)

Na imagem, podemos perceber que numa simples inicialização facilmente consumimos  aproximados **8,20%** do espaço disponível, porém o MongoDB nos da opções para atenuarmos esse consumo.

Podemos, por exemplo, configura o **oplog** para consumir **1GB**,
configura o journal , para utilizar  a opção de **smallfiles** que consome **384MB**, no máximo, em seguida, a atribuição  inicial e fatorada por 4, passando de **64MB** para **16MB** e sua pré-alocação de **128MB** para **32MB**, o que e bastente significativo

![ex2](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/ex2.png)

A imagem nos mostra bem o ganho de espaço com apenas configurações simples. 
Isso por que não mencionamos a utilização do mecanismo de armazenamento **WiredTiger**  e seus métodos de compressão, pois estamos apenas dando alguma ideia de fatores que devemos levar em consideração ao utilizarmos o banco de dados.
