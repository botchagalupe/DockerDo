Docker Swarm
-------------------

**Setup** 

```
docker-machine create --driver virtualbox dev1

eval "$(docker-machine env dev1)"

docker pull swarm

docker history swarm

docker run swarm     #(get help)
```

**Create a Cluster** 

```
sid=$(docker run swarm create) 

files $ echo $sid
1ffdb70193793b943df9456a35c24817
```

**Create the Swarm Manager**

```
docker-machine create -d virtualbox --swarm --swarm-master --swarm-discovery token://$sid swarm-master

docker-machine ls

eval "$(docker-machine env swarm-master)"

docker-machine ls

docker info
```

**Create Swarm Nodes**

```
docker-machine create -d virtualbox --engine-label itype=frontend --swarm --swarm-discovery token://$sid swarm-node-01

docker-machine create -d virtualbox --swarm --swarm-discovery token://$sid swarm-node-02

docker-machine create -d virtualbox --swarm --swarm-discovery token://$sid swarm-node-03

docker-machine ls

docker-machine env --swarm swarm-master   # (checkout the different port)

eval "$(docker-machine env --swarm swarm-master)"

docker-machine ls    (Notice non of the docker machines have the asterick) 

docker info

docker run swarm list token://$sid

docker ps     #(no containers are running in the swarm)
```

**Look at the Four Nodes**

```
docker-machine ls

eval "$(docker-machine env swarm-master)"

docker ps

eval "$(docker-machine env swarm-node-01)"

docker ps

eval "$(docker-machine env swarm-node-02)"

docker ps

eval "$(docker-machine env swarm-node-03)"

docker ps
```

**Running Docker Instances with Swarm  (explain Spead vs Binpack**

```
eval "$(docker-machine env --swarm swarm-master)"

docker ps 

docker run -itd --name engmgr ubuntu 

docker ps 

for i in `seq 1 6`; do docker run -itd -e constraint:itype!=frontend --name eng$i ubuntu; done

docker ps 

docker run -itd --name engmgr-c -e affinity:container==engmgr ubuntu
```

**Cleanup**

```
docker-machine kill $(docker-machine ls -q)

docker-machine rm $(docker-machine ls -q)
```
