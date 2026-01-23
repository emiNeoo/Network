# router temel yapılandırma komutları

aşağıdaki topolojiyi oluşturdum

<img width="1211" height="555" alt="image" src="https://github.com/user-attachments/assets/cbb50be9-31fc-4a34-bf2e-22f17641ccab" />

## router yapılandırması
enable 
clock set 16:10:00 August 2026
conf t
hostname emine
enable secret cisco emine
username emine password emine // lokal kullanıcı adı ve şifre oluşturma
telnet ayarları için
````
line vty 10 15 
login local
exit
````
router ip yapılandırması
````
interface g0/0 
ip address 192.168.1.1 255.255.255.0
no shut
exit
end
````

önemli kontrol komutları
````
show ip interface brief 
show ip route 
show running-config
show startup- config
show flash
````
cisco packet tracer'da pclere ip vermek için desktop sekmesinde ip konfigürasyonu penceresine girilir ve örnekteki gibi doldurulur

<img width="868" height="323" alt="image" src="https://github.com/user-attachments/assets/ee212919-5911-4557-9c69-d8830318c665" />



pc1 den routera telnet denemesi


``telnet 192.168.1.1``


<img width="329" height="168" alt="image" src="https://github.com/user-attachments/assets/a3d175db-b94b-43f6-b6e0-53d48b08dee7" />


PC1 den laprop 1 e tracert komutu denemesi

``tracert 192.168.2.11``

<img width="577" height="137" alt="image" src="https://github.com/user-attachments/assets/28f84c47-5482-4979-86fb-3dcb47600dad" />

