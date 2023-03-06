# drassadnikov_microservices
drassadnikov microservices repository
#Установка docker на yandex cloud:
https://docs.docker.com/desktop/install/ubuntu/

sudo apt-get install ca-certificates curl gnupg lsb-release
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo service docker start
sudo docker run hello-world

#Установить с помощью удобного скрипта 

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
Executing docker install script, commit: 7cae5f8b0decc17d6571f9f52eb840fbc13b2737
<...>

# Установка docker-machine 
curl -sL https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-Linux-x86_64 \
    > /tmp/docker-machine
sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine

--------------------------------


yc-user@fhm3eh2q3kdvid2nvp3t:~$ sudo docker version
Client: Docker Engine - Community
 Version:           23.0.1
 API version:       1.42
 Go version:        go1.19.5
 Git commit:        a5ee5b1
 Built:             Thu Feb  9 19:46:49 2023
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          23.0.1
  API version:      1.42 (minimum version 1.12)
  Go version:       go1.19.5
  Git commit:       bc3805a
  Built:            Thu Feb  9 19:46:49 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.18
  GitCommit:        2456e983eb9e37e47538f59ea18f2043c9a73640
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
yc-user@fhm3eh2q3kdvid2nvp3t:~$ 


docker-machine create --driver generic --generic-ip-address=51.250.70.85 --generic-ssh-user yc-user --generic-ssh-key ~/.ssh/yc  docker-host 
eval $(docker-machine env docker-host)

sudo apt-get update
sudo apt-get install ./docker-desktop-4.17.0-amd64.deb

# Setup address and port of remote PC
HOST_IP=51.250.70.85
HOST_PORT=22
# Setup administrator's name for the remote PC
HOST_USERNAME=yc-user
# Name of the deb-package in the current folder
PACKAGE_NAME=docker-desktop-4.17.0-amd64.deb
# Setup complex server address
STKH_SERVER_ADDRESS=192.168.1.62

# Copy package to the remote PC
scp -p "$HOST_PORT" "$PACKAGE_NAME" -i ~/.ssh/yc $HOST_USERNAME@$HOST_IP:/tmp
# Execute package installation
ssh -p "$HOST_PORT" $HOST_USERNAME@$HOST_IP "DEBIAN_FRONTEND=noninteractive dpkg --install /tmp/$PACKAGE_NAME; apt-get update; DEBIAN_FRONTEND=noninteractive apt-get install -fy"
# Setup server address on the remote PC for the client
ssh -p "$HOST_PORT" $HOST_USERNAME@$HOST_IP "stkh-client --server=$STKH_SERVER_ADDRESS"
# Delete package from the remote PC
ssh -p "$HOST_PORT" $HOST_USERNAME@$HOST_IP "rm /tmp/$PACKAGE_NAME"



yc-user@fhm3eh2q3kdvid2nvp3t:~$ curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && 
>     chmod +x /tmp/docker-machine &&
>     sudo cp /tmp/docker-machine /usr/local/bin/docker-machine

yc-user@fhm3eh2q3kdvid2nvp3t:~$ docker-machine --version
docker-machine version 0.16.2, build bd45ab13

yc-user@fhm3eh2q3kdvid2nvp3t:~$ docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                       SWARM   DOCKER    ERRORS
docker-host   *        generic   Running   tcp://51.250.70.85:2376           v23.0.1   


docker run --rm -ti tehbilly/htop
docker run --rm --pid host -ti tehbilly/htop

# ДЗ по docker-3.
Соберем образы с нашими сервисами:

yc-user@docker-host:~$ docker run -d --network-alias=post drassadnikov/post:1.0

docker build -t drassadnikov/post:1.0 ./post-py

docker build -t drassadnikov/comment:1.0 ./comment

docker build -t drassadnikov/ui:1.0 ./ui

Загрузка docker image: 
$ docker tag reddit:latest drassadnikov/otus-reddit:1.0
$ docker push drassadnikov/otus-reddit:1.0

$ docker login
Проверка:
docker run --name reddit -d -p 9292:9292 drassadnikov/otus-reddit:1.0

-------------
yc-user@docker-host:~$ docker build -t drassadnikov/post:1.0 ./post-py
[+] Building 38.3s (10/10) FINISHED                                    
yc-user@docker-host:~$ docker push drassadnikov/post:1.0


