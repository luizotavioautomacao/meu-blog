- Para o nosso primeiro setup vamos usar como base o da [Rocket](https://www.youtube.com/watch?v=rCeGfFk-uCk&ab_channel=Rocketseat) porém com algumas diferenças ;]


#configurar o typescript como padrão do projeto
```
npm i typescript -D
npm i -g typescript
tsc --init
```
#Outras dependências
```
npm i mongoose --save
npm i express --save
npm i nodemon --save
npm i socket.io --save
npm i @types/express --save
npm install -g ts-node
```

Dentro do package.json
"scripts": {
    "dev": "nodemon --exec ts-node ./src/server.ts"
  }

  `npm run dev`

  #OBS
  ESLint e Jest => estudar sobre
