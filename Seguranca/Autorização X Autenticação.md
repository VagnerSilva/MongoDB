##Autentificação
Identifica, verifica, valida o usuário.

## Autorização
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

## **MongoDB-CR**
Utiliza o mesmo modelo de Challenge/Response , ou seja, utiliza-se da autenticação de senha para valida o usuário.
Com a versão 3.0 do MongoDB, foi substituído pelo mecanismo SCRAM-SHA-1; O que significa que a partir dessa versão o mesmo foi **depreciado**, portanto, não é recomendado seu uso em produção.

## **X.509**
E um mecanismo baseado em certificados, implementado na versão 2.6.
Seu uso implica na utilização de conexão [TLS](https://pt.wikipedia.org/wiki/Transport_Layer_Security) obrigatória.
Nas versões a partida da 3.2.6, já se tem suporte nativo ao TLS.

## **LDAP**
O [Lightweigth  Diretory Access Protocolo](https://pt.wikipedia.org/wiki/LDAP) é apenas fornecido na versão Enterprise.
Ele fornece um mecanismo para acessar e manter informações de diretório distribuído numa rede. Sendo frequentemente utilizado por e-mail e programas para procurar informações a partir de um servidor.

## **Kerberos**
Assim como o LDAP,  Kerberos só esta presente na versão Enterprise do MongoDB.
Desenvolvido no MIT ,  [Kerberos](https://pt.wikipedia.org/wiki/Kerberos) é um protocolo  de autenticação e é amplamente aceito por ser bastante seguro seu método de autenticação.
Diferente do LDAP ele é especifico para fins de autenticação , onde o LDAP, de modo especifico,  é mais voltado para armazenamento de metadados sobre o usuário a organização.
Como o LDAP , Kerberos e uma forma de autenticação externa, onde o acesso ao MongoDB ocorre através de credenciais do Kerberos. O que significa que as credencias não são armazenadas no MongoDB

Dessa forma, Podemos nos autenticar no MongoDB, através das credenciais do LDAP, fazendo com que o acesso seja feito de forma externa. Ou seja, as credenciais de acesso não está no cliente MongoDB, como os mecanismos anteriores e sim no servidor de acesso LDAP

## **Autenticação Interna**
Mesmo estando em um rede interna, corporativa, considerada "segura", ainda sim , e importante que as comunicações entre os servidores de replicas ou cluster shared do MongoDB sejam autenticadas.
Para isso o MongoDB, atualmente dá suporte a dois tipos de autenticação interna o **Keyfile**  que usa SCRAM-SHA-1 e o X.509

***Com o Keyfile*** o conteúdo agi como uma senha compartilhada entre os membros de replicas ou cluster shared. 
***A mesma chave deve está presente em cada um dos membros que comunicam entre si.***. Sendo assim, e altamente importante que se mantenha tal arquivo (senha) de maneira segura, onde o mesmo deve ter de no minimo 6 à 1024 caracteres e só pode conter  caracteres no conjunto da **[BASE64](https://pt.wikipedia.org/wiki/Base64)**, onde, também devemos ressaltar que **espaços em brancos são ignorados.**

***X.509***
Se utiliza de certificados para autenticar os membros (replicas set e shardes)
Embora você pode utiliza o certificado para ambos os membros, é altamente recomendado que se utilize um certificado diferente para cada um dos membros.
Dessa forma se um certificado for comprometido, você emite apenas um novo certificado para um único  membro ao em vez de reconfigura todo seu cluster.

É importante dizer que ao utilizamos os mecanismo de autenticação interna de forma automática e ativado a autenticação do cliente
