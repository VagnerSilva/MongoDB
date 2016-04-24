# Segurança

#### Introdução
----------

Um dos itens mais importantes na construção de uma aplicação, um projeto, e a sua segurança. 
Proteger um banco de dados e principalmente trabalhando com dados distribuídos, acaba sendo um tema bastante amplo , pois envolve segurança a nível de sistema, rede, assim como a própria base de dados.

Portanto, neste capitulo veremos como  nos conectar de forma segura no MongoDB.  Com dicas de rede e explicar a importância da segurança do sistema , especialmente no MongoDB.
Também , veremos de modo geral, como você pode criar instâncias seguras, configurações, a utilização do protocolo [*SSL*](www.hostnet.com.br/wiki/index.php/SSL) (Secure Sockets Layer) e suas funcionalidades, onde nos aprofundaremos em um modulo apenas abordando sobre SSL.
Falaremos, também, de alguns conceitos bastantes comuns no requisito de segurança que são:

 - Autentificação
 - Autorização
 - Auditoria

#### Autentificação 
----------
De modo simples, é a identificação de quem você é.
Ou seja, samos testados, mediante a um desafio que tem como objetivo avaliar se você e quem você diz ser. A prática mais básica, desse desafio,  é a utilização de usuário (nome, e-mail, etc...) e senha , sendo autentificado apenas se esses dois requisitos forem validos, ou seja, verdadeiros, sendo assim , confirmando que realmente você é quem diz ser.

#### Autorização
----------
A autorização, de forma sucinta, é responsável por dizer o que você pode fazer, ou, de modo obvio, autorizado a fazer.

#### Auditoria
----------
E uma questão corporativa, usuários comuns não tem acessos a essas informações.
De modo geral a auditoria e o que foi feito, quando foi feito e por quem.
Por exemplo, a verificação se atividade foi realizada de forma correta, se houve permissão para execução, se quem vez tinha permissão para fazer ou estava dentro do processo, etc.
Ou seja, são medicas corporativas para garantir a segurança e qualidade no processo.  

