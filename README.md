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


docker-machine create --driver generic --generic-ip-address=51.250.70.85 --generic-ssh-user yc-user --generic-ssh-key ~/.ssh/yc  docker-host eval $(docker-machine env docker-host)

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
> 
>     chmod +x /tmp/docker-machine &&
> 
>     sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
yc-user@fhm3eh2q3kdvid2nvp3t:~$ docker-machine --version
docker-machine version 0.16.2, build bd45ab13

yc-user@fhm3eh2q3kdvid2nvp3t:~$ docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                       SWARM   DOCKER    ERRORS
docker-host   *        generic   Running   tcp://51.250.70.85:2376           v23.0.1   


docker run --rm -ti tehbilly/htop
docker run --rm --pid host -ti tehbilly/htop
