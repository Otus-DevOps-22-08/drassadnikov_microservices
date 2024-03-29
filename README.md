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
curl -sL https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-Linux-x86_64 > /tmp/docker-machine
sudo mv /tmp/docker-machine /usr/local/bin/docker-machine && chmod +x /usr/local/bin/docker-machine

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

# ДЗ по docker-4

yc-user@docker-host:~$ docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                        SWARM   DOCKER    ERRORS
docker-host   -        generic   Running   tcp://51.250.68.228:2376           v23.0.1   
yc-user@docker-host:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
yc-user@docker-host:~$ docker run -ti --rm --network none joffotron/docker-net-tools -c ifconfig
lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)


yc-user@docker-host:~$ docker run -ti --rm --network host joffotron/docker-net-tools -c ifconfig
br-0de816ba9d9e Link encap:Ethernet  HWaddr 02:42:15:B0:15:99  
          inet addr:172.20.0.1  Bcast:172.20.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:15ff:feb0:1599%32565/64 Scope:Link
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:22 errors:0 dropped:0 overruns:0 frame:0
          TX packets:26 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2505 (2.4 KiB)  TX bytes:2748 (2.6 KiB)

docker0   Link encap:Ethernet  HWaddr 02:42:00:0B:0D:FF  
          inet addr:172.17.0.1  Bcast:172.17.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:ff:fe0b:dff%32565/64 Scope:Link
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:39033 errors:0 dropped:0 overruns:0 frame:0
          TX packets:57474 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2493060 (2.3 MiB)  TX bytes:611559648 (583.2 MiB)

eth0      Link encap:Ethernet  HWaddr D0:0D:1B:6F:C7:00  
          inet addr:10.128.0.25  Bcast:10.128.0.255  Mask:255.255.255.0
          inet6 addr: fe80::d20d:1bff:fe6f:c700%32565/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:533370 errors:0 dropped:0 overruns:0 frame:0
          TX packets:687263 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:2526840911 (2.3 GiB)  TX bytes:461032800 (439.6 MiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1%32565/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:21304 errors:0 dropped:0 overruns:0 frame:0
          TX packets:21304 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1667972 (1.5 MiB)  TX bytes:1667972 (1.5 MiB)


yc-user@docker-host:~$ sudo ln -s /var/run/docker/netns /var/run/netns
yc-user@docker-host:~$ sudo ip netns
Error: Peer netns reference is invalid.
Error: Peer netns reference is invalid.
netns
default
yc-user@docker-host:~$ docker network create reddit --driver bridge
Error response from daemon: network with name reddit already exists
yc-user@docker-host:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
e64f4827c2db   bridge    bridge    local
b8c4ddd8e438   host      host      local
cb574e276348   none      null      local
0de816ba9d9e   reddit    bridge    local


docker run -d --network=reddit mongo:latest
docker run -d --network=reddit drassadnikov/post:1.0
docker run -d --network=reddit drassadnikov/comment:1.0
docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:1.0

docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post drassadnikov/post:1.0
docker run -d --network=reddit --network-alias=comment drassadnikov/comment:1.0
docker run -d --network=reddit -p 9292:9292 drassadnikov/ui:1.0

docker network create back_net --subnet=10.0.2.0/24
docker network create front_net --subnet=10.0.1.0/24

docker run -d --network=front_net -p 9292:9292 --name ui drassadnikov/ui:1.0
docker run -d --network=back_net --name comment drassadnikov/comment:1.0
docker run -d --network=back_net --name post drassadnikov/post:1.0
docker run -d --network=back_net --name mongo_db  --network-alias=post_db --network-alias=comment_db mongo:latest

#docker network connect <network> <container>

docker network connect front_net post
docker network connect front_net comment

1) Зайдите по ssh на docker-host и установите пакет bridge-utils
> docker-machine ssh docker-host
> sudo apt-get update && sudo apt-get install bridge-utils
2) Выполните:
> docker network ls
3) Найдите ID сетей, созданных в рамках проекта.
23
Избавляем бизнес от ИТ-зависимости
Bridge network driver
4) Выполните:
 > ifconfig | grep br

 yc-user@docker-host:~$ sudo apt install net-tools
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  net-tools
0 upgraded, 1 newly installed, 0 to remove and 29 not upgraded.
Need to get 194 kB of archives.
After this operation, 803 kB of additional disk space will be used.
Get:1 http://mirror.yandex.ru/ubuntu bionic/main amd64 net-tools amd64 1.60+git20161116.90da8a0-1ubuntu1 [194 kB]
Fetched 194 kB in 0s (7,947 kB/s) 
Selecting previously unselected package net-tools.
(Reading database ... 65264 files and directories currently installed.)
Preparing to unpack .../net-tools_1.60+git20161116.90da8a0-1ubuntu1_amd64.deb ...
Unpacking net-tools (1.60+git20161116.90da8a0-1ubuntu1) ...
Setting up net-tools (1.60+git20161116.90da8a0-1ubuntu1) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
yc-user@docker-host:~$ 

