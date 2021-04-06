# Docker Swarm Manager Installation

## Set up the server hostname
```sh
export USE_HOSTNAME=swarmworker1.idn.media
echo $USE_HOSTNAME > /etc/hostname
hostname -F /etc/hostname
```

## Install the latest updates
```sh
apt-get update
apt-get dist-upgrade
```

## Install the latest stable version of Docker CE
```sh
apt-get remove docker docker-engine docker.io containerd runc
apt-get update
apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
``` sh
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
```sh
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io
```

## Install Docker Compose (Optional)

```sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
``` sh
sudo chmod +x /usr/local/bin/docker-compose
```
``` sh
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

# Init Docker Swarm

``` sh
docker swarm init
```

## Join Docker Host as Swarm Manager

on primary swarm manager  node
```sh
docker swarm join-token manager
```

``` sh
docker swarm join --token SWMTKN-1-******** 10.100.xxx.xxx:<PORT>
```

## Setup Swarmpit (optional)

on primary swarm manager node
``` sh
git clone https://github.com/swarmpit/swarmpit -b master
docker stack deploy -c swarmpit/docker-compose.yml swarmpit
```

## Setup Portainer (optional)

on primary swarm manager node
``` sh
curl -L https://downloads.portainer.io/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
```
