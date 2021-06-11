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