yc-user@docker-host:~$ ifconfig | grep br
br-0de816ba9d9e: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.20.0.1  netmask 255.255.0.0  broadcast 172.20.255.255
br-a6bd2ce16501: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.1  netmask 255.255.255.0  broadcast 10.0.2.255
br-d21eb922c0f1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.1.1  netmask 255.255.255.0  broadcast 10.0.1.255
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet 10.128.0.25  netmask 255.255.255.0  broadcast 10.128.0.255
yc-user@docker-host:~$ 


sudo iptables -nL -t nat

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination         
MASQUERADE  all  --  10.0.1.0/24          0.0.0.0/0           
MASQUERADE  all  --  10.0.2.0/24          0.0.0.0/0           
MASQUERADE  all  --  172.20.0.0/16        0.0.0.0/0           
MASQUERADE  all  --  172.17.0.0/16        0.0.0.0/0           
MASQUERADE  tcp  --  10.0.1.2             10.0.1.2             tcp dpt:9292

Chain DOCKER (2 references)
target     prot opt source               destination         
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
--DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:9292 to:10.0.1.2:9292

yc-user@docker-host:~$ ps ax | grep docker-proxy
 4672 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 9292 -container-ip 10.0.1.2 -container-port 9292
 4679 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 9292 -container-ip 10.0.1.2 -container-port 9292
 8858 pts/0    S+     0:00 grep --color=auto docker-proxy

# docker-compose

sudo apt install docker-compose

docker kill $(docker ps -q) 


export USERNAME=drassadnikov
docker-compose up -d
docker-compose ps


yc-user@docker-host:~$ export USERNAME=drassadnikov
yc-user@docker-host:~$ docker-compose up -d
Pulling post_db (mongo:3.2)...
3.2: Pulling from library/mongo
a92a4af0fb9c: Pull complete
74a2c7f3849e: Pull complete
927b52ab29bb: Pull complete
e941def14025: Pull complete
be6fce289e32: Pull complete
f6d82baac946: Pull complete
7c1a640b9ded: Pull complete
e8b2fc34c941: Pull complete
1fd822faa46a: Pull complete
61ba5f01559c: Pull complete
db344da27f9a: Pull complete
Digest: sha256:0463a91d8eff189747348c154507afc7aba045baa40e8d58d8a4c798e71001f3
Status: Downloaded newer image for mongo:3.2
Creating ycuser_post_1 ... 
Creating ycuser_post_db_1 ... 
Creating ycuser_ui_1 ... 
Creating ycuser_comment_1 ... 
Creating ycuser_post_1
Creating ycuser_post_db_1
Creating ycuser_ui_1
Creating ycuser_post_db_1 ... done
yc-user@docker-host:~$ docker-compose ps
      Name                   Command             State                    Ports                  
-------------------------------------------------------------------------------------------------
ycuser_comment_1   puma                          Up                                              
ycuser_post_1      python3 post_app.py           Up                                              
ycuser_post_db_1   docker-entrypoint.sh mongod   Up      27017/tcp                               
ycuser_ui_1        puma                          Up      0.0.0.0:9292->9292/tcp,:::9292->9292/tcp
yc-user@docker-host:~$ 


yc-user@docker-host:~$ docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                        SWARM   DOCKER    ERRORS
docker-host   -        generic   Running   tcp://51.250.68.228:2376           v23.0.1   
yc-user@docker-host:~$ eval $(docker-machine env docker-host)


