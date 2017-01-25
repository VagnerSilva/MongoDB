## **SCRAM-SHA-1** 

Como sabemos o SCRAM-SHA-1 e o **mecanismo padrão de autenticação** do MongoDB.
Seu modo de configuração e bastante simples. Para ativa-lo, basta adiciona o comando --auth, ao iniciar uma instância do MongoDB.

![--auth](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/auth.PNG)

Também é possível  habilitar, tal recurso, através do arquivo de configuração, que utiliza o padrão [YAML](https://pt.wikipedia.org/wiki/YAML),  podendo ser criado por qualquer editor de texto.

![configFile](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/configFile.PNG)

E iniciando uma instância através do comando --conf

![config](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/config.PNG)


Sendo assim, ao acessarmos uma instância , configurada  com o parâmetro de segurança habilitado e tentarmos efetuar algum comando através do shell MongoDB, tal como, show dbs, por exemplo,  teremos o **return code 13, indicamos que não temos autoridade**  de execução do comando.

![test](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/test.PNG)

Porém, **localmente**  temos algumas exceções, dessa forma **podemos criar, `*uma unica vez*`,  um novo usuário**.

![create](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/create.PNG)

E assim, efetuarmos a autenticação pelo comando **db.auth**, caso não  haja nem um problema teremos o **valor  1** indicando que nosso usuário foi autenticado.

![create](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/dbAuth.PNG)
