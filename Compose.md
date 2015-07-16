Docker Compose
-------------------

### Installation on OS X

**Install Compose** 

```
sudo wget --no-check-certificate https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m`

sudo mv docker-compose-`uname -s`-`uname -m` /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

**Install Docker (client)  (if not already installed)** 

```
sudo wget --no-check-certificate -O /usr/local/bin/docker https://get.docker.com/builds/Darwin/x86_64/docker-latest

sudo chmod +x /usr/local/bin/docker

docker -v
```

**Canonical Docker-Compose (Python/Redis) example**

docker-compose.yml

```
web:
  build: .
  command: python app.py
  ports:
   - "5000:5000"
  volumes:
   - .:/code
  links:
   - redis
 redis:
  image: redis
```

Dockerfile

```
FROM python:2.7
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
```

app.py

```
from flask import Flask
from redis import Redis
import os
app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    return 'Hello World! I have been seen %s times.' % redis.get('hits')

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

requirements.txt 

```
flask
redis
```

Commands

```
docker-compose up -d

docker-compose ps

docker-compose logs

curl localhost:5000

docker-compose stop
```

**Tomcat Sample example**

```
docker pull tomcat

docker history tomcat | grep -i expose

cid=$(docker run -d -P tomcat)

docker ps

curl localhost:32xxx 

docker kill $cid

docker rm $cid
```

**Now let's Compose it...**

compose-ex1.yml

```
tomcatapp:
  image: tomcat
  ports:
   - "8080"
```

Commands

```
docker-compose -f compose-ex1.yml up -d

docker-compose -f compose-ex1.yml ps

docker-compose -f compose-ex1.yml logs

curl localhost:32xxx 

docker-compose -f compose-ex1.yml stop

docker-compose -f compose-ex1.yml rm
```

**Now let's Compose it... (add a sample.war file)**

Figure out where the webapps directory is

```
docker run -it tomcat bash 

ls   
```

nginx.conf

```
worker_processes 1;

events { worker_connections 1024; }

http {

    sendfile on;

    gzip              on;
    gzip_http_version 1.0;
    gzip_proxied      any;
    gzip_min_length   500;
    gzip_disable      "MSIE [1-6]\.";
    gzip_types        text/plain text/xml text/css
                      text/comma-separated-values
                      text/javascript
                      application/x-javascript
                      application/atom+xml;

    # List of application servers
    upstream app_servers {

        server tomcatapp1:8080;
        server tomcatapp2:8080;
        server tomcatapp3:8080;

    }

    # Configuration for the server
    server {

        # Running port
        listen [::]:80;
        listen 80;

        # Proxying the connections connections
        location / {

            proxy_pass         http://app_servers;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;

        }
    }
}
```

compose-ex2.yml

```
nginx:
  image: nginx
  links:
   - tomcatapp1:tomcatapp1
   - tomcatapp2:tomcatapp2
   - tomcatapp3:tomcatapp3
  ports:
   - "80:80"
  volumes:
   - nginx.conf:/etc/nginx/nginx.conf
tomcatapp1:
  image: tomcat
  volumes:
   - sample.war:/usr/local/tomcat/webapps/sample.war
tomcatapp2:
  image: tomcat
  volumes:
   - sample.war:/usr/local/tomcat/webapps/sample.war
tomcatapp3:
  image: tomcat
  volumes:
   - sample.war:/usr/local/tomcat/webapps/sample.war
```

Commands

```
export COMPOSE_FILE=compose-ex2.yml

docker-compose up -d

docker-compose ps 

docker exec composetest_nginx_1 cat /etc/hosts

docker exec composetest_tomcatapp1_1 ip a 

docker exec composetest_tomcatapp2_1 ip a

docker exec composetest_tomcatapp3_1 ip a

curl http://localhost/sample/

docker-compose stop
```