export USERNAME=drassadnikov
docker-compose up -d
docker-compose ps

### Базовое имя проекта

- Как образуется базовое имя проекта?: 
<docker_compose_folder>_<service_name>_<incrementing_number_of_containers_with_same_name>
- Как можно его задать? 
Выставить признак container_name для любого контейнера


# ДЗ к monitoring-1


 yc compute instance create --name docker-host --zone ru-central1-a --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 --ssh-key ~/.ssh/yc.pub

docker-machine create --driver generic --generic-ip-address=84.201.158.141 --generic-ssh-user yc-user --generic-ssh-key ~/.ssh/yc docker-host
eval $(docker-machine env docker-host)

docker run --rm -p 9090:9090 -d --name prometheus prom/prometheus

yc-user@docker-host:~$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                                       NAMES
5f31a4ee1554   prom/prometheus   "/bin/prometheus --c…"   16 seconds ago   Up 13 seconds   0.0.0.0:9090->9090/tcp, :::9090->9090/tcp   prometheus

yc-user@docker-host:~$ docker-machine ip docker-host
84.201.158.141

84.201.158.141:9090

export USER_NAME=drassadnikov
docker build -t $USER_NAME/prometheus .

fatal: not a git repository (or any of the parent directories): .git

git init

# ДЗ к logging-1

export USER_NAME=drassadnikov
cd ./src/ui && bash docker_build.sh && docker push $USER_NAME/ui:logging
--yc-user@docker-host:~/src/ui$ docker push drassadnikov/ui:logging

cd ../post-py && bash docker_build.sh && docker push $USER_NAME/post:logging
cd ../comment && bash docker_build.sh && docker push $USER_NAME/comment:logging


yc compute instance create --name logging --zone ru-central1-a --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 --memory 4 --ssh-key ~/.ssh/yc.pub

Установите golang:
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt update
sudo apt install golang-go
go env -w GO111MODULE=off

Установите драйвер для Yandex.Cloud:
go get -u github.com/yandex-cloud/docker-machine-driver-yandex

docker-machine create --driver yandex --yandex-token=y0_AgAAAAALvgvzAATuwQAAAADPy4cGyfqeI7nxQ-ufz30CLGqQszcZTqY  default


echo $PATH
export PATH="$HOME/go/bin:$PATH"
export PATH="$GOPATH/bin:$PATH"


export FOLDER_ID=b1gpf2ca5rpkbvk7phms
export $SA_KEY_PATH= ~/.ssh/key.json

 docker-machine create --driver yandex --yandex-image-family "ubuntu-1804-lts" --yandex-platform-id "standard-v1" --yandex-folder-id $FOLDER_ID --yandex-sa-key-file $SA_KEY_PATH --yandex-memory "4" logging

docker-machine create --driver docker-machine-driver-yandex --yandex-image-family "ubuntu-1804-lts" --yandex-platform-id "standard-v1" --yandex-folder-id $FOLDER_ID --yandex-memory "4" logging

yc-user@fhm98vl8pl26mb650te8:~$ docker-machine create --driver yandex --yandex-image-family "ubuntu-1804-lts" --yandex-platform-id "standard-v1" --yandex-folder-id $FOLDER_ID --yandex-memory "4" --yandex-token y0_AgAAAAALvgvzAATuwQAAAADPy4cGyfqeI7nxQ-ufz30CLGqQszcZTqY --yandex-nat logging

sudo chmod 666 /var/run/docker.sock

# ДЗ к kubernetes-1

kubeadm init --apiserver-cert-extra-sans=85.234.3.77 --apiserver-advertise-address=0.0.0.0 --control-plane-endpoint=85.234.3.77 --pod-network-cidr=10.244.0.0/16


kubeadm join 172.18.28.96:6443 --token sn3b09.dqs8crqdjin6lg7w \
	--discovery-token-ca-cert-hash sha256:27ec14c56795c8895f39bc599934ee11f136c59007928a94cd08977fb0102712 
[root@redos redos]# 

#ДЗ к gitlab-ci

sudo apt-get install ca-certificates curl gnupg lsb-release

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://51.250.77.119'
  ports:
    - '80:80'
    - '443:443'
    - '2222:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'

    ----------------------

