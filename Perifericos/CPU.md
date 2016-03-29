# CPU
Ao implantarmos uma sistema de banco de dados, muitas das vezes não consideramos alguns fatores que podem ser determinante para aplicação. tal como o consumo da CPU. 
Existem duas categorias que podem impactar no consumo, são elas:

> **User CPU:**  que são os recursos utilizados pelo usuário.

> **System CPU:** que são os recursos consumidos pelo sistema.

A  seguir veremos alguns pontos que podem impactar ou ajudar a mitigar o consumo elevado de processamento, levando-se em conta a  storage engine **MMAPV**.

User CPU
----------

**Consultas:** 
Count, Distinct, Sort,  são bastante utilizados  e, em volumetria de dados muito grandes, podem consumir bastante recurso. Embora não seja novidade, e sempre bom ter em mente que a consulta e umas das que mais afetam o desempenho do CPU do lado do usuário.

**Iterações:**
A iteração, pode ser um vilão ou um herói , se tratando de consumo de processo.
Por exemplo, se efetuarmos um **consulta**  usando recursos de pesquisa tais como ,**distinct** e **count**,  e se **reduzimos sua ranger de iteração** para milhares, ao em vez utilizarmos uma consulta simples que varre milhões de dado, isso pode contribuir para uma melhoria de desempenho de forma bastante significativa.

**Clock Speed:** 
A velocidade de processamento também e um fator importante no desempenho. Portanto e recomenda CPUs com frequência maior ou igual a **2.4GHz** , assim como, modelos  iguais ou melhores que os processadores **i5**.

**Servidor:** 
Recursos utilizados do lado do servidor, tais como **Map-Reduce** e **Intensive Aggregation Framework**, podem potencialmente aumento o consumo do CPU do usuário.

**V8 (JavaScript engine):**
O driver V8, pode ser um aliado bastante interessante, pois trabalha bem o **paralelismo**, além de ser uma opção moderna, podendo contribuir para melhoria de desempenho, em recursos **javascript**, do lado do servidor.

System CPU
----------
###### *não consideraremos recursos de I/O, apenas coisas que resultam nos processos do MongoDB, pois melhorias realizadas no MongoDB não anularão o consumo desses recursos.

**"Scan em grandes partes da memória:"**
A varredura de grandes lotes de tabela e índices, por exemplo, assim como atualizações, de forma frequente, podem consumir porcentagens consideráveis, podendo variar entre 20% a 30% do CPU, não que seja uma problema em alguns casos, mas é algo no qual devemos atentar.

**TCMalloc - Thread-Caching Malloc:**
O sistema de alocação de memória do mongoDB, foi modificado para tcmalloc a partir da verão 2.2, devido a problemas em suas versões anteriores com o sistema de alocação malloc que era utilizado como padrão.

**Considerações formais:**
Quando possível MongoDB usará múltiplos CPUs de processamento, porém, existe algumas funções e padrões que consumirão recursos intensivo da CPU e ocasionalmente estará virgulado a uma unica CPU e nesse casos a velocidade de processamento e muito importante, ou seja, quanto mais melhor. No geral MongoDB tirará proveito de múltiplos processadores, não sendo uma preocupação o aumento de velocidade, mas em casos específicos tal aumento deve ser considerado. 
