####Autentificação
Identifica, verifica, valida o usuário.

#### Autorização
Verifica os privilégios do usuário.

Mecanismo de Autentificação 
-------------
Basicamente no MongoDB e divido em duas partes, categorias.

**Cliente/Autenticação do usuário**, cujo é a forma que cliente autentica no MongoDB , E, posteriormente, **autenticação interna** , cujo a forma que diferentes membros de uma replica definem ou compartilham a autenticação com o outro. 
Sendo assim, temos os seguintes mecanismos de autenticação

> **Cliente/Autenticação do usuário:**
:  Versão de Comunidade
>  
  > **Challange/Response (Pergunta e responta)**
  >> SCRAM-SHA-1
  > > MongoDB-CR 
  > 
>  **Autenticação via Certificado**
> >  X.509
: Versão para Enterprise
> **Autenticação Externa**
>>   LDAP
> Kerberos


>  **Autenticação interna:**
>> - KeyFile(SCRAM-SHA-1)
> - x.509


## **SCRAM-SHA-1**

Um dos mecanismo de autenticação padrão do MongoDB, que utiliza o modelo Challenge/Response, no caso, usuário e senha.
SCRAM-SHA-1 um [IETF](https://pt.wikipedia.org/wiki/Internet_Engineering_Task_Force) padrão, que define os métodos recomendados para implementação do mecanismo de Challenge/Response para autenticar o usuário com senha.

SCRAM-SHA-1 , protege sua aplicação dos seguintes ataques:

> - [Eavesdropping](https://pt.wikipedia.org/wiki/Eavesdropping) - Pode ser lido todo o tráfego trocando entre o cliente e o servidor. Onde para se proteger uma cliente nunca deve enviar uma senha como texto simples através da rede. 
> - [Replay](https://en.wikipedia.org/wiki/Replay_attack) - Pode ser reenviado do cliente respostas validas ao servidor.
> - Database Compromise - Onde pode ser lido o contéudo da memória persistida no servidor, neste caso, SCRAM-SHA-1, hash as senhas antes de armazená-las. Sendo assim, um invasor não é capaz de passar por um servidor sem o conhecimento de uma credencial valida, o que mitiga nosso próximo item de ataques

> - Servidores Maliciosos