yc compute instance create --name gitlab-ci-vm --zone ru-central1-a --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=50 --memory 4 --ssh-key ~/.ssh/yc.pub

c-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ docker ps
CONTAINER ID   IMAGE                     COMMAND             CREATED          STATUS                            PORTS                                                                                                             NAMES
00c35defa105   gitlab/gitlab-ce:latest   "/assets/wrapper"   13 seconds ago   Up 8 seconds (health: starting)   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp, 0.0.0.0:2222->22/tcp, :::2222->22/tcp   gitlab_web_1
yc-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ docker rm 00c35defa105 -f
00c35defa105
yc-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ docker images
REPOSITORY         TAG       IMAGE ID       CREATED      SIZE
gitlab/gitlab-ce   latest    70e5f81b95ee   2 days ago   2.83GB
yc-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ docker rmi 70e5f81b95ee
Untagged: gitlab/gitlab-ce:latest
Untagged: gitlab/gitlab-ce@sha256:a87a43aadb7c574dbbdc5ec85e51902e6a6325c7d82eb392849b886d91883f4a
Deleted: sha256:70e5f81b95ee5470121980c4b609a27ac034ce835b02100d1117b84d2f99076b
Deleted: sha256:a4ce86a50959758046fe1f5b7d1ecef622f7be43fb17d00b83dcb970e40cc5c2
Deleted: sha256:69e979bd5234856166dea9c16bfc5b2bd6ad7624ea6a8cc56e16e93dc8b0fb5a
Deleted: sha256:3efc11edb3eb10ad07ae2df8e9c5ab5dd55e57a9f1141579b415fe32d1b952f6
Deleted: sha256:45b6ea610eff200ab2d04003925d027ce29ae6ca21a6a6c191b67946593e4165
Deleted: sha256:1ec1b0e34454cbf430f022aaea2e1aca584dce67e64437da6ccbe119050e604b
Deleted: sha256:e3b18ff7468a81e22536228c7c1fa7ae5730dde862cb5780b8a708c25b335ae8
Deleted: sha256:93422ab53f18163ad1b5da777a3c84b40b157e9b780b5cfa9c183de9e85e5c31
Deleted: sha256:6021993d84a2d9db89ec74cf44366ae3f2d9f9810f3c77b2bd0cd5ddf14825b1
yc-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ sudo nano docker-compose.yml
yc-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ docker-compose up -d

yc-user@fhmvrockpkbsl0m3gjm7:/srv/gitlab$ sudo nano /srv/gitlab/config/initial_root_password

# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before data$
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: kTiqTADrA/y2FAmP0djrS9osaIuuFXJjHbjxhZwo3fI=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.


git clone http://158.160.51.118/homework/example.git
cd example
[redos@redos example]$ git checkout -b gitlab-ci-1
[redos@redos example]$ git remote add gitlab http://158.160.51.118/homework/example.git
[redos@redos example]$ git push gitlab gitlab-ci-1

[redos@redos example]$ git pull origin gitlab-ci-1
[redos@redos example]$ git add .
[redos@redos example]$ git commit -m "удален"

---------
[redos@redos example]$ git add .gitlab-ci.yml
[redos@redos example]$ git commit -m 'add pipeline definition'
[redos@redos example]$ git push gitlab gitlab-ci-1


 Settings -> CI/CD -> Pipelines ->
Runners и посмотреть на секцию Set up a specific Runner manually :  GR1348941BE458YiokYceozmgpEaY

yc-user@fhmvrockpkbsl0m3gjm7:~$ docker run -d --name gitlab-runner --restart always -v /srv/gitlabrunner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:latest

yc-user@fhmvrockpkbsl0m3gjm7:~$ docker ps
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS                 PORTS                                                                                                             NAMES
5241e63f5a94   gitlab/gitlab-runner:latest   "/usr/bin/dumb-init …"   20 seconds ago   Up 15 seconds                                                                                                                            gitlab-runner
bbe55bb98b84   gitlab/gitlab-ce:latest       "/assets/wrapper"        4 hours ago      Up 4 hours (healthy)   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp, 0.0.0.0:2222->22/tcp, :::2222->22/tcp   gitlab_web_1

docker exec -it gitlab-runner gitlab-runner register --help

