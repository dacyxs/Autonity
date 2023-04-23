##Requirements

#Hardware 

To run an Autonity Go Client node, we recommend using a host machine (physical or virtual) with the following minimum specification:

Requirement	At least	Recommended
OS	Ubuntu 20.04 LTS	Ubuntu 20.04 LTS
CPU	3.10 GHz with 8 CPU’s	3.10 GHz with 16 CPU’s
RAM	8GB	16GB
Storage	1024GB free storage for full nodes and Validators	1024 GB free storage for full nodes and validators

##Network 
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
sudo iptables -A INPUT -p tcp --dport 30303 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 30303 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8545 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8546 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 6060 -j ACCEPT

sudo ufw allow 30303/tcp
sudo ufw allow 30303/udp
sudo ufw allow 8545/tcp
sudo ufw allow 8546/tcp
sudo ufw allow 6060/tcp
sudo ufw reload

Download latest stable version. 
wget https://github.com/autonity/autonity/releases/download/v0.10.1/autonity-linux-amd64-0.10.1.tar.gz

Extract the file after download is completed.
tar -xzvf autonity-linux-amd64-0.10.1.tar.gz

(Optional) Copy the binary to /usr/local/bin so it can be accessed by all users, or other location in your PATH :
sudo cp -r autonity /usr/local/bin/autonity

Update the Server
sudo apt-get update && sudo apt-get upgrade

Install the necessary packages to allow apt to use a repository over HTTPS:
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

Installing the Docker image
sudo apt install docker.io -y
sudo systemctl enable --now docker
systemctl restart docker.service

Once the installation is complete, start the Docker service and enable it to start at boot:
sudo systemctl start docker
sudo systemctl enable docker

Verify that Docker is installed correctly by running the hello-world container:
sudo docker run hello-world

To limit the size of the log files, add the following to the file /etc/docker/daemon.json

nano /etc/docker/daemon.json

{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "500m",
    "max-file": "20"
  }
}

Press CTRL + X Then Y to save and quit.

Restart the Docker service to ensure the change is reflected:

sudo systemctl restart docker

Pull the Autonity Go Client image from the Github Container Registry:

docker pull ghcr.io/autonity/autonity:latest

Verify the authenticity of the Autonity Docker images against the official image digests 
docker images --digests ghcr.io/autonity/autonity
REPOSITORY                               TAG       DIGEST                                                                    IMAGE ID       CREATED        SIZE
ghcr.io/autonity/autonity                latest    sha256:0eb561ce19ed3617038b022db89586f40abb9580cb0c4cd5f28a7ce74728a3d4   3375da450343   3 weeks ago    51.7MB

install git 
sudo apt-get install git

install golang
wget https://golang.org/dl/go1.17.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.17.4.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
source ~/.profile
go version

install c compiler
sudo apt-get install build-essential
gcc --version

Clone/Copy the Autonity repo:
git clone git@github.com:autonity/autonity.git

Enter the autonity directory and build autonity:
cd autonity
make autonity

(Optional) Copy the generated binary to /usr/local/bin so it can be accessed by all users, or other location in your PATH :
sudo cp build/bin/autonity /usr/local/bin/autonity

Verify the installation
$ ./autonity version
Autonity
Version: 0.10.1
Architecture: amd64
Protocol Versions: [66]
Go Version: go1.18.2
Operating System: linux
GOPATH=
GOROOT=/usr/local/go

If using Docker, the setup of the image can be verified with:
$ docker run --rm ghcr.io/autonity/autonity:latest version

Run Autonity (binary or source code install)
mkdir autonity-chaindata
autonity \
    --datadir ./autonity-chaindata  \
    --piccadilly  \
    --http  \
    --http.addr 0.0.0.0 \
    --http.api aut,eth,net,txpool,web3,admin  \
    --http.vhosts \* \
    --ws  \
    --ws.addr 0.0.0.0 \
    --ws.api aut,eth,net,txpool,web3,admin  \
    --nat extip:<IP_ADDRESS>
    
    Where:

<IP_ADDRESS> is the node’s host IP Address, which can be determined with curl ifconfig.me.
--piccadilly specifies that the node will use the Piccadilly tesnet. For other tesnets, use the appropriate flag (for example, --bakerloo).

Create a working directory for installing Autonity. For example:
mkdir autonity-go-client
cd autonity-go-client

