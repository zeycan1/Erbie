![image](https://github.com/zeycan1/Erbie/assets/108004368/2cf4b36a-0586-42b5-934e-c4d43ae808ec)


# Erbie


## Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	4|
| RAM	| 8+ GB |
| Storage	| 500 GB SSD |


## Eğer sunucunuzda daha önceden kurulu wormholes varsa önce onu siliyoruz:

```
docker stop wormholes && docker rm wormholes && docker rmi wormholestech/wormholes:v1
rm -rf /wm
```


## Önce Sunucumuzu güncelliyoruz:
```
sudo apt-get update && sudo apt-get upgrade

```
## wget'i yüklüyoruz:
```
apt-get install wget
```

## Şimdi Docker Kurulumu Yapıyoruz:
```
sudo apt install docker.io -y
```

```
sudo systemctl enable --now docker
```

```
systemctl restart docker.service
```
## Erbie Kurulumunu yapıyoruz:
```
wget -O erbie_install.sh https://docker.erbie.io/erbie_install.sh && sudo bash erbie_install.sh
```
## Private keyinizi girin ve Enter tuşuna basın.

![image](https://github.com/zeycan1/Erbie/assets/108004368/a211bc2f-3708-4bb9-8878-454abe84a905)

## Kurulum tamamlanıca aşağıdaki gibi görünecektir.

![image](https://github.com/zeycan1/Erbie/assets/108004368/f4c8e081-36a7-4bac-bca2-e1db1e3419da)

## Aşağıdaki komutu girerek dockerin doğru bir şekilde çalıştığından emin olun.
```
sudo docker ps -a
```
## Çıktısı aşağıdaki gibi olmalıdır.

![image](https://github.com/zeycan1/Erbie/assets/108004368/748735e1-88a5-4a10-bde0-6c5486d75fb6)

## Logları kontrol etmek için monitör kuruyoruz:
```
nano monitor.sh

```
## Açılmış olan pencereye tek seferde aşağıdaki komutları yapıştırıyoruz. Sonra ctrl+x diyor ve y diyerek ekrandan çıkıyoruz.
```
#!/bin/bash
function info(){
   cn=0
   vl=$(wget https://docker.erbie.io/version>/dev/null 2>&1 && cat version|awk '{print $2}')
   while true
   do
            echo "the monitor version is $vl"
            echo "$cn second."
            echo "node $1"
            rs1=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_peerCount","id":64}' 127.0.0.1:$1 2>/dev/null`
            count=$(parse_json $rs1 "result")
            echo "Number of node connections: $((16#${count:2}))"
            rs2=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":64}' 127.0.0.1:$1 2>/dev/null`
            blckNumber=$(parse_json $rs2 "result")
            echo "Block height of the current peer: $((16#${blckNumber:2}))"
            sleep 5
            clear
            let cn+=5
   done
}

function parse_json(){
   echo "${1//\"/}"|sed "s/.*$2:\([^,}]*\).*/\1/"
}

function main(){
   if [[ $# -eq 0 ]];then
            info 8545
   else
            info $1
   fi
}

main "$@"

```
## Monitorümüz kurulduğuna göre artık loglara aşağıdaki komutla bakabiliriz:
```
bash ./monitor.sh
```

## İşinize Yarayabilecek Diğer Komutlar:

## Bağlantı durumu kontrol.
```
curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"net_peerCount","id":1}' http://127.0.0.1:8545
```
## Blok kontrolu
```
curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":1}' http://127.0.0.1:8545
```
## Bakiye kontrol

- Cüzdan adresi yazan yere cüzdan adresini yazınız.
```
curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["cüzdan adresi","pending"],"id":1}' http://127.0.0.1:8545
```
## Versiyon kontrol
```
curl -X POST -H "Content-Type:application/json" --data '{"jsonrpc":"2.0","method":"eth_version","id":64}' http://127.0.0.1:8545
```

## Şu anda test tokenlerimiz henüz gönderilmediği için stake edemiyoruz. Tokenlerimiz gelince onları da stake ederek tüm işlemlerimizi tamamlayacağız ve ödüllerimizin gelmesini bekleyeceğiz.


### ŞİMDİLİK HEPSİ BU KADAR... 
## Eğer rehberim faydalı olduysa yıldızlayıp forklarsanız memnun olurum :))



