docker exec -it gitlab-runner gitlab-runner register --url http://158.160.51.118/ --non-interactive --locked=false --name DockerRunner --executor docker  --docker-image alpine:latest --registration-token "GR1348941BE458YiokYceozmgpEaY" --tag-list "linux,xenial,ubuntu,docker" --run-untagged

#ДЗ к kubernetes-2


dnf install akmod-VirtualBox kernel-devel-5.15.87-1.el7.3.x86_64 akmods --kernels 5.15.87-1.el7.3.x86_64 && systemctl restart vboxdrv.service

kubeadm join 192.168.1.62:6443 --token jha11m.kwdagynrv2gaunmr \
	--discovery-token-ca-cert-hash sha256:9b002db4c9f30d79f512c72edfa57d88bcb34a6340778ca160146ce81291fd77

  [root@redos redos]# kubeadm token create --print-join-command
kubeadm join 192.168.1.62:6443 --token 548hia.hldfbw8vqcwuutbx --discovery-token-ca-cert-hash sha256:9b002db4c9f30d79f512c72edfa57d88bcb34a6340778ca160146ce81291fd77

[root@redos redos]# kubectl apply -f https://docs.projectcalico.org/archive/v3.20/manifests/calico.yaml

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJek1ETXhNakEwTURReU1Gb1hEVE16TURNd09UQTBNRFF5TUZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTCt4CkZnYVBUaUIvSDNUazYxeHUrMmEyMGFTakoyNHBjVXNlWEQ3TmVVaUF6M0xpS0VRbjJTU0RmZzBSNWFaeVBLMy8KbDdWcUZwMFpzT3h3YkFRZS9YdHYzOW5lNkpwd3JNN2puTDA0N0JFZnVZYkhwNXd6Zkowa0trMlpIZXdtNkc3awpDWEovL3NUSkNndGp3dXJCTEpnYmlmRVBVWGlkVkpkWGsxa24xZjhhYmE5VXB6S1BVanI5czhzb2h1L2E3SjdXClMrY1RmYUdXV1RmRFg3aVUrNE1YSWVaREtLR0V0WFFSY2VTVTZCUTQ4ajJWMXRYRjlpemMwL3BzSGxISlU4eDUKNzJDQUZnSGdYM0l1ZkZISlUxSWcrNkFNUklta2JjdXVtK3o0anJkNzJsYmRtMXlRVk4xRGVJMWovWm15M0FMUAozYlZYNHBzdWtFQ005ckpUSnlVQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZET1AyNTBmNEVhZGF2eWdETW1ocjlIZzNsYWpNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFCUkFYVGZKTFoxWXMvdHZ0N1JKUlpqdzFlRVJ6QWV2S3M2cTJCTk9jeDg4S0VqZldiSgpFVkh4UUhhbmlYOHgxNzNuMUZ1cnZQNHh4ZGU3N25RNDE5b2hMYmhEaFBrN0l0UmJod1R6Nlo5UFc2ZGZDWTJTCjVESEdyME1OK3JQbVVjVXR4ZG1HbTRoOU9teXRjWnM5ZTVQK204R1h1cUt4bUpiZ1BwSUpOT2FLcWRoZ3RWWGkKRkdla2p5YXMwSjFuMDhuTG9mREEzNnlxbWN6T2FmRDEwMERENmR6OUlpMFRKcnpsUDhiN0ZKQWdzTXRRS3A1RApmbldLRTRjSjJ6eHdScWtSR1J3YVV3Z3F2RWR0RE9wQzRzNlVGTnlKQmgyZnJUZGdhdnVYOGZEYmdpYzNpRHpxCkpiVDVGWnE3czdoOEV6UFNaN1lWdTBCcHVxdVkyZXRacHZKSAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    server: https://192.168.1.62:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURJVENDQWdtZ0F3SUJBZ0lJREMxMm9leVlFMmd3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TXpBek1USXdOREEwTWpCYUZ3MHlOREF6TVRFd05EQTBNakphTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQTU0bCsrL0cvaytXamxxaFYKdW01ZFFMMlMzRnhwTmNTb1FYUVBhODg1dU5aMWZqQjZDN01UY09DaDd0cnllV3J1MU0wOVBvVDFPN1I3ZFpOcwozNmN3NXM0YkhoL1lGa1VPb2M5c3k1bkJxSitlaVhibUdrYytTanlNc0RYVnpEVm5SS1V2SGxmRFlKRHBLcGVGCktUSGRndndBaTdGZlRVajNlUCswckM0Rnp0dFBQSngxSUFqNEYwQVA4TXNrcGduNkdTc2g2dUdsaUlrVVk4eHEKaUtjRVI5ck5uc0xyc0FVTHBnZjl1NXRTZHJNZ1lkT3hjek4rTkFBUVNQcG9LZ2RxekFrZDlrQmh0LzlFSHFvaApuK2k1THVrRHYwYkF4RDVZQmJZeEE0amtzbWdMMzNxQVB6bmtreUdyc013VVFaNlEzVWczRml4MTdLdllKZ01GClBaUU5EUUlEQVFBQm8xWXdWREFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RBWURWUjBUQVFIL0JBSXdBREFmQmdOVkhTTUVHREFXZ0JRemo5dWRIK0JHbldyOG9BekpvYS9SNE41VwpvekFOQmdrcWhraUc5dzBCQVFzRkFBT0NBUUVBaU1FSWJVS0dKYldnYWlWVWJCdkd1V3VzTm80MGR2SlJ4cGNnCnBEN3VCRzhTejZDZnllalFzNlhwZVl4NTNCWm5IaUxTZ1Q3SmpkaWE4dFd4VjZOU29nMWttcDh0YTZtNHgrY1gKTUlDS0RXYytZdUNEVU4xUWgwcks1MEc1MUN1U21yalJiZEZCcG5MWlQrWHhhZEJWK25CUHBpcjFrR3ZZTXFxSwo0WTEzMERvKzVhM0FZUXA2bUxKYUJwWFp1QVZFajF1MmZ3eG1MNjh0SWNrM21yVzNLUUdCL0pBNDNBVFVoZUNPCmcraGY4eE54eXNKcnMrRVlwaUpRaGY0Y1VjS2Z3eWpLS0JiZnJPcEJZRDlhb1B6Zmp5b0NFcmptYTZWa2VYaUQKWkk2S3QrZXBGY25SdjRkMERGRXVxQzJxYVRwNXAvMlFBQUx2dmJlMjFQejd3ZkZ1OGc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBNTRsKysvRy9rK1dqbHFoVnVtNWRRTDJTM0Z4cE5jU29RWFFQYTg4NXVOWjFmakI2CkM3TVRjT0NoN3RyeWVXcnUxTTA5UG9UMU83UjdkWk5zMzZjdzVzNGJIaC9ZRmtVT29jOXN5NW5CcUorZWlYYm0KR2tjK1NqeU1zRFhWekRWblJLVXZIbGZEWUpEcEtwZUZLVEhkZ3Z3QWk3RmZUVWozZVArMHJDNEZ6dHRQUEp4MQpJQWo0RjBBUDhNc2twZ242R1NzaDZ1R2xpSWtVWTh4cWlLY0VSOXJObnNMcnNBVUxwZ2Y5dTV0U2RyTWdZZE94CmN6TitOQUFRU1Bwb0tnZHF6QWtkOWtCaHQvOUVIcW9obitpNUx1a0R2MGJBeEQ1WUJiWXhBNGprc21nTDMzcUEKUHpua2t5R3JzTXdVUVo2UTNVZzNGaXgxN0t2WUpnTUZQWlFORFFJREFRQUJBb0lCQUc3T2Vwc1FndUxBejUxVwpTbERDYUphSEl6V2FkQzlyUWlxdzVJQnYxK3dCbHBFaG1nYm5XTEo0am9iRSthM3A0d3FzZmxiaFFvdWtRRUZ3Cm9IWVlpV3FyMElhR0x0L1poTHNqamFtU2wvK2ZCRHc5VHJuY3hvNjRrNHZ3OTdTWENpanI5TFRNdzQvL1NkYzgKVkZuMnAwLzhVamFJV0ZlZ1IrNzhVUGJsdjVuU3FmdkdkaG5kcUlrRzJNb01SRXN5Q25UdEl2ZUkzQzltaXpJbQpOS3oxOFlabkRaUlJtTURyRGFXcVk0ODVHNmhic3hGanhraXRydGhvcXhpd2VUSGczRm00bitEV1ZLMHFkbUpSCmJLVUNObDBWUjJpQUdxb1IyTkliOGxmNVFrOGVBcEFhRWU5VzNCd3dCVktyUUlIdU9qT0RiSEZMQTJtVUtFL0UKR0RrS0RJRUNnWUVBOVVTRkJqU1hBSGxoYi9PNC9xb0ZGT0VkbExlazlpRTFla3liNSt1dVJxcklPdjhzYVRZbApscVcvb2NUSjlaMzl1QlRIUmQzSG1WZUZjbENtNDNkZ1BUaW9oNUIxMVhtelBSaHZDdlg1WnZQb1kvM0E2dnJwClYxdDZkSUNDNUVWUWRZbEtQb0x0a2RZWlgxeVlGSlo1T1Q1SXhJSnNnWms4dTZLUk1mWXZKZVVDZ1lFQThhc3EKeDF6bTdid2JpSEQvbmdudzd2VHVISTVVTWRWMk1FVVZHRC8zV1dEdHRsaXVabUNqQ1IrUFhFOWppbnBMRXh4eAptaEp2TWdheXJlUkFMTUZVQ1NhdVlITWdadTRGTnpVdWRqMXJVNnc3VXAvdVdkMjd3ZnE5UURmV0ZrdytwMzRsClJTYjBvZndzbjhIdWJqMmVnRkh1NVJxSFpwZWJ3bmZxajU4QldBa0NnWUVBOEorUWdrNEY5d2tlZHQ3OWw1cmwKOFY0SnoyVjhDWno3QWtrMmk0bkZLTDlVUWMwbW5QSHFYcW11SDk4WTVFZGtLN3oyNDZ4NXJnOFhkTmQ3WTU0eQpaTjI1T1lhWWxCOFpvYzdlNGpuL3ZPbCtETnRlOFNuSTAxT0VCOWdza2hjT29NRllmWXVsMTNYYzNwblEraUhHClBFckd5VVBMZ0RuK1EzZHlTem5qZDZFQ2dZQW1RdnRhNVJLS3dTVjZ6S2tyMUZjWS9oNVUxeFB0YitadWJnR1EKL1Ura0R2eVR5aWFTZnVwUkgzWUxIMmFiSGhHVXpRUVBhS3ZDTjkxQ09za09UTzJKSlY2bVZwUGl0L3lMYVJnYQpFRTlWeUFiOFplWE94SlJkZWQxTXRZcG5yVnFlR2hLOGlCWmpMeEhCbVdxdWVZTUd2Zkljdzc1OE43U3BiV0x0CnFqY0VRUUtCZ1FEc0hXSENlY1hMZlA4Tk1ueU5wTUZMa3ZMU0dBRzdXdlNKRDU5cGNobjBKU0QzV2tsV2FyT0QKTGV1K05wOXFkMVAvNmFqaEtmNVVIaW1LeEJBdW9mODg0YnFMZVFHTys0b0FIUjQvL1NGYUFGRjdDN3VjWGx1KwpBTGZrL1pDNEhiRjFOYzluOFE0OEhNT0dQK1FrdlczemRWRzUzQ0wxMHdLVE1iYVRuS2lSaHc9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=

#ДЗ к kubernetes-3


$ kubectl get services -n dev
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
comment ClusterIP 10.11.248.14 <none> 9292/TCP 1d
comment-db ClusterIP 10.11.252.16 <none> 27017/TCP 1d
post LoadBalancer 10.11.240.67 35.193.193.168 5000:30298/TCP 1d
post-db ClusterIP 10.11.242.112 <none> 27000/TCP 18m
ui NodePort 10.11.253.107 <none> 9292:30221/TCP 1d

Обновите mongo-network-policy.yml так, чтобы post-сервис дошел до
базы данных


yc compute disk create --name k8s --size 4 --description "disk for k8s"

yc compute disk list


apiVersion: v1
kind: PersistentVolume
metadata:
 name: mongo-pv
spec:
 capacity:
 storage: 4Gi
 accessModes:
 - ReadWriteOnce
 csi:
 driver: disk-csi-driver.mks.ycloud.io
 fsType: ext4
 volumeHandle: fhmhmk31eu4kjhopi7a1