yc-user@docker-host:~$ docker build -t drassadnikov/comment:1.0 ./comment
[+] Building 69.3s (11/11) FINISHED                                     
yc-user@docker-host:~$ docker push drassadnikov/comment:1.0

yc-user@docker-host:~$ docker build -t drassadnikov/ui:1.0 ./ui
[+] Building 138.7s (13/13) FINISHED     
yc-user@docker-host:~$ docker push drassadnikov/ui:1.0

yc-user@docker-host:~$ docker images
REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
drassadnikov/ui            1.0       a89cfda1edc2   3 minutes ago    464MB
drassadnikov/comment       1.0       160db7aff9d3   18 minutes ago   771MB
drassadnikov/post          1.0       aab0b5b68bb8   31 minutes ago   121MB
mongo                      latest    dcfda10b61c5   2 days ago       645MB
drassadnikov/otus-reddit   1.0       0fd770b5fef9   5 days ago       664MB
hello-world                latest    feb5d9fea6a5   17 months ago    13.3kB


yc-user@docker-host:~$ docker network create reddit
0dd8ded65c4fbd9c4e8422b0b3fbe791b74882ca558b92e94280306a7bbc81d8

docker network create reddit
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post drassadnikov/post:1.0
docker run -d --network=reddit --network-alias=comment drassadnikov/comment:1.0
docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:1.0

yc-user@docker-host:~$ docker network create reddit
0dd8ded65c4fbd9c4e8422b0b3fbe791b74882ca558b92e94280306a7bbc81d8
yc-user@docker-host:~$ docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
c1911cf98f145a52af1f686d5925287a9015074a93fe725712d17c059e4fd17e
yc-user@docker-host:~$ docker run -d --network=reddit --network-alias=post drassadnikov/post:1.0
79e36cdda9eb6975c746d5931f17e05e00fbc481176ed6cd9ec4732f1780096e
yc-user@docker-host:~$ docker run -d --network=reddit --network-alias=comment drassadnikov/comment:1.0
327911f5ce72ce90c5ec2f4a02cf1938f9337def613a817b9825be386601cd02
yc-user@docker-host:~$ docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:1.0
704faaf692aac16c936f5a2de9e38010197f5f723d2c03fc038ff2dedab78dc8
yc-user@docker-host:~$ 


yc-user@docker-host:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED              STATUS              PORTS                                       NAMES
704faaf692aa   drassadnikov/ui:1.0        "puma"                   43 seconds ago       Up 41 seconds       0.0.0.0:9292->9292/tcp, :::9292->9292/tcp   modest_kowalevski
327911f5ce72   drassadnikov/comment:1.0   "puma"                   About a minute ago   Up About a minute                                               flamboyant_shirley
79e36cdda9eb   drassadnikov/post:1.0      "python3 post_app.py"    About a minute ago   Up About a minute                                               charming_roentgen
c1911cf98f14   mongo:latest               "docker-entrypoint.s…"   About a minute ago   Up About a minute   27017/tcp                                   stoic_elgamal


yc-user@docker-host:~$  docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                        SWARM   DOCKER    ERRORS
docker-host   -        generic   Running   tcp://51.250.68.228:2376           v23.0.1   


http://51.250.68.228:9292/

Удаляем контейнер:
yc-user@docker-host:~$ docker rm 79e36cdda9eb -f
79e36cdda9eb

yc-user@docker-host:~$ docker images
REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
drassadnikov/ui            1.0       a89cfda1edc2   28 minutes ago   464MB
drassadnikov/comment       1.0       160db7aff9d3   43 minutes ago   771MB
drassadnikov/post          1.0       aab0b5b68bb8   56 minutes ago   121MB
mongo                      latest    dcfda10b61c5   2 days ago       645MB
drassadnikov/otus-reddit   1.0       0fd770b5fef9   5 days ago       664MB
hello-world                latest    feb5d9fea6a5   17 months ago    13.3kB


Удаляем образ:
docker rmi aab0b5b68bb8

yc-user@docker-host:~$ docker rmi aab0b5b68bb8
Untagged: drassadnikov/post:1.0
Untagged: drassadnikov/post@sha256:e3b9b6944bddfce067671923f1e543e554f591f496e2899687aac608457638f7
Deleted: sha256:aab0b5b68bb85de94299f7a8ce5f8eebab7deaf6d16c2c9d2c2be377e4770106
yc-user@docker-host:~$ 

