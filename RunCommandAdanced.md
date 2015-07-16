Doccker - Run Commands Advanced
-------------------   

Look at docker hub
check out ubuntu, centos and fedora

**Search dockerhub** 

    docker -v 
    history
    docker search ubuntu
    docker search -s 10 ubuntu
    docker search --no-trunc=true -s 10 ubuntu
    
    docker images
    docker pull ubuntu
    docker pull ubuntu:14.04
    docker pull ubuntu:trusty
    docker images

( explain all 5 tags and tagging)

    docke
    docker ps    (look at what happened.. ran /bin/bash)

    docker run -itd ubuntu /bin/sh
    docker ps  (look at what happened.. ran /bin/sh)
    
    docker history <image id>   (Show the CMD) 

    docker run -itd ubuntu uname -a
    docker ps -a  (look at what happened..no in the CMD STDIN gone)
    docker logs <cid>

    docker run -itd ubuntu sleep 10 && watch docker ps

**cleanup**

    docker kill $(docker ps -q) && docker rm $(docker ps -aq)
   
    docker run -itd --name job1 ubuntu /bin/sh -c "while true; do echo Docker Rocks; sleep 1; done"
    
    docker logs -h
    docker logs -ft job1
 
    docker kill job1
    docker rm job1 
    docker ps -a

    docker run -itd --name job2 ubuntu /bin/sh -c "while true; do echo Docker Rocks; sleep 1; done"
    docker kill $(docker ps -lq)  (stop the last container) 
    docker rm $(docker ps -lq)
    
**Fun with PIDS**

    docker run -itd --name job2 ubuntu /bin/sh -c "while true; do echo Docker Rocks; sleep 1; done"
    docker stats job2
    watch docker top job2 -ef  (watch the pid change) 
    docker exec -itd job2 sleep 20
    watch docker top job2 -ef
    
**Using Inspect ** 

    docker inspect -h 
    docker inspect job2
    docker inspect --format '{{.Name}} {{.State.Running}}' job2
     
**Fun with 1.6 **

    docker ps -a
    docker run  -itd --name job4 --label=NodeNumber=3 --label=NodeType=cluster ubuntu
    docker inspect job4 > jmw.out  (find /Labels) 
    docker inspect --format '{{.Name}} {{.Config.Labels.NodeType}}' job4
    
    cid=$(docker run -itd ubuntu)
    docker attach $cid
    ulimit -a
    cid=$(docker run -itd --ulimit nofile=1024:1024 ubuntu)
    docker attach $cid
    ulimit -a
    
```    
cid=$(docker run -itd --ulimit nofile=1024:1024 --ulimit core=102400 --ulimit nproc=1000 --ulimit nice=100 --ulimit memlock=8196 --ulimit fsize=8192 --ulimit rss=4096 --ulimit cpu=4 --ulimit locks=1000 --ulimit sigpending=100 --ulimit msgqueue=1000 --ulimit nice=100 --ulimit rtprio=100 ubuntu)

docker attach $cid
ulimit -a
```
    
