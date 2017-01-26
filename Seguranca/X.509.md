## **X.509**

Inicialmente é importante ressaltar que a autenticação   por via do  certificado x.509 **requer um conexão [TLS](https://pt.wikipedia.org/wiki/Transport_Layer_Security)** segura.

Para identificar se a versão que estamos a utilizar da suporte ao protocolo TLS , basta digitamos no terminal o comando **mongod --version**

![openssl](https://github.com/VagnerSilva/MongoDB/blob/master/Seguranca/imgs/openssl.PNG)

Sendo possível ser visualizado a versão do [OpenSSL](https://pt.wikipedia.org/wiki/OpenSSL) , concluímos que tal versão oferece suporte ao protocolo TLS. Sendo assim, podemos utilizar o mecanismo de autenticação X.509.
