## **X.509**

Inicialmente é importante ressaltar que a autenticação   por via do  certificado x.509 **requer um conexão [TLS](https://pt.wikipedia.org/wiki/Transport_Layer_Security)** segura.

Para identificar se a versão que estamos a utilizar da suporte ao protocolo TLS , basta digitamos no terminal o comando **mongod --version**

![openssl](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/openssl.PNG)

Sendo possível ser visualizado a versão do [OpenSSL](https://pt.wikipedia.org/wiki/OpenSSL) , concluímos que tal versão oferece suporte ao protocolo TLS. Sendo assim, podemos utilizar o mecanismo de autenticação X.509.

E se estamos falando de OpenSSL, obviamente, estamos falando de certificados, nos quais  são responsáveis pela autenticação do cliente ao servidor, dessa forma não necessitamos da utilização de um usuário e senha como no modelo SCRAM-SHA-1.


### **Pontos importantes**
------------------
Para produção deve-se utilizar certificados validos **assinados por uma unica [Autoridade de Certificação](https://pt.wikipedia.org/wiki/Autoridade_de_certifica%C3%A7%C3%A3o)**  (***CA - Certificate Authority***). Podendo ser gerado e mantido por você, sua empresa, até mesmo ser utilizado certificados fornecidos por terceiros.

A Autoridade de Certificação (**CA**),  **deve, obrigatoriamente, ser utilizada na geração dos cerificados clientes e servidor(es).**
Onde **cada instância do MongoDB deve conter um certificado exclusivo.**



***

 - ***Pré-requisito - Certificado Membro - Servidor***
 
 1. Os certificados dos membros de replicaset e shared, devem emitir o mesmo certificado X.509  (**CA**), como mencionado anteriormente.

 2. O assunto (parâmetro **subject** do OpenSSL)  **deve ao menos conter  um valor preenchido** dos seguintes atributos do nome destinto  (Distinguished Name (**DN**)).
	  > Organization (**O**) - Organização
    
    > Organizational Unit (**OU**) - Unidade Organizacional
   
   > Domain Component (**DC**). - Componente de Domínio  
 3.  Os atributos DN (**O, OU e DC**)  **devem ser iguais para todos os certificados** do servidor, não importando a ordem.
 4. O atributo **CN** (Common Name - Nome Comum) **deve possuir o mesmo hostname do servido**. Como **boa pratica** considera o seguinte exemplo de DN para um conjuntos de replicaset.
 
	  > subject= CN=<**hostname1**>,OU=DepTI,O=MongoDB,ST=SP,C=BR
    >
     subject= CN=<**hostname2**>,OU=DepTI,O=MongoDB,ST=SP,C=BR
    > 
    subject= CN=<**hostname3**>,OU=DepTI,O=MongoDB,ST=SP,C=BR
 5. Se Utilizar de Chave Extendida (Extended Key Usage) inclua a seguinte configuração

    > **extendedKeyUsage = clientAuth**
    
***

 - ***Pré-requisito - Certificado Cliente***
 
 1. O certificado X.509  (**CA**), deve ser o mesmo, tanto para o servidor quanto para o cliente, como mencionado anteriormente.
 2.  O DN (**O, OU e DC**), do certificado do cliente, **deve ser diferente** do DN utilizado pelos servidores.
 
	  >  Caso o DN (**O, OU e DC**) do certificado do cliente tenha os mesmos dados do DN do servidor, o usuário terá total acesso ao sistema, pois a instância do cliente será reconhecida como um cluster do servidor.
		
 3. ***Cada usuário deverá ter o seu próprio certificado.***
 4. O certificado do cliente deve possuir os seguintes campos:
 
	 > keyUsage = digitalSignature
	 >
   > extendedKeyUsage = clientAuth

 

***
