## Getting Started with Libnetwork ##

# Install

```

sudo apt-get install -y unzip

wget -qO- https://experimental.docker.com/ | sh

cd /usr/bin
sudo wget https://dl.bintray.com/mitchellh/consul/0.5.2_linux_amd64.zip
unzip *.zip
rm *.zip

# On host 1

sudo /usr/bin/docker -d --kv-store=consul:localhost:8500 --label=com.docker.network.driver.overlay.bind_interface=eth1

sudo /usr/bin/docker -d --default-network=overlay:multihost --kv-store=consul:192.168.33.10:8500 --label=com.docker.network.driver.overlay.bind_interface=eth1

echo DOCKER_OPTS=\\"--default-network=overlay:multihost --kv-store=consul:192.168.33.10:8500 --label=com.docker.network.driver.overlay.bind_interface=eth1 \\" >> /etc/default/docker

service docker restart


consul agent -server -bootstrap -data-dir /tmp/consul -bind=10.254.101.22 &

# Ohost 2

sudo /usr/bin/docker -d --kv-store=consul:localhost:8500 --label=com.docker.network.driver.overlay.bind_interface=eth1 --label=com.docker.network.driver.overlay.neighbor_ip=10.254.101.21


echo DOCKER_OPTS=\\"--default-network=overlay:multihost --kv-store=consul:192.168.33.10:8500 --label=com.docker.network.driver.overlay.bind_interface=eth1 --label=com.docker.network.driver.overlay.neighbor_ip=192.168.33.11\\" >> /etc/default/docker

service docker restart

consul agent -server -bootstrap -data-dir /tmp/consul -bind=10.254.101.22 &

consul agent -data-dir /tmp/consul -bind=10.254.101.22 &

consul join 10.254.101.21

```

# On Machine 1

```
docker network create -d overlay prod 

docker network ls

docker network info prod

docker service publish db1.prod

cid=$(docker run -itd --net=none ubuntu)

docker service attach $cid db1.prod

docker exec -it $cid sh 
cat /etc/hosts

```

# On Machine 2

```
docker network info

docker network ls

cid=$(docker run -itd --net=none ubuntu)

docker service publish db2.prod

docker service attach $cid db2.prod
```
