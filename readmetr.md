<h1 align="center"> Autonity - Piccadilly Games R2 Guide </h1>

![image](https://user-images.githubusercontent.com/106930902/233866088-85068fd1-a996-499e-9737-51cab182046b.png)

Başlangıç Zamanı: 24 Nisan 2023 00:00:00 UTC<br>
Süre: 4 hafta<br>
Tema: Node infrastructure<br>
Görevler hakkında daha fazla bilgi edinmek için ziyaret edin: https://game.autonity.org/round-2/<br></h1>

## Gereksinimler

# Donanım

Autonity Go İstemci düğümü çalıştırmak için, aşağıdaki minimum özelliklere sahip bir ana makine (fiziksel veya sanal) kullanmanızı öneririz:

Minimum Gereksinimler	
OS	Ubuntu 20.04 LTS	<br>
CPU	3.10 GHz with 8 CPU’s<br>
RAM	8GB	<br>
Storage	1024GB free storage for full nodes and Validators<br>

# Network 
Sabit IP'li halka açık bir internet bağlantısı gereklidir. Aşağıdakilerde gelen trafiğe izin verilmelidir:

TCP, UDP 30303 düğüm p2p (DEVp2p) iletişimi için.<br>
TCP 8545 düğüme http RPC bağlantıları yapmak için.<br>
TCP 8546 düğüme WebSocket RPC bağlantıları yapmak için.<br>
TCP 6060 Autonity metriklerini dışa aktarmak için (önerilir ama zorunlu değil)<br>

Aşağıdaki komutu çalıştırarak portun zaten açık olup olmadığını kontrol edin ( <port_number> yerine kontrol etmek istediğiniz portun numarasını yazın.):<br>
sudo netstat -tuln | grep <port_number><br>

Eğer port zaten açık değilse, iptables kullanarak güvenlik duvarı ayarlarını değiştirerek açabilirsiniz. Örneğin, HTTP trafiği için 80 portunu açmak için aşağıdaki komutu çalıştırabilirsiniz:<br>
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT<br>

Eğer ufw (Uncomplicated Firewall) gibi bir güvenlik duvarı yönetim aracınız varsa, portu açmak için bunu kullanabilirsiniz. Örneğin, ufw kullanarak HTTP trafiği için 80 portunu açmak için aşağıdaki komutları çalıştırabilirsiniz:<br>
sudo ufw allow 80/tcp<br>
sudo ufw reload<br>

Son olarak, değişikliklerin etkili olması için portu kullanan hizmetleri yeniden başlatmanız gerekebilir. Örneğin, HTTP trafiği için 80 portunu açtıysanız, web sunucunuzu yeniden başlatmanız gerekir.<br>
İlgili tüm portları açmak için aşağıdaki kod parçacığını çalıştırabilirsiniz;<br>
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

# Sunucuyu Güncelleme
```
sudo apt-get update && sudo apt-get upgrade
```

# Apt'nin HTTPS üzerinden bir depoyu kullanmasına izin vermek için gerekli paketleri yükleyin:
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

# git yükleyin
```
sudo apt-get install git
```

# golang yükleyin
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

# c compiler yükleyin
```
sudo apt-get install build-essential
```
```
gcc --version
```


# Docker image yükleyin
```
sudo apt install docker.io -y
sudo systemctl enable --now docker
systemctl restart docker.service
```

# Kurulum tamamlandığında, Docker hizmetini başlatın ve başlangıçta başlaması için etkinleştirin:
```
sudo systemctl start docker
sudo systemctl enable docker
```

# Hello-world konteynerini çalıştırarak Docker'ın doğru şekilde kurulduğunu doğrulayın:
```
sudo docker run hello-world
```

# Log dosyalarının boyutunu sınırlamak için, /etc/docker/daemon.json dosyasını açın. 

```
nano /etc/docker/daemon.json
```
# Aşağıdakileri ekleyin, daha sonra CTRL + X ve Y basarak kaydedin ve çıkış yapın. 
```
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "500m",
    "max-file": "20"
  }
}
```


# Değişikliğin yansıtılması için Docker hizmetini yeniden başlatın:

```
sudo systemctl restart docker
```

# En son autonity sürümünü indirin.
```
wget https://github.com/autonity/autonity/releases/download/v0.10.1/autonity-linux-amd64-0.10.1.tar.gz
```

# İndirme tamamlandıktan sonra dosyayı çıkarın.
```
tar -xzvf autonity-linux-amd64-0.10.1.tar.gz
```

# (İsteğe bağlı) İkili dosyayı /usr/local/bin klasörüne veya PATH'deki başka bir konuma kopyalayın, böylece tüm kullanıcılar tarafından erişilebilir hale gelir:
```
sudo cp -r autonity /usr/local/bin/autonity
```


# Github Konteyner Kaydından Autonity Go İstemci imajını çekin:

```
docker pull ghcr.io/autonity/autonity:latest
```

# Autonity Docker imajlarının doğruluğunu resmi imaj özetleriyle doğrulayın. Çıktı şu şekilde olmalıdır.
```
docker images --digests ghcr.io/autonity/autonity
```

![image](https://user-images.githubusercontent.com/106930902/233865723-cde2fe5d-88bf-4b58-9a3b-f591a81ae3d0.png)


# Kurulumu doğrulayın. Aşağıdaki çıktıyı alacaksınız;
```
./autonity version
```
# Kurulumu doğrulayın.Aşağıdaki çıktıyı alacaksınız;
Autonity<br>
Version: 0.10.1<br>
Architecture: amd64<br>
Protocol Versions: [66]<br>
Go Version: go1.18.2<br>
Operating System: linux<br>
GOPATH=<br>
GOROOT=/usr/local/go<br>

# Docker kullanılıyorsa, imajın kurulumu şununla doğrulanabilir:
```
docker run --rm ghcr.io/autonity/autonity:latest version
```



# Autonity Yardımcı Aracını (aut) Ayarlayın
```
apt install pipx
apt install python3-pip
python3 -m pip install --user pipx
python3 -m pipx ensurepath
python3 -m pip install --user --upgrade pipx
apt install python3.8-venv
```

# Pipx komutunu çalıştırmak için sunucuyu aşağıdaki kod ile yeniden başlatmamız gerekiyor. (Aynı sunucuya kurulu başka bir işlem gerçekleştiriyorsanız. İşlemlerinizin etkilenmediğine emin olunuz!)

```
sudo reboot
```

# Daha sonra aut'u indirin

```
pipx install git+https://github.com/autonity/aut.git
```

# .autrc dosyasını aşağıdaki komutla düzenleyin;

```
nano /root/.autrc
```

```
[aut]
rpc_endpoint = https://rpc1.piccadilly.autonity.org/
```

# Autonity'u çalıştırın (ikili veya kaynak kod yüklemesi)

```
mkdir autonity-chaindata
```

# İlk olarak yeni bir ekran oluşturun.
```
apt install screen
screen -S node
```
# Daha sonra aşağıdaki komutu girerek node'u çalıştırın. <IP_ADDRESS> bölümünü kendi makine IP'niz ile değiştirmeniz gerekmektedir. --piccadilly yazan bölüm testnet adı değişirse değiştirilmesi gerekir. R2 süresince aynı kalabilir. Screen içerisinden CTRL + C kullanarak çıkış yapmayın, CTRL + A + D ile çıkış yapın. Yoksa node'unuz duracaktır. 

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


# Düğüm günlüklerine geri dönmek için aşağıdaki kodu girmeniz gerekiyor:
```
screen -r node
```

# Aşağıdaki kodu yazabiliyorsanız, kurulumunuz tamamlanmıştır.
```
aut node info
```

#   Blok numarası sorgulamak için aşağıdaki kodu yazabilirsiniz.
```
aut block height
```

# Hesabınızda bulunan bakiyeyi kontrol etmek için aşağıdaki kod kullanılabilir. <_addr>'yi değiştirmeyi unutmayın.
```
aut account balance <_addr>
```

# Hesabınızda bulunan newton bakiyeyi kontrol etmek için aşağıdaki kod kullanılabilir. <_addr>'yi değiştirmeyi unutmayın.
```
aut account balance --ntn <_addr>
```

# Aut kullanarak hesap oluşturun. Bu, anahtar dosyası oluşturacak ve size adresinizi verecektir.
```
aut account new
```

# Daha sonra özel anahtarı almak için aşağıdaki kodla mesajı imzalamanız gerekiyor. Dosya adını kendi dosya adınızla değiştirin.
```
aut account sign-message "I have read and agree to comply with the Piccadilly Circus Games Competition Terms and Conditions published on IPFS with CID QmVghJVoWkFPtMBUcCiqs7Utydgkfe19wkLunhS5t57yEu" --keyfile /root/.autonity/keystore/dosyaadı
```

# Faucet Aracılığı ile hesabınıza test token almak için aşağıdaki linki kullanın.
https://faucet.autonity.org/
![image](https://user-images.githubusercontent.com/106930902/233856072-0cbeafb5-bd48-4b1a-b092-0a5d2c458346.png)

# Aşağıdaki form aracılığı ile kayıt yaptırmanız gerekecektir. 
https://game.autonity.org/incentive-game-forms-frontend/registration.html


# Original document: 
https://docs.autonity.org/
https://game.autonity.org/

# Official links of the project: 
https://discord.gg/autonity
https://twitter.com/autonity_
https://autonity.org/

# Follow me @
https://twitter.com/Dacxys


