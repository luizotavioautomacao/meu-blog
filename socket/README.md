#back-end

Para abrir uma conexão cliente-servidor é necessário usar socket.
Vamos usar essa [video-aula](https://www.youtube.com/watch?v=-jXfKDYJJvo) e entender um pouco mais sobre o assunto.

[biblioteca io](https://www.npmjs.com/package/socket.io) =>
**`npm i io --save`**

`io.on('connection', socket => { /*/ O evento connection recebe o objeto socket   /*/})`

Tipos de eventos:
- socket.on
- socket.emit
- socket.broadcast.emit 