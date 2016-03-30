# Disco
As de falarmos sobre como planejar a utilização do disco, falaremos de como o MongoDB efetua o alocamento dos arquivos e o pre-alocamento;

####Alocamento
----------

**Pré-alocação**

Por padrão quando inserimos algo  em um banco  de dados e gerado um arquivo de no minimo **64MB**.

Esse  tamanho vai se duplicando até atingir seu tamanho máximo que e de **2048MB** (2 GigaBytes).

Subconjunto de dados: ![alocação](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/aloca.png)


Porém, esse arquivo de **2GB** sempre está vazio, ou seja, esta pré-alocando, garantindo que esse espaço esteja disponível para quando os dados atingirem essa capacidade.

Assim sendo, quando inserimos dados num banco, ao alocar esse dado um arquivo de pré-alocação e gerado.

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
Isso por que não mencionamos a utilização do mecanismo de armazenamento **WiredTiger**  e seus métodos de compressão (que iremos ver em outra oportunidade), ou, até mesmo a abdicação do uso do **journal**, utilizando o parâmetro  de configuração **nojournal**, em casos que não precisamos nos preocupar com a durabilidade dos dados,  pois estamos apenas dando alguma ideia de fatores que devemos levar em consideração ao utilizarmos o banco de dados, para auxilio do gerenciamento do espaço em disco.

####Atualizações
----------
Sabemos que ao inserimos um dado no banco, uma arquivo de  pré-alocação e gerado, 

![pre](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/pre.png)

Na movimentação de dados , fisicamente nada esta sendo de fato a locado na pré-alocação, porém se por uns instante alguma alteração, persistir alguma informação no subconjunto pré-alocado ou o documento que esta sendo movimento seja maior do que o espaço pre-alocado, um novo arquivo de pré-alocação será gerado.

![mov](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/mov.png)

Ou seja, uma simples movimentação de documento pode ocasionar um aumento no consumo e vale ressaltar que esse aumento não vai se reverter sem intervenção manual.


####Recuperando espaço em disco
----------
Até aqui vimos como funciona o pré-alocação dos dados no bancos de dados e sabemos que as atualizações, movimentações, dos dados acabam reservando espaços que não estão de fato sendo utilizados.
Então, agora veremos alguns meios de recuperarmos alguns desses espaços.

**Compact Command**

```js
db.runCommand( { compact: <collectionName> } )
```

Ao contrario do que pensamos , **compact** não diminuir o tamanho em disco , caso estejamos utilizando como storage engine o **MMPAv1** , ele apenas desfragmenta e reescreves todos os dados e índices de uma coleção (**collection**).

Desta forma, como o campact efetua desfragmentação
ocasiona na movimentação dos dados, e como sabemos isso pode leva a um aumento,

Já com a utilização do padrão do sistema de armazenamento na versão 3.2, **WiredTiger** o comando irá liberar espaço em disco para o sistema operacional. 
Porém irá reescreve as coleção e índices, sendo útil nos casos de remoção de grande quantidade de dados.


**Repair Command**

```js
db.runCommand( { repairDatabase: 1 } )
```
O **repair** e similar ao processo realizado no padrão **WiredTiger** do **compact** , onde ele reescreve os dados, porém também tenta recupera arquivos corrompidos.

Porém, devemos fazer uma ressalva no processo de reescrita dos dados.
Pois como os dados serão reescritos e necessário que haja a mesma quantidade de dados da base na qual estamos reparando. Assim ele criará um arquivo novo com seu tamanho "correto" e removera a estrutura antiga.

![reoair](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/repair.png)

**Replica Sets**
Em um conjunto de replicas  não temos a necessidade de se preocupa,  se temos 2 vezes o espaço em disco ou não.
  
Pois basta excluirmos os dados de uma **replica segundaria** e **ressincronizarmos**  com a **replica primaria** , fazendo o mesmo procedimento para as demais replicas.
A desvantagem e que ao realizarmos esse procedimento devemos ter em mente que **todos os dados** serão **transferidos pela rede**, com isso,  sabemos que a rede deve ser boa.

####Monitoração
----------
O MongoDB não efetua a monitoração do espaço do disco. 
Sendo assim, cabe a nós efetuarmos tal monitoramento, verificando sempre se a espaço suficiente para que a aplicação consegui inserir novos dados, pois o MongoDB só emitirá alertas quando não houver mais espaço, o que não podemos deixar ocorrer.
Com isso e sempre bom e **fundamental** a  utilização de ferramentas fora do MongoDB, que tenham a capacidade de efetuar a monitoração do espaço em disco e que possa nos avisar de quanto uma determinado porcentagem de espaço foi consumida (no geral 80% da capacidade do disco), para que possamos tomar uma ação preventiva.
