<h1 align="center"> Autonity - Piccadilly Games R2 Guide </h1>

![image](https://user-images.githubusercontent.com/106930902/233866088-85068fd1-a996-499e-9737-51cab182046b.png)

Start Time: April 24th 2023 00:00:00 UTC<br>
Duration: 4 weeks<br>
Theme: Node infrastructure<br>
In order to get more info about tasks, visit: https://game.autonity.org/round-2/<br></h1>

#You can see Turkish Guide @ https://github.com/dacyxs/Autonity/blob/main/readmetr.md


# Requirements

## Hardware 

To run an Autonity Go Client node, we recommend using a host machine (physical or virtual) with the following minimum specification:

Requirement	At least	Recommended
OS	Ubuntu 20.04 LTS	Ubuntu 20.04 LTS
CPU	3.10 GHz with 8 CPU’s	3.10 GHz with 16 CPU’s
RAM	8GB	16GB
Storage	1024GB free storage for full nodes and Validators	1024 GB free storage for full nodes and validators

## Network 
A public-facing internet connection with static IP is required. Incoming traffic must be allowed on the following:

TCP, UDP 30303 for node p2p (DEVp2p) communication.
TCP 8545 to make http RPC connections to the node.
TCP 8546 to make WebSocket RPC connections to the node.
TCP 6060 to export Autonity metrics (recommended but not required)

Check if the port is already open by running the following command(Replace <port_number> with the number of the port you want to check.):
sudo netstat -tuln | grep <port_number>

If the port is not already open, you can open it by modifying the firewall settings using iptables. For example, to open port 80 for HTTP traffic, you can run the following command:
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

If you have a firewall management tool such as ufw (Uncomplicated Firewall) installed, you can use it to open the port instead. For example, to open port 80 for HTTP traffic using ufw, you can run the following commands:
sudo ufw allow 80/tcp
sudo ufw reload

Finally, you may need to restart any services that are using the port for the changes to take effect. For example, if you opened port 80 for HTTP traffic, you would need to restart your web server.
In order to open all related ports you can run below snippet;
```
sudo iptables -A INPUT -p tcp --dport 30303 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 30303 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8545 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8546 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 6060 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

sudo ufw allow 30303/tcp
sudo ufw allow 30303/udp
sudo ufw allow 8545/tcp
sudo ufw allow 8546/tcp
sudo ufw allow 6060/tcp
sudo ufw allow 22/tcp
sudo ufw reload
```

## Update the Server
```
sudo apt-get update && sudo apt-get upgrade
```

## Install the necessary packages to allow apt to use a repository over HTTPS:
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

## install git 
```
sudo apt-get install git
```

## install golang
```
wget https://golang.org/dl/go1.17.4.linux-amd64.tar.gz
```
```
sudo tar -C /usr/local -xzf go1.17.4.linux-amd64.tar.gz
```
```
export PATH=$PATH:/usr/local/go/bin
```
```
source ~/.profile
```
```
go version
```

## install c compiler
```
sudo apt-get install build-essential
```
```
gcc --version
```


## Installing the Docker image
```
sudo apt install docker.io -y
```
```
sudo systemctl enable --now docker
```
```
systemctl restart docker.service
```

## Once the installation is complete, start the Docker service and enable it to start at boot:
```
sudo systemctl start docker
```
```
sudo systemctl enable docker
```

## Verify that Docker is installed correctly by running the hello-world container:
```
sudo docker run hello-world
```

## To limit the size of the log files, add the following to the file /etc/docker/daemon.json

```
nano /etc/docker/daemon.json
```
```
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "500m",
    "max-file": "20"
  }
}
```
Press CTRL + X Then Y to save and quit.

## Restart the Docker service to ensure the change is reflected:

```
sudo systemctl restart docker
```

## Download latest stable version. 
```
wget https://github.com/autonity/autonity/releases/download/v0.10.1/autonity-linux-amd64-0.10.1.tar.gz
```

## Extract the file after download is completed.
```
tar -xzvf autonity-linux-amd64-0.10.1.tar.gz
```

## (Optional) Copy the binary to /usr/local/bin so it can be accessed by all users, or other location in your PATH :
```
cd build/bin
```
```
sudo cp -r autonity /usr/local/bin/autonity
```
```
cd
```


## Pull the Autonity Go Client image from the Github Container Registry:

```
docker pull ghcr.io/autonity/autonity:latest
```

## Verify the authenticity of the Autonity Docker images against the official image digests. Output should be as followed.
```
docker images --digests ghcr.io/autonity/autonity
```

![image](https://user-images.githubusercontent.com/106930902/233865723-cde2fe5d-88bf-4b58-9a3b-f591a81ae3d0.png)


## Verify the installation. You will get below output;
```
./autonity version
```

Autonity
Version: 0.10.1
Architecture: amd64
Protocol Versions: [66]
Go Version: go1.18.2
Operating System: linux
GOPATH=
GOROOT=/usr/local/go

## If using Docker, the setup of the image can be verified with:
```
docker run --rm ghcr.io/autonity/autonity:latest version
```



## Setup the Autonity Utility Tool (aut)
```
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
```
```
wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz
```
```
tar -xf Python-3.9.6.tgz
```
```
cd Python-3.9.6/
```
```
./configure --enable-optimizations
```
```
apt install python3-pip
```
```
pip install pipx
```
```
python3 -m pip install --user pipx
```
```
python3 -m pipx ensurepath
```
```
python3 -m pip install --user --upgrade pipx
```
```
apt install python3.8-venv
```

## We have to reboot in order to run pipx command. 

```
sudo reboot
```

## Then download aut 

```
sudo pipx install git+https://github.com/autonity/aut.git
```

## Edit file .autrc with below command;

```
nano /root/.autrc
```

```
[aut]
rpc_endpoint = https://rpc1.piccadilly.autonity.org/
nodekey=autonity-chaindata/autonity/nodekey
keyfile=.autonity/keystore/keystorenameofyours
```

## Run Autonity (binary or source code install)

```
mkdir autonity-chaindata
```

## First create a new screen
```
apt install screen
```
```
screen -S node
```

```
docker run \
    -t -i \
    --volume $(pwd)/autonity-chaindata:/autonity-chaindata \
    --publish 8545:8545 \
    --publish 8546:8546 \
    --publish 30303:30303 \
    --publish 30303:30303/udp \
    --publish 6060:6060 \
    --name autonity \
    --rm \
    ghcr.io/autonity/autonity:latest \
        --datadir ./autonity-chaindata  \
        --piccadilly \
        --http  \
        --http.addr 0.0.0.0 \
        --http.api aut,eth,net,txpool,web3,admin  \
        --http.vhosts \* \
        --ws  \
        --ws.addr 0.0.0.0 \
        --ws.api aut,eth,net,txpool,web3,admin  \
        --nat extip:<IP_ADDRESS>
```

## Where: <IP_ADDRESS> is the node’s host IP Address, which can be determined with curl ifconfig.me.
--piccadilly specifies that the node will use the Piccadilly tesnet. For other tesnets, use the appropriate flag (for example, --bakerloo).

exit with CTRL + A + D ( Dont use CTRL + C)

## in order to get back to node logs, you should enter below code: 
```
screen -r node
```

## If you are able to write below code your setup is completed.
```
aut node info
```
 
## Get the block number:
```
aut block height
```

## Check the auton balance of an account:
```
aut account balance <_addr>
```

## Check the newton balance of an account: 
```
aut account balance --ntn <_addr>
```

## Create account using aut. This will create key file and give you your address. 
```
aut account new
```

##Then we have to sign-message in the form with below code in order to get private key. Change the filename with your specific file name.
```
aut account sign-message "I have read and agree to comply with the Piccadilly Circus Games Competition Terms and Conditions published on IPFS with CID QmVghJVoWkFPtMBUcCiqs7Utydgkfe19wkLunhS5t57yEu" --keyfile /root/.autonity/keystore/filename
```

## Fund the account
https://faucet.autonity.org/
![image](https://user-images.githubusercontent.com/106930902/233856072-0cbeafb5-bd48-4b1a-b092-0a5d2c458346.png)

## Register to game from the below link;
https://game.autonity.org/incentive-game-forms-frontend/registration.html


## Continue to create Validator after you registered for the game;
https://github.com/dacyxs/Autonity/blob/main/On_chain_tasks.md

## Original document: 
https://docs.autonity.org/
https://game.autonity.org/

## Official links of the project: 
https://discord.gg/autonity
https://twitter.com/autonity_
https://autonity.org/

## Block Explorer
https://piccadilly.autonity.org/

## Dashboard
https://validators.game.autonity.org/dashboards/?query=

## Follow me @
https://twitter.com/Dacxys

