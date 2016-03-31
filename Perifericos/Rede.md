# Rede
Vamos falar de algumas opção para  fazermos melhor uso dos hosts.

####Segregação de recursos
----------
O meio mais simples de segregação e a utilização de vários hosts.
Para a maioria de nós a implementação de coisas tais como [cloud](https://pt.wikipedia.org/wiki/Computa%C3%A7%C3%A3o_em_nuvem), [EC2](https://aws.amazon.com/pt/ec2/),
software entre outros, são as maneiras de segregarmos as coisas.
Grandes servidores tais como Amazon e outros provedores de nuvem, fornecem **maquinas virtuais**, com opções de **configurações**, para que possamos alocar nossos serviços de acordo nossa necessidade.

Sua maior preocupação ao implementar um serviço de banco ao implementar um serviço de banco de dados e o **gargalo** (aqueles 90% de consumo de algum recurso).

Então cabe a nos sempre efetuar um **investigação**, se utilizar de recurso de **benchmarks** para sabe se um determinada configuração vai atender nossa necessidade e qual melhor estratégia para implementação.

Por exemplo, se consideramos que temos um hardware com,

**CPU**: **12** núcleos (**core**)
**RAM**: **512GB**

Quantos processamos  **mongod** poderíamos utilizar com uso de disco de **400GB**, neste **host**?

Um único processo mongod neste caso, teoricamente, não seria um problema, pois estaríamos  consumindo apenas 1 core, e 80% da memória, sem falar do consumo de conexões.
Para cada **mongod** temos o **limite** máximo de **20.000 conexões**.

Mas, ainda sim, temos que consider outros recursos , que podem aumentar o consumo de memoria como o sistema operacional, por exemplo.

Já se optarmos por utilizar **4** núcleos ,limitarmos a conexões em **15.000** e consequentemente usarmos **80GB** de disco, teríamos o seguinte senário.

![Exemplo](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/rec.png)

Visualmente, notamos uma breve redução no uso da memória, o que e sempre uma boa.
Mas, como vemos, agora temos **60.000 conexões** e conseqüentemente devemos nos perguntas. **Temos portas suficientes?**

Ainda , em cima desse exemplo, aumentando o consumo do núcleos para 4, o que não deve ser um problemas, pois ainda sim teríamos 8 núcleos. 

Porém, devemos ter ciência, que o processo **mongod** pode executar rotinas em paralelo, tais como **aggretation** e **map-raduce**, o que pode aumentar pastante o consumo da CPU. Além de nos preocuparmos também, com outros processos que estão sendo executado.

Dessa forma, podemos notar que não e uma das tarefas mais facies implementar recursos em um host, pois temos que sempre nos preocupar com fatores direitos e indiretos , com intuito sempre de evitarmos os temidos **gargalos**.

####Virtualização
----------

A vários soluções aqui como  [Critix](https://www.citrix.com/), 
[VMware](http://www.vmware.com/br), [Openstack](https://pt.wikipedia.org/wiki/Openstack)  , entre outras, basicamente você contrata uma parte do hardware que e gerenciado pela tecnologia [Hypervisor](https://pt.wikipedia.org/wiki/Hipervisor) .

Com isso temos mais flexibilidade para efetuarmos segregações, onde podemos utilizar configurações de hardware bastantes especificas para implementação e até mesmo, movermos a VM para um outro host.

A uma nota que não podemos deixa de citar, e o uso do recurso VMware - **memory ballooning**.

A memory ballooning, basicamente, nos permitir **adicionar mais memoria** ou **remover o que foi adicionado**. Isso facilita em casos que o processo **mongod** necessita demais memória, mas, e importante ressaltar que a **remoção de memoria** pode ser **extremamente prejudicial.**  
Pois pode **ocasionar problemas graves** no MongoDB, como **corromper dados**, portanto, ao executar um processo mongod em uma VMware, **sempre certifique-se que a opção memory ballooning esteja desativada.**

**Arquitetura**:
![VM](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/vm.png)


####Containers
----------
Container e uma alternativa a virtualização, porém, o MongoDB, não e muito testado neste tipo de ambiente.
Uma outra desvantagem e que diferente da virtualização, neste momento,não e possível  move um container para um novo host, mas, muito embora, se espera que no futuro esse recurso seja possível.

**Arquitetura**:
![VM](https://github.com/VagnerSilva/MongoDB/blob/master/Perifericos/imgs/cont.png)
