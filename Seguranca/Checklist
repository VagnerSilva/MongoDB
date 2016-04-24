# Checklist

#### Controle de Acesso
----------
Insiste em configurarmos a autenticação no MongoDB como ativa (**enable**).
Por padrão a configuração de **controle de acesso** (Access Control) esta desativada.
Ao ativa-la, o **usuário apenas poderá acessa os recursos do banco de dados nos quais tenha privilégio**.

Para habilitar o controle de acesso temos as seguintes opções:

**Por linha de comando**
```javascript
    mongod --auth --datapath "caminho"
```
**Por arquivo de configuração**
```javascript
    security:
	    authorization: <string>
```
  
######**String valor:**  **enabled** ou **disabled** 


#### Configura regras
----------
Após iniciarmos a instancia configurada para acesso autentificado a **primeira coisa que devemos fazer** e a criação de um **usuário com perfil de administrador** (admin) , onde vale ressalta que tal procedimento deve ser realizado para cada base.

```javascript
    use basededados
db.createUser(
  {
    user: "nomeDoUsuaro",
    pwd: "senha",
    roles: [ { role: "userAdminAnyDatabase", db: "basededados" } ]
  }
)
```

######**userAdminAnyDatabase e uma das regras que podemos utilizar, para saber mais, acesse o seguinte link: [built-in-roles](https://docs.mongodb.org/manual/reference/built-in-roles/)** 
por padrão utilizamos "
