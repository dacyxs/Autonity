<h1 align="center"> Autonity - Piccadilly Games R2 Rehber </h1>

![image](https://user-images.githubusercontent.com/106930902/233866088-85068fd1-a996-499e-9737-51cab182046b.png)

Başlangıç saati: April 24th 2023 00:00:00 UTC<br>
Süre: 4 hafta<br>
Tema: Node infrastructure<br>
Görevler hakkında daha fazla bilgi almak için şu adresi ziyaret edin: https://game.autonity.org/round-2/<br></h1>

#You can see English Guide @ https://github.com/dacyxs/Autonity/blob/main/readme.md


# Gereksinimler

## Donanım

Bir Autonity Go Client düğümünü çalıştırmak için, aşağıdaki minimum özelliklere sahip bir ana makine (fiziksel veya sanal) kullanılması öneriliyor:

Requirement	At least	Recommended
OS	Ubuntu 20.04 LTS	Ubuntu 20.04 LTS
CPU	3.10 GHz with 8 CPU’s	3.10 GHz with 16 CPU’s
RAM	8GB	16GB
Storage	1024GB free storage for full nodes and Validators	1024 GB free storage for full nodes and validators

## Network 
Statik IP ile halka açık bir internet bağlantısı gereklidir. Aşağıdaki alanlarda gelen trafiğe izin verilmelidir:

TCP, UDP 30303 for node p2p (DEVp2p) communication.
TCP 8545 to make http RPC connections to the node.
TCP 8546 to make WebSocket RPC connections to the node.
TCP 6060 to export Autonity metrics (recommended but not required)

Aşağıdaki komutu çalıştırarak bağlantı noktasının zaten açık olup olmadığını kontrol edin(<port_number> öğesini kontrol etmek istediğiniz bağlantı noktasının numarasıyla değiştirin.):<br>
sudo netstat -tuln | grep <port_number><br>

Port zaten açık değilse, iptables kullanarak güvenlik duvarı ayarlarını değiştirerek açabilirsiniz. Örneğin, HTTP trafiği için 80 numaralı bağlantı noktasını açmak üzere aşağıdaki komutu çalıştırabilirsiniz:<br>
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT<br>

Yüklü ufw (Karmaşık Olmayan Güvenlik Duvarı) gibi bir güvenlik duvarı yönetim aracınız varsa, bunun yerine bağlantı noktasını açmak için kullanabilirsiniz. Örneğin, ufw kullanarak HTTP trafiği için 80 numaralı bağlantı noktasını açmak için aşağıdaki komutları çalıştırabilirsiniz:<br>
sudo ufw allow 80/tcp<br>
sudo ufw reload<br>

Son olarak, değişikliklerin etkili olması için bağlantı noktasını kullanan tüm hizmetleri yeniden başlatmanız gerekebilir. Örneğin, HTTP trafiği için 80 numaralı bağlantı noktasını açtıysanız, web sunucunuzu yeniden başlatmanız gerekir.<br>
İlgili tüm portları açmak için aşağıdaki snippet'i çalıştırabilirsiniz;<br>
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

## Sunucuyu Güncelle
```
sudo apt-get update && sudo apt-get upgrade
```

## Apt'nin HTTPS üzerinden bir havuz kullanmasına izin vermek için gerekli paketleri kurun:
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

## Kurulum tamamlandıktan sonra Docker hizmetini başlatın ve açılışta başlamasını sağlayın:
```
sudo systemctl start docker
```
```
sudo systemctl enable docker
```

## Merhaba dünya kapsayıcısını çalıştırarak Docker'ın doğru yüklendiğini doğrulayın:
```
sudo docker run hello-world
```

## Günlük dosyalarının boyutunu sınırlamak için /etc/docker/daemon.json dosyasına aşağıdakini ekleyin

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
Kaydetmek ve çıkmak için CTRL + X Sonra Y tuşlarına basın.

## Değişikliğin yansıtıldığından emin olmak için Docker hizmetini yeniden başlatın:

```
sudo systemctl restart docker
```

## En son sürümü indirin.
```
wget https://github.com/autonity/autonity/releases/download/v0.10.1/autonity-linux-amd64-0.10.1.tar.gz
```

## İndirme işlemi tamamlandıktan sonra dosyayı çıkartın.
```
tar -xzvf autonity-linux-amd64-0.10.1.tar.gz
```

## (İsteğe bağlı) İkili dosyayı /usr/local/bin konumuna kopyalayın, böylece tüm kullanıcılar veya PATH'nizdeki başka bir konum tarafından erişilebilir:
```
cd build/bin
```
```
sudo cp -r autonity /usr/local/bin/autonity
```
```
cd
```


## Autonity Go Client görüntüsünü Github Container Registry'den çekin:

```
docker pull ghcr.io/autonity/autonity:latest
```

## Autonity Docker görüntülerinin gerçekliğini resmi görüntü özetlerine göre doğrulayın. Çıktı aşağıdaki gibi olmalıdır.
```
docker images --digests ghcr.io/autonity/autonity
```

![image](https://user-images.githubusercontent.com/106930902/233865723-cde2fe5d-88bf-4b58-9a3b-f591a81ae3d0.png)


## Kurulumu doğrulayın. Aşağıdaki çıktıyı alacaksınız;
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

## Docker kullanılıyorsa görüntünün kurulumu şu şekilde doğrulanabilir:
```
docker run --rm ghcr.io/autonity/autonity:latest version
```



## Autonity Utility Tool'u Kurun (aut)
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

## Pipx komutunu çalıştırmak için yeniden başlatmamız gerekiyor.

```
sudo reboot
```

## Ardından aut'u indirin

```
sudo pipx install git+https://github.com/autonity/aut.git
```

## .autrc dosyasını aşağıdaki komutla düzenleyin;

```
nano /root/.autrc
```

```
[aut]
rpc_endpoint = https://rpc1.piccadilly.autonity.org/
```

## Autonity'yi çalıştırın (ikili veya kaynak kodu yüklemesi)

```
mkdir autonity-chaindata
```

## İlk önce yeni bir ekran oluşturun
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

## Burada: <IP_ADDRESS>, düğümün curl ifconfig.me ile belirlenebilen ana bilgisayar IP Adresidir.
--piccadilly, düğümün Piccadilly tesnet'i kullanacağını belirtir. Diğer tesnet'ler için uygun bayrağı kullanın (örneğin, --bakerloo).

CTRL + AND ile çıkın ( CTRL + C kullanmayın)

## düğüm günlüklerine geri dönmek için aşağıdaki kodu girebilirsiniz:
```
screen -r node
```

## Aşağıdaki kodu yazabiliyorsanız kurulumunuz tamamlanmıştır.
```
aut node info
```
 
## Blok numarasını almak isterseniz:
```
aut block height
```

## Bir hesabın otomatik bakiyesini kontrol etmek isterseniz:
```
aut account balance <_addr>
```

## Bir hesabın newton bakiyesini kontrol etmek isterseniz(newton validatorluk sonucu gelmeye başlayacak):
```
aut account balance --ntn <_addr>
```

## aut kullanarak hesap oluşturun. Bu, anahtar dosyası oluşturacak ve size adresinizi verecektir. 
```
aut account new
```

##Daha sonra private key almak için aşağıdaki kod ile formdaki mesajı imzalamamız gerekiyor. Dosya adını kendi dosya adınızla değiştirin.
```
aut account sign-message "I have read and agree to comply with the Piccadilly Circus Games Competition Terms and Conditions published on IPFS with CID QmVghJVoWkFPtMBUcCiqs7Utydgkfe19wkLunhS5t57yEu" --keyfile /root/.autonity/keystore/<dosyaadı>
```

## Fund the account
https://faucet.autonity.org/
![image](https://user-images.githubusercontent.com/106930902/233856072-0cbeafb5-bd48-4b1a-b092-0a5d2c458346.png)

## Aşağıdaki linkten oyuna kayıt olun;
https://game.autonity.org/incentive-game-forms-frontend/registration.html


## Oyuna kaydolduktan sonra Validator oluşturmaya devam edin;
https://github.com/dacyxs/Autonity/blob/main/On_chain_tasks.md

## Orijinal document: 
https://docs.autonity.org/
https://game.autonity.org/

## Projenin resmi linkleri:
https://discord.gg/autonity
https://twitter.com/autonity_
https://autonity.org/

## Block Explorer
https://piccadilly.autonity.org/

## Follow me @
https://twitter.com/Dacxys

