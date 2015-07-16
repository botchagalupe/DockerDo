Docker Basic Networking 
-------------------  

** Setup **

     sudo apt-get install -y bridge-utils curl
     docker run -d --name db -p 3306:3306 mysql
     docker run -d --name wp1 -p 80:80 wordpress
     docker run -d --name wp2 -p 81:80 wordpress

**Commands**

     ip a  ( explain eth0=nat eth1 my ip, docker0= bridge)  (docker0 sunset /16 65k)

    ip a show docker0

**Explain docker0 (this is a virtual bridge and the IP is the gateway for all contianers
 on this docker host)**

**172.17.42.1/16**

     docker run -itd â€”name u1 ubuntu
     docker exec u1 ip a

     ip a 

     brctl show docker0  (explain veth each container has a eth0 veth pair)

     docker run -it â€”name u2 ubuntu
     sudo apt-get update
     sudo apt-get install -y inetutils-traceroute

     traceroute docker.com

**(look at iptables nat configuration)**
     sudo iptables -t nat -L -n  (docker also setup masquerade rule for all container traffic). 
**allows any outbound but by default no inboundâ€¦(show the masa rule)**

**(these rules are configured dynamically by docker based on how you use the EXPOSE** 
     docker history httpd   (look at the expose cmd for port 80)

     docker run -itd --name web1 -P httpd

     docker exec web1 ip a   (look at the ip address)

     sudo iptables -t nat -L -n

     curl localhost:32768

     docker run -itd --name web2 -p 80 httpd

     sudo iptables -t nat -L -n   (look at the new DNAT rule)

     docker run -itd --name web3 -p 8080:80 httpd

     sudo iptables -t nat -L -n   (look at the new DNAT rule)

     curl localhost::8080

**on the LAMP stack setup docker host**

     docker ps (look at my three machinesâ€¦ ) 

     ip a  ( explain eth0=nat eth1 my ip, docker0= bridge)  (docker0 sunset /16 65k)

     brctl show docker0  (explain veth )

     sudo iptables -t nat -L -n  (docker also setup masquerade rule for all container traffic). 

**allows any outbound but by default no inboundâ€¦(show the masa rule)**

     ip addr show eth1

**hit the web pageâ€¦ at 80 then at 8080 then 1936**

     cat /etc/haproxy/haproxy.cfg

     docker run -d --name wp3 -p 82:80 wordpress

     docker exec wp3 ip a

     sudo vi /etc/haproxy/haproxy.cfg   (add new interface)

     sudo service haproxy reload 

**hit 1936**

