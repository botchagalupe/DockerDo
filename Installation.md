Docker Installation
-------------------

### ON UBUNTU 14-10

**Repo install usually back leveled** 

    sudo apt-get install -y docker.io  #
    sudo usermod -aG docker vagrant
    docker info
    docker -v 
    docker version
    sudo service docker restart

**Use (for latest)** 

    wget -qO- https://get.docker.com/ | sh

**Pre release**

    wget -qO- https://test.docker.com/ | sh

**Basic Commands**

    docker info
    docker -v 
    docker version
    sudo service docker restart

### Centos 7  

**Repo install usually back leveled** 

    sudo yum install docker
    sudo service docker start
    sudo chkconfig docker on #(start at boot)
    docker info
    docker version
    sudo service docker restart

**Alternativly (for latest)** 

    sudo wget https://get.docker.com/builds/Linux/x86_64/docker-latest -O /usr/bin/docker
    sudo chmod +x /usr/bin/docker
    sudo /usr/bin/docker -d &
    sudo docker info
    sudo docker version
    
**( a plane install, no docker conf.. not startup configs..)**

### On Fedora 20 

    sudo yum -y remove docker
    sudo yum install docker-io
    sudo service docker start
    sudo chkconfig docker on (start at boot)

