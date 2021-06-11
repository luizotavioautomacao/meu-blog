npm, abreviação de Node Package Manager, é duas coisas: primeiro, é um repositório online para a publicação de projetos Node.js de código aberto; segundo, é um utilitário de linha de comando para interagir com o repositório mencionado, o que ajuda na instalação de pacotes, no gerenciamento de versões e no gerenciamento de dependências. Depois de ter um pacote que você deseja instalar, ele pode ser instalado com um único comando de linha de comando.

### Fazer upload de biblioteca no NPM
1. Criar um repositório no github
2. Git clone do repositório localmente
3. Criar um conta npm
4. `npm set init.author.email “seu email aqui" `
5. `npm set init.author.name “seu nome aqui” `
6. `npm set init.license “MIT”` -> [licença MIT](https://pt.wikipedia.org/wiki/Licen%C3%A7a_MIT)
7. `npm adduser`
8. Criar o o package.json
9. `npm init`
10. Implementar biblioteca
11. Fazer commit e push
12. fazer publish no npm -> `npm publish`
13. Se tiver problemas ao fazer upload -> `until npm publish; do echo "Try again"; sleep 2; done`

[Tutorial rápido](https://www.alura.com.br/artigos/criando-e-publicando-uma-biblioteca-javascript-no-npm)