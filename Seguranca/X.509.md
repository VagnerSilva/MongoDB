## **X.509**

Inicialmente é importante ressaltar que a autenticação   por via do  certificado x.509 **requer um conexão [TLS](https://pt.wikipedia.org/wiki/Transport_Layer_Security)** segura.

Para identificar se a versão que estamos a utilizar da suporte ao protocolo TLS , basta digitamos no terminal o comando **mongod --version**

![openssl](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/openssl.PNG)

Sendo possível ser visualizado a versão do [OpenSSL](https://pt.wikipedia.org/wiki/OpenSSL) , concluímos que tal versão oferece suporte ao protocolo TLS. Sendo assim, podemos utilizar o mecanismo de autenticação X.509.

E se estamos falando de OpenSSL, obviamente, estamos falando de certificados, nos quais  são responsáveis pela autenticação do cliente ao servidor, dessa forma não necessitamos da utilização de um usuário e senha como no modelo SCRAM-SHA-1.


### **Pontos importantes**
------------------
Para produção deve-se utilizar certificados validos **assinados por uma unica [Autoridade de Certificação](https://pt.wikipedia.org/wiki/Autoridade_de_certifica%C3%A7%C3%A3o)**  (***CA - Certificate Authority***). Podendo ser gerado e mantido por você ou sua empresa, até mesmo ser utilizado certificados fornecidos por terceiros.

Ou seja, a Autoridade de Certificação (**CA**),  **deve, obrigatoriamente, ser utilizada na geração dos cerificados clientes e servidor(es).**
Frisando que,  **cada instância do MongoDB deve conter um certificado exclusivo.**

O **certificado do cliente** deve conter os seguintes campos:
> **keyUsage** = digitalSignature
**extendedKeyUsage** = clientAuth

Também, o assunto (parâmetro **subject**) **deve conter seu nome de distinto** (Distinguished Name (**DN**)), **deferente** de qualquer servidor, Seja ele um ReplicaSet ou Shared.

**Além disso, os seguintes atributos**, do parâmetro subject:
> Organization (**O**) - Organizaçao
> 
> Organizational Unit (**OU**) - Unidade Organizacional
> 
> Domain Component (**DC**). - Componente de Domínio

**Deve-se diferenciar de ao menos um desses atributos em relação aos outros certificados**.

Pois **a utilização, de um certificação cliente, com o mesmo dado**, de um certificado membro( replicaSet ou Shared) **implicará na permissão total no sistema**, uma vez que o cliente será identificado como parte do cluster.
