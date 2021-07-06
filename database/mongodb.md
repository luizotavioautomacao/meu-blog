O MongoDB é um banco de dados de documentos do NoSQL de software livre projetado para funcionar com JSON e armazenar informações sem esquema. Ele é horizontalmente escalonável, o que significa que vários computadores menores farão o trabalho para você. É bom para flexibilidade e dados não estruturados, além de armazenar em cache a análise em tempo real.

MondoDB -> document database [Documentação](https://docs.mongodb.com/manual/tutorial/manage-mongodb-processes/)

- JSON -> [JavaScript Object Notation](https://www.json.org)
-- null, Booleanm Number, String, Object, Array
- O MongoDB trabalha com o [BSON](http://bsonspec.org) (Binary JSON) que é uma extensão do JSON para o mongo.
-- MinKey, MaxKey, Timestamp - tipos ultilizados internamente no MongoDB;
-- BinData - array de bytes para dados binários;
-- ObjectId - identificador único de um registro do MongoDB;
-- Date - represetanção de data;
-- Expressões regulares

##Mão na massa (após [instalação do mongodb](https://www.mongodb.com/try/download/community e [Robo3T](https://robomongo.org/download))):
1. Baixe o arquivo [megasena.csv](https://drive.google.com/file/d/10SyxeyRxNn7USzAb9rJwGv_xlQHmcv6w/view?usp=sharing))
2. Em seguida abra o terminal e execute o serviço do mongo
```
  mongod
 ```
 3. Vá no diretório que tem o arquivo baixado e execute o comando pelo terminal
```
mongoimport -c megasena --type csv --headerline megasena.csv
```

OBS:
* Se o documento passar de bilhões de registros deve-se separá-los por cluster pela data.

Cursos e artigos
- [w3big](http://www.w3big.com/pt/mongodb/default.html)
- [install-mongodb-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04)

Após instalar no Windows, devemos criar um path ou executar `mongod` na pasta que contém o executável do mongod (no meu computador -> C:\Program Files\MongoDB\Server\4.4\bin )
Parar criar um path e executar `mongod` em qualquer pasta do cmd
set PATH=%mongod%;C:\Program Files\MongoDB\Server\4.4\bin
ou executar `mongod` na pasta que contém o executável do mongod (no meu computador -> C:\Program Files\MongoDB\Server\4.4\bin )
Pesquisa por "variável de ambiente" e adicione no botão 'novo' o caminho do mogond (no meu computador -> C:\Program Files\MongoDB\Server\4.4\bin ) na seção "Variáveis do sistema" -> agora é só ser feliz! =]