http://51.250.68.228:9292/
Post successuly published
------------------

yc-user@docker-host:~$ docker volume create reddit_db
reddit_db
yc-user@docker-host:~$ docker kill $(docker ps -q)
9d45dabccabb
704faaf692aa
327911f5ce72
c1911cf98f14
yc-user@docker-host:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post drassadnikov/post:1.0
docker run -d --network=reddit --network-alias=comment drassadnikov/comment:1.0
docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:2.0

docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post drassadnikov/post:1.0
docker run -d --network=reddit --network-alias=comment drassadnikov/comment:1.0
docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:2.0

------------
FROM ruby:2.2
RUN apt-get update -qq && apt-get install -y build-essential
ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME
ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME
ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292
CMD ["puma"]

FROM ubuntu:16.04
RUN apt-get update \
 && apt-get install -y ruby-full ruby-dev build-essential \
 && gem install bundler --no-ri --no-rdoc
ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME
ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME
ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292
CMD ["puma"]

------------------

yc-user@docker-host:~$ docker build -t drassadnikov/ui:2.0 ./ui
[+] Building 1.2s (13/13) FINISHED 

yc-user@docker-host:~$ docker push drassadnikov/ui:2.0

yc-user@docker-host:~$ docker images
REPOSITORY                 TAG       IMAGE ID       CREATED             SIZE
drassadnikov/post          1.0       162f55beec9b   18 minutes ago      121MB
drassadnikov/ui            1.0       a89cfda1edc2   49 minutes ago      464MB
drassadnikov/ui            2.0       a89cfda1edc2   49 minutes ago      464MB
drassadnikov/comment       1.0       160db7aff9d3   About an hour ago   771MB
mongo                      latest    dcfda10b61c5   2 days ago          645MB
drassadnikov/otus-reddit   1.0       0fd770b5fef9   5 days ago          664MB
hello-world                latest    feb5d9fea6a5   17 months ago       13.3kB

yc-user@docker-host:~$ docker kill $(docker ps -q)
efe9b3130b67
8bc640f1b295
5a4237c3bc1a
yc-user@docker-host:~$ 


docker run -d --network=reddit --network-alias=post_db  --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post drassadnikov/post:1.0
docker run -d --network=reddit --network-alias=comment drassadnikov/comment:1.0
docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:2.0


Перезагрузка контейнеров:
yc-user@docker-host:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                       NAMES
e423dbdbedb5   drassadnikov/ui:2.0        "puma"                   2 minutes ago   Up 2 minutes   0.0.0.0:9292->9292/tcp, :::9292->9292/tcp   sad_moser
28b75f0594fa   drassadnikov/comment:1.0   "puma"                   2 minutes ago   Up 2 minutes                                               jolly_poincare
d6c633cd5c20   drassadnikov/post:1.0      "python3 post_app.py"    2 minutes ago   Up 2 minutes                                               eloquent_chatterjee
b2084d0743e9   mongo:latest               "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   27017/tcp                                   funny_vaughan
yc-user@docker-host:~$ docker restart e423dbdbedb5
e423dbdbedb5
yc-user@docker-host:~$ docker restart 423dbdbedb5e423dbdbedb5
Error response from daemon: No such container: 423dbdbedb5e423dbdbedb5
yc-user@docker-host:~$ 
yc-user@docker-host:~$ docker restart 28b75f0594fa
28b75f0594fa
yc-user@docker-host:~$ docker restart d6c633cd5c20
d6c633cd5c20
yc-user@docker-host:~$ docker restart b2084d0743e9
b2084d0743e9
yc-user@docker-host:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS              PORTS                                       NAMES
e423dbdbedb5   drassadnikov/ui:2.0        "puma"                   4 minutes ago   Up About a minute   0.0.0.0:9292->9292/tcp, :::9292->9292/tcp   sad_moser
28b75f0594fa   drassadnikov/comment:1.0   "puma"                   4 minutes ago   Up 28 seconds                                                   jolly_poincare
d6c633cd5c20   drassadnikov/post:1.0      "python3 post_app.py"    4 minutes ago   Up 18 seconds                                                   eloquent_chatterjee
b2084d0743e9   mongo:latest               "docker-entrypoint.s…"   4 minutes ago   Up 10 seconds       27017/tcp                                   funny_vaughan
yc-user@docker-host:~$ 



