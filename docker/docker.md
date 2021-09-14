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
### Compartilhando imagens 
Fazer cadastro para login no [hub.docker](https://hub.docker.com/)
`docker history IMAGE ID` => histórico do que foi feito no docker
- Alternativa ao [docker history, mas usando a web](https://www.ctl.io/developers/blog/post/imagelayers-io-docker-visualization-and-badges) sem fazer download da image
Após cadastro:
`docker login`
É possível criar repositórios à vontade porém na conta free somente um repositório privado
`docker login registry.seilaqual.com` Se não usar o hub.docker e usar outra opção
Subir uma imagem no Docker Hub:
1. Fazer o commit => `docker commit -m 'meu container' CONTAINER ID`
2. Fazer o push => `docker push usuario/nome_da_imagem:versão`
`docker push luizotavioautomacao/debian_apache_2:1.0` => luizotavio/debian_apache_2   1.0       38a6e0a23f74
`docker search nome_usuario` => procurar repositórios na sua conta
`docker image rm` => responsável por remover a imagem do disco local
Registrar imagens localmente
Vamos clonar o repositório do [Docker Distribution](https://github.com/distribution/distribution)
`docker container run -d -p 5000:5000 --restart=always --name registry registry:2` => criamos um container chamado "registry" que utiliza a imagem "registry:2" e com o parâmetro "--restart=always" sempre será reiniciado acaso houver problema
`docker tag IMAGE ID localhost:5000/debian_apache_2` => criar um nova tah para fazer o push localmente
`docker push localhost:5000/debian_apache_2` => fizemos um regustry local
Para mais informações => https://github.com/distribution/distribution

### Gerenciando a rede dos containers
-dns => indica o servidor DNS
-hostname => indica um hostname
-link => cria um link entre os containers sem a necessidade de se saber o IP um do outro
-net => permite configurar o modo de rede que você usará com o container, a mais conhecida é -net=host que permite o container utiliza a rede do host para se comunicar a não crie um stack de rede para o container
-expose => expõe a porta do container apenas
-publish => expõe a porta do container apenas
-default-gateway => determina a rota padrão
-mac-address => determina um MAC address
`iptables -L -n`
`iptables -L -n -t nat`

### Docker Machine, Docker Swarm e Docker Compose
Docker Machine é usado para criar vários hosts => [documentação](https://docs.docker.com/machine/)
Instalando o Docker Compose
```
wget https://github.com/docker/compose/releases/download/1.6.2/docker-compose-Linux-x86_64
mv docker-compose-Linux-x86_64 /usr/local/bin/dockercompose
chmod +x /usr/local/bin/docker-compose
```
Verificando se o Docker Compose foi instalado e sua versão:
`docker-compose —version` => docker-compose version 1.29.2, build 5becea4c
build => indica o caminho do seu dockerfile
command => executa um comando
container_name => nome para container
dns => indica o DNS server
dns_search => especifica um search domain
dockerfile => especifica um dockerfile alternativo
env_file => especifica um arquivo com variáveis de ambiente
environment => adiciona variáveis de ambiente
expose => expõe a porta do container
external_links => linka containers que não estão especificados no Docker Compose atual
extra_hosts => adiciona uma entrada no "etc/hosts" do container
image => indica uma imagem
labels => adiciona metadados ao container
links => linka container dentro do mesmo Compose Docker
log_driver => indica o formato de log a ser gerado, por exemplo, syslog, json-file, etc
log_opt => indica para onde mandar os logs. Pode ser local ou em syslog remoto
net => modo de uso da rede
ports => expõe as portas do container e do host
volumes, volume_driver => monta volumes no container
volumes_from => monta volumes através de outro container
`docker-compose up`
`docker-compose up -d`
`docker-compose ps`
`docker-compose stop`
`docker-compose start`
`docker-compose logs`
