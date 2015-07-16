Docker Machine
-------------------

### Installation on OSX

**Install Machine** 

```
sudo wget --no-check-certificate -O /usr/local/bin/docker-machine https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_darwin-amd64

sudo chmod +x /usr/local/bin/docker-machine

docker-machine -v
```

**Install Docker (client)** 

```
sudo wget --no-check-certificate -O /usr/local/bin/docker https://get.docker.com/builds/Darwin/x86_64/docker-latest

sudo chmod +x /usr/local/bin/docker

docker -v
```

### Running Machine on Virtualbox

```
docker-machine create --driver virtualbox dev1

docker-machine ls

eval "$(docker-machine env dev1)"

docker run busybox echo hello world

docker ps -a

```  

### Running Machine on Digital Ocean

```
docker-machine create --driver digitalocean --digitalocean-access-token=$DIGITAL_OCEAN_TOKEN dev2

docker-machine ls

eval "$(docker-machine env dev2)"

docker-machine ls   (show the active machine)

docker run busybox echo hello world

docker ps -a
```  

### Running Machine on Amazon EC2

```
docker-machine -D create --driver amazonec2 --amazonec2-access-key $AWS_ACCESS_KEY_ID --amazonec2-secret-key $AWS_SECRET_ACCESS_KEY --amazonec2-vpc-id $AWS_VPC_ID --amazonec2-zone b dev3

docker-machine ls

eval "$(docker-machine env dev3)"

docker-machine ls   (show the active machine)

docker run busybox echo hello world

docker ps -a
``` 

