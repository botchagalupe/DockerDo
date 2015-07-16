Docker Dockerfile 
-------------------

### ON UBUNTU 14-10

**Basic Three Commands FROM, RUN and CMD** 

apache-ex1

```
FROM ubuntu:14.04

RUN apt-get -y install apache2 

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

Commands

```
docker build -f apache-dockerfile-ex1 -t apache-ex1 .

docker images
    
cid=$(docker run -itd apache-ex1)
    
docker exec $cid ip a

nid=$(docker inspect --format '{{.NetworkSettings.IPAddress}}' $cid)
    
curl $nid
```

**Caching** 

```
docker build -f apache-dockerfile-ex1 -t apache-ex1 .
    
docker build --no-cache=true -f apache-dockerfile-ex1 -t apache-ex1 .
```

**Alternate Syntax** 

apache-ex2

```
FROM ubuntu:14.04

RUN ["apt-get","install","-y","apache2"]

CMD /usr/sbin/apache2ctl -D FOREGROUND
```

**Finding index.html** 

```
docker run -it apache-ex1 /bin/sh

find / -name index.html
```

**More Commands ADD and EXPOSE** 

apache-ex3

```
FROM ubuntu:14.04

RUN \
  apt-get update && \
  apt-get -y install apache2 

ADD index.html /var/www/html/index.html

EXPOSE 80 

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

index.html

```
<h1>Docker Rocks!</h1>
```

Commands

```
cat index.html 
    
docker build -f apache-dockerfile-ex3 -t apache-ex3 .
    
cid=$(docker run -itd -P apache-ex3)
    
nid=$(docker inspect --format '{{.NetworkSettings.IPAddress}}' $cid)
    
curl $nid
```

Show from Web Browser 

```
ip a show eth1

docker ps -a
```

**Volume Command** 

apache-ex4

```
FROM ubuntu:14.04

VOLUME [ "/var/www/html" ]

ADD index.html /var/www/html/index.html

RUN \
  apt-get update && \
  apt-get -y install apache2 

EXPOSE 80 

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

Commands

```
docker build -f apache-dockerfile-ex4 -t apache-ex4 .
    
cid=$(docker run -itd -v ~/test/index.html:/var/www/html/index.html wordpress-ex4)
    
nid=$(docker inspect --format '{{.NetworkSettings.IPAddress}}' $cid)
    
curl $nid
```

Modify index.html  

    curl $nid
    
**Final Touches** 

apache-ex5

```
FROM ubuntu:14.04

MAINTAINER John Willis  <john@socketplane.io>
ENV REFRESHED_AT 2016-04-20 

VOLUME [ "/var/www/html" ]
WORKDIR /var/www/html

ADD index.html /var/www/html/index.html

RUN \
  apt-get update && \
  apt-get -y install apache2 

EXPOSE 80 

ENTRYPOINT ["/usr/sbin/apache2ctl"]
CMD ["-D", "FOREGROUND"]
```

Show Using Variable for Refresh

```
docker build -f apache-dockerfile-ex5 -t apache-ex5 .
    
docker build -f apache-dockerfile-ex5 -t apache-ex5 .
```

Modify REFRESH VARIABLE in the Dockerfile

```
docker build -f apache-dockerfile-ex5 -t apache-ex5 .
```    

