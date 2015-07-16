Docker Run Command Basics
-------------------   

**Run first container** 

    docker ps -a
    docker run busybox 
    docker ps
    docker ps -a
    docker run -i busybox  (STDIN)
    ls
    pwd
    hostname

**(Crtl-d) to cancel**

    docker ps
    docker ps -a
    docker run -t busybox

**Note: a ctrl-c doesn't kill it** 

    docker ps
    docker ps -a
    docker run -it busybox

**(basic Shell)** 

    hostname
    ps -ef 
    exit 
    docker ps
    docker ps -a


    docker run -it busybox
    hostname
    ps -ef 

**ctrl-p-q (quits without killing the container)**

    docker ps
    docker ps -a
    docker run -d busybox

**(It's gone)**

    docker ps
    docker ps -a


    docker run -itd busybox
    docker ps 
    docker attach xxxx
    ls

**ctrl-p-q (quits without killing docker attach the container***

    docker ps --no-trunc=true
 
 **(long CID)** 

    cid=$(docker run -itd busybox)
    echo $cid
    docker inspect $cid
    docker inspect --format '{{.NetworkSettings.IPAddress}}' $cid
    docker stop $cid

**Or.. .use the name** 

    docker run --name john1 -itd busybox
    docker attach john1
    docker stop john1
    docker run -itd busy box
    docker run -itd busy box
    docker run -itd busy box
    docker run -itd busy box
    docker run --name john2 -itd busybox
    docker run --name john3 -itd busybox
    docker ps -q 


     docker ps 
     docker rm <cid>
     docker rm $(docker ps -aq)
     
