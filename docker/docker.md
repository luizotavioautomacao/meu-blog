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
### Criando Volumes:
`docker container run -ti --mount type=bind,src=/home/luizotavio/mvp/primeiro_dockerFile,dst=/volume ubuntu`
`docker container run -ti --mount type=bind,src=/home/luizotavio/mvp/primeiro_dockerFile,dst=/volume,ro ubuntu` => ready only
`docker container run -ti --mount type=bind,src=/home/luizotavio/mvp/primeiro_dockerFile/Dockerfile,dst=/Dockerfile ubuntu`
### Criando Volumes de maneira elegantes:
`docker volume create nomeVolume1`
### Para removê-lo:
`docker volume rm nomeVolume1`
### Para verificar detalhes desse volume:
`docker volume inspect nomeVolume1`
### Para remover volumes que não estão sendo utilizados:
`docker volume prune`
### Para montar o volume em algum container:
`docker container run -d --mount type=volume,source=nomeVolume1, destination=/varopa nginx` 
--mount => comando para criar volumes
type=volume => indica que o tipo é "volume". Ainda existe "bind", onde, em vez de indicar um volume, você indicaria um diretório como source
destination=/var/opa => onde no container montarei esse volume
### Localizando volumes:
`docker volume inspect --format '{{ .Mountpoint }}' nomeVolume1`
"-f" ou "--format" => é um filtro da saída do inspect
### Vamos criar um container com o nome de database com um volume chamado /data
`docker container create -v /data --name database centos`
`docker inspect -f {{.Mounts}} database`
--volumes-from => utilizado quando queremos montar um volume disponibilizado por outro container
-e => utilizado para informar variáveis de ambiente para o container
### Agora vamos criar um container com o Banco de Dados (PostgreSQL) para acessar o volume database
`docker run -d -p 5432:5432 --name pgsql1 --volumes-from database -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql`
`docker run -d -p 5433:5432 --name pgsql2 --volumes-from database -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql`
### Fazer backup do diretório /data do container "database"
`mkdir backup`
`cd backup`
`docker run -ti --volumes-from database -v $(pwd):/backup debian tar -cvf /backup/backup.tar /data`

### Criando e gerenciando imagens
```
mkdir /root/Dockerfiles
cd /root/Dockerfiles
mkdir apache
cd apache
vim Dockerfile
FROM debian

RUN apt-get update && apt-get install -y apache2 && apt-get clean
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL description="Webserver"

VOLUME /var/www/html
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachect1"]
CMD ["-D", "FOREGROUND"]
```
FROM => indica a imagem a servir como base (primeira linha do Dockerfile)
RUN => lista de comandos que deseja executar na criação da imagem
ENV => define variáveis de ambiente
LABEL => adiciona metadata à imagem, como descrição, versão, etc ...
VOLUME => define um volume a ser montando no container
ADD => copia novos arquivos, diretórios, arquivos TAR ou arquivos remotos e os adiociona ao filesystem do container
CMD => executa um comando. Diferente do RUN que executa o comando quando está "buildando" a imagem, o CMD executa somente quando o container for iniciado
COPY => copia novos arquivos e diretórios e os adiciona ao filesystem do container
MAINTAINER => autor da imagem
USER => determina qual usuário será utilizado na imagem (por default é o root)
WORKDIR => responsável por mudar do diretório "/" (raiz) para o especificado nele
ENTRYPOINT => permite configurar um container para rodar um executável. Quando o executável for finalizado, o container também será.
No exemplo do Dockerfile o ENTRYPOINT:
"/usr/sbin/apachect1" => esse é o comando
"-D", "FOREGROUND => parâmetros
No shell ficaria assim:
`/usr/sbin/apachect1 -D FOREGROUND`
- [Mais detalhes](https://www.slideshare.net/jfnredes/images-deep-dive)

Após criar o arquivo Dockerfile vamos fazer o build:
`docker build .`
`docker image ls`
Vamos executar novamente passando o parâmetro "-t" que é responsável por adicionar uma tag
`docker build -t luizotavio/apache:1.0 .`
`docker image ls`i
Vamos executar o container criado:
`docker container run -ti luizotavio/apache:1.0`
`ps -ef` => verificar se a porta 80 está "LISTEN"
`/etc/init.d/apache2 start`
`apt update && apt install iproute2` => se não executar o comando abaixo
`ip addr show eth0`

### Multi-stage => pipeline no dockerfile
Podemos compilar nossa aplicação em um container e executá-la em outro container => mais rápido e menos memória
---
### Customizar uma imagem a partir de uma existente
`docker container run -ti debian:8 /bin/bash`
Vamos instalar o apache como exemplo:
`apt-get update && apt-get install -y apache2 && apt-get clean`
`Ctrl + p + q`
`docker container ls`
`docker commit -m "meu container" CONTAINER ID`
`docker tag IMAGE ID luizotavio/debian_apache_2:1.0`
`docker container run -ti luizotavio/debian_apache_2:1.0 /bin/bash`
`/etc/init.d/apache2 start`
`ss -atn`
`ip addr show eth0`

