# ubuntu-squid-iptables
#### Install Docker:
```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

#### Pull Docker Image from DockerHub:
```
docker pull ubuntu/squid
```
#### create a Docker container for Squid proxy.
```
sudo docker run --name squid-proxy -d --restart=always -p 3128:3128 ubuntu/squid
```
This command creates a container named "squid-proxy" using the Squid proxy Docker image from the sameersbn Docker Hub repository. It maps the container port 3128 to the host port 3128, which is the default port for Squid proxy.

#### Configure the Squid proxy container to allow access from the host network.
```
docker exec -it squid-proxy /bin/bash
echo "http_access allow all" >> /etc/squid/squid.conf
service squid reload
exit
```
This command enters the container shell, adds an access rule to the Squid configuration file to allow all traffic, and reloads the Squid service.

#### Set up the iptables rules for the transparent proxy.


Create a new iptables rule that allows traffic to the Squid proxy container. For example, to allow incoming traffic on port 3128 to the container with IP address 172.17.0.2, you can run the following command:
```
iptables -A INPUT -p tcp --dport 3128 -d 172.17.0.2 -j ACCEPT
```
This command adds a new rule to the INPUT chain of the iptables firewall, which accepts incoming TCP traffic on port 3128 and forwards it to the container with the specified IP address.

Save the new iptables rule to make it persistent across reboots. Depending on your operating system, you can use different tools to do this. For example, on Ubuntu, you can use the iptables-persistent 

```
sudo apt-get install iptables-persistent
sudo service iptables-persistent save
```

#### Network configuration in browser

``` 
Open Browser > settings > Network settings > settings > Manual proxy configuration
```
here give squid ip address and port
    
#### check the logs
path: /var/log/squid/
```
tail -f access.log
``` 

