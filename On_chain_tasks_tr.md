# Onboard validator<br>
# Register as a Validator<br>
## Prerequisites <br>

Bir validator kaydetmek için ihtiyacınız olan:<br>

1)Ana makinenizde çalışan Autonity Go Client'ın çalışan bir örneği. Bu, validatör olarak kaydedilecek node olacaktır.<br>
2)Yapılandırılmış bir aut örneği.<br>
3)Auton ile finanse edilmiş bir hesap (işlem gazı maliyetlerini ödemek için). Bu hesabın, doğrulayıcının hazine hesabı haline geleceğini unutmayın - doğrulayıcıyı yönetmek için kullanılan hesap, bu hesap aynı zamanda doğrulayıcının staking ödüllerinden payını alacaktır.<br>
4)Bu kılavuz ayrıca JSON işlemci jq komut satırının kullanılabilir olduğunu varsayar.<br>

## Puanlama Kuralı
Bu görevdeki kayıtlı her katılımcı, Validatör düğümünü zincire kaydetmeyi başardığı için  50 puan alır. <br>
Görevin geri kalanında, algoritmanda düğümünüzün çalışma süresiyle orantılı puanlar alacaksınız. <br>
Düğümünüz her dönem için bir çalışma süresi ölçüsü alır ve bu ölçümün ≥%90 olduğu her dönem için 1 puan alırsınız. <br>
Dönemlerin her biri ≈30 dakikadır, bu nedenle 2. Tur 3 hafta sürerse mükemmel performans gösteren bir düğüm ≈1008 puan kazanabilir. <br>

Puanlar günlük olarak 00:00:00 UTC'den sonra tahsis edilecektir. Görev başına ve genel puan tabloları mevcuttur https://validators.game.autonity.org/dashboards/ <br>

Bir doğrulayıcının Tur başlangıç saatinden önce katılması durumunda, kayıt için kazanılan 50 puanın puan tablosunda gösterileceğini unutmayın. Görevin geri kalanı için puanlar yalnızca Turun başlangıç ve bitiş tarihi arasında toplanacaktır. <br>

#Devam etmek için mevcut bloğunuzun(currentblock) en yüksek Bloğa(highestBlock) eşit olması gerekir

![image](https://user-images.githubusercontent.com/106930902/235779105-e9e30aa4-e855-44e7-8c79-aecc4c72b775.png)


## Adım 1. Düğüm sahipliğinin kriptografik bir kanıtını oluşturun
Bu, Autonity Go Client'ı çalıştıran ana makinede autonity genEnodeProof komutu kullanılarak gerçekleştirilmelidir:<br>
<TREASURY_ACCOUNT_ADDRESS>, <br> hesap adresinizle değiştirilmelidir
Aşağıdaki komutla signature hex'inizi alın

```
SIGN_ADDR=$(autonity genEnodeProof --nodekey autonity-chaindata/autonity/nodekey <TREASURY_ACCOUNT_ADDRESS> | awk '{print $3}')
```
```
echo "Signature Address: " $SIGN_ADDR
```

## Adım 2. Validator enodunu ve adresini belirleyin
```
ENODEURL=$(aut node info -r http://127.0.0.1:8545 | grep enode | awk '{print $2}' | tr -d ,'"')
```
```
echo $ENODEURL
```
The url is returned in the admin_enode field with echo line.
```
VALIDATOR=$(aut validator compute-address $ENODEURL)
```

Size geri dönen bu tanımlayıcıyı not edin. Bu, doğrulayıcınız için benzersiz koddur.
![image](https://user-images.githubusercontent.com/106930902/233868590-7a9c2c15-a421-4837-993c-7d87bde03b2e.png)

## Adım 3. Kayıt işlemini gönderin.

<ENODE_URL>: Adım 2'de döndürülen kodlama url'si.<br>
<PROOF>: 1. Adımda oluşturulan enode sahipliğinin kanıtı.<br>

```
REGS_VALI=$(aut validator register $ENODEURL $SIGN_ADDR | aut tx sign - | aut tx send -)
```
```
echo "Validator Registration TX : " $REGS_VALI
```
  
İşlem tamamlandığında (bir bloğa dahil edilmesini beklemek ve durumu döndürmek için aut tx wait <txid> kullanın), düğüm etkin durumda doğrulayıcı olarak kaydedilir. Hisse kendisine bağlandıktan sonra konsensüs komitesine seçilmek için uygun hale gelecektir.

## Adım 4. Kaydı onaylayın
```  
aut validator list
```
  
Aşağıdakileri kullanarak doğrulayıcı ayrıntılarını kontrol edebilirsiniz:

```
aut validator info --validator $VALIDATOR
```
## Adım 5. validator onboarded mesajını imzalamak için nodekey'den yeni bir anahtar dosyası oluşturun. İkinci adım newkeyfilename öğesini aşağıdaki ilk adımda oluşturulan dosya adıyla değiştirin. Formda bu işlemler ile oluşturulan imzayı kullanın.
  
```
aut account import-private-key autonity-chaindata/autonity/nodekey
```
  ```
  aut account sign-message "validator onboarded" --keyfile .autonity/keystore/<newkeyfilename>
  ```
  
Validator kayıt formu
https://game.autonity.org/incentive-game-forms-frontend/validator.html
  
# Follow me @
https://twitter.com/Dacxys

  
  
