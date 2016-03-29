# Disco
As de falarmos sobre como planejar a utilização do disco, falaremos de como o MongoDB efetua o alocamento dos arquivos e o pre-alocamento;

####Alocamento
----------

**Pré-alocação**

Por padrão quando inserimos algo  em um banco  de dados e gerado um arquivo de no minimo **64MB**.

Esses  tamanho vai se duplicando até atingir seu tamanho máximo que e de **2048MB** (2 GigaBytes).

![alocação](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/aloca.png)


Porém, esse arquivo de **2GB** sempre está vazio, ou seja, esta pré-alocando, garantindo que esse espaço esteja disponível para quando os dados atingirem essa capacidade.

Assim sendo, quando inserimos dados num banco e atingindo um determinado valor de alocação um arquivo de pré-alocação e gerado.

![pre-alocado](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/pre-aloca.png)

####Planejamento
----------
Com o entendimento da forma que os dados são alocados, agora podemos começa a planejar o gerenciamento de uso do disco,  dessa forma, vamos ao seguinte exemplos.

Temos uma unidade de **100GB**
Considerando que usaremos um conjunto de replicas, samos que  o oplog será gerado e consumira **5% do espaço livre.**
Considerando que o temos **100GB  de espaço livre** teríamos **5GB**  sendo pré-alocado.
