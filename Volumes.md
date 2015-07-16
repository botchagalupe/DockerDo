 Docker Volumes 
-------------------   
**setup**

```
FROM ubuntu:14.04

MAINTAINER John Willis  <john@socketplane.io>

VOLUME ["/john99"]

CMD ["/bin/sh"]
```
    docker build -f myimage.dockerfile -t myimage .
    
**Volumes**

(***Simple Run***)

    docker run -it -v /john1 busybox
    cd john1
    touch file1
    ctrl-p-q   # Keep in running 
    
    docker restart <cid>
    docker exec <cid> ls /john1

  The file stays until we remove the container  
  however, if we start a new container based on the orgi busybox image

    docker run -it -v /john1 busybox
    cd john1
    ls  (not there)   This was a new run..
    exit

  We can also have volumes that have volumes defined in the imagee build.. 

    docker images   (show my image)  
    docker inspect <imageid>
    docker history <imagename>

    docker run -itd myimage
    cd mydir 
    ls    (file1 will alwys be there)
    touch file2  (same rues apply) 
    ls
    exit

    docker run -it -v /john1 myimage
    ls   (both directories) 
   
   cd into both directories

   Switch back to the docker host.. 
   
```   
mkdir john3
cd john3
touch file3
touch file4
   
docker run -it -v /vagrant/john3:/john3 myimage    (host needs to be abs path)
```    

  ## good for testing src code ##
  
```
cd john3
ls
touch file5
ls
exit
```

  on docker host.. 
  
```
cd john3
ls   (see files 3 and 4 and 5)
 
docker run -it -v ~/john3:/john3:ro myimage   (point out ~) 
cd john3
vi file5
save???

docker run -it -v ~/.bash_history:/.bash_history myimage

docker ps -a   (see what's running)
docker kill $(docker ps -q)
docker rm $(docker ps -aq)   
docker ps -a

docker run -it --name john1 -v ~/john3:/john3 myimage
ls  (directories are there.. and files trust me) 
### ctrl-pq   (keep running) 

docker ps

docker run -it --name john2 --volumes-from john1 myimage
ls
###  ctrlpq

docker run -it --name john3 --volumes-from john2 myimage

***(make a backup)**

docker run --rm --volumes-from john1 -v $(pwd):/backup busybox tar cvf /backup/john2.tar /john
```

