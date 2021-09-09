## Links
[Play with Docker](https://labs.play-with-docker.com)
[Curso de introdução a docker](https://www.youtube.com/watch?v=j9vfSaCIyPI&list=PLXzx948cNtr8N5zLNJNVYrvIG6hk0Kxl-)
- [Slide](https://insightlab.ufc.br/wp-content/uploads/2020/05/Introdu%C3%A7%C3%A3o-a-Docker-compactado.pdf) do curso acima
[Ambiente de desenvolvimento nodejs com docker](https://youtu.be/AVNADGzXrrQ)

## Resumo comandos
`docker run hello-world`
`docker --version`

## Livros
(Descomplicando Docker)[https://www.amazon.com.br/Descomplicando-Jeferson-Fernando-Nogueira-Vitalino/dp/857452901X/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=docker&qid=1630433004&sr=8-1]

## Referências de profissionais da área
(Jeferson Fernando Vitalino)[https://www.linkedin.com/in/jefersonfernando/?originalSubdomain=nl]

---
Instalando versão recente do docker no Linux:
`curl -fsSL https://get.docker.com/ | sh`

[Docker + WSL](https://docs.docker.com/desktop/windows/wsl/)

Primeiros passos:
1. `docker container run hello-world`
2. `docker image ls`
3. `docker container ls` -> listar containers em execução
4. `docker container ls -a` -> listar containers em execução, parados e/ou finalizados

Modo interativo ou daemonizando o container => "-ti" x "-d"
"-t" => disponibiliza um console para nosso container
"-i" => mantém o STDIN aberto mesmo que você não esteja conectado no container
"-d" => faz com que o container rode um daemon, ou seja, sem a interatividade que os outros parâmetros fornecem

Exemplos:
`docker container run -ti centos:7`
Para confirmar que estamos dentro do container:
`cat /etc/redhart-release` => CentOS Linux release 7.9.2009 (Core)
O arquivo acima indica qual a versão do Centos que estamos utilizando
- Sair do container e continuar executando => `Ctrl + p + q`
- Para confirmar a execução do container => `docker ps` 
- Para voltar ao bash do container => `docker container attach CONTAINER ID`
Criar um docker sem executar:
`docker container create -ti ubuntu`
Listar e depois executar um docker criado:
1. `docker container ls -a`
2. `docker container start CONTAINER ID`
3. `docker container attach CONTAINER ID`
4. `cat /etc/issue` => Ubuntu 20.04.3 LTS \n \l
5. `Ctrl + d` => encerra processo de execução do docker
Parar de executar o docker:
`docker container stop CONTAINER ID`
`docker container restart CONTAINER ID` => restart
`docker container pause CONTAINER ID` => pausar
`docker container unpause CONTAINER ID` => "despausar"
Visualizando consumo de recursos pelo container:
`docker container stats CONTAINER ID` => para sair:  `Ctrl + c` 
`docker container stats` => visualizar todos os containers
Quais processos estão em execução:
`docker container top CONTAINER ID`
Para verificar os logs de um determinado container:
`docker container logs CONTAINER ID` => exibe STDOUT: histórico de mensagens em primeiro plano
`docker container logs -f CONTAINER ID`
Remover um container (a imagem continuará no host):
`docker container rm CONTAINER ID`
`docker container rm -f CONTAINER ID` => forçar a parada e em seguida remover 
Especificando a quantidade de memória
`docker container run -ti --name nomeDocker1 debian`
`docker container inspect nomeDocker1 | grep -i mem` => visualizar a quantidade de memória que está configurado para esse container
`docker container run -ti -m 512m --name nomeDocker2 debian` => utilizados o parâmetro "-m" ou "--memory" para especificar a quantidade de memória
`docker container update -m 256m --cpus=4 nomeDocker2` 
`docker update help` => ver as opções completas
Criando arquivo Dockerfile ou .dockerfile:
```
FROM debian
RUN /bin/echo "HELLO DOCKER!"
```
`docker build -t tosko:1.0 .`
Criando Volumes:
`docker container run -ti --mount type=bind,src=/home/luizotavio/mvp/primeiro_dockerFile,dst=/volume ubuntu`
`docker container run -ti --mount type=bind,src=/home/luizotavio/mvp/primeiro_dockerFile,dst=/volume,ro ubuntu` => ready only
`docker container run -ti --mount type=bind,src=/home/luizotavio/mvp/primeiro_dockerFile/Dockerfile,dst=/Dockerfile ubuntu`
Criando Volumes de maneira elegantes:

