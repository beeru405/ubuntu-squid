# ubuntu-squid
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

#### Configure the Squid proxy container to allow access from the host network.
```
docker exec -it squid-proxy /bin/bash
echo "http_access allow all" >> /etc/squid/squid.conf
service squid reload
exit
```

#### Test the transparent proxy by accessing a website from the host network.
```
curl -L -v http://www.google.com
```

#### Set up the iptables rules for the transparent proxy.
```
iptables -A INPUT -p tcp --dport 3128 -d 172.17.0.2 -j ACCEPT
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

