## Cisco switchte DHCP konfigüre etmek 

bilgisayardan konsol kablosu ile switche bağlantı yaptıktan sonra Putty yardımı ile switch termimnal arayüzüne bağlandım
burada switchi aşağıdaki gibi konfigüre ettim
```
enable
conf t
```

bir vlan oluşturdum
```
vlan 10
name emine
exit
```

bu bir switcjh oldğu için routing özelliğini açmam gerekti. sonuçta bu switchin ağın gatewayi olarak çalışması gerek.
``ip routing ``

vlan 10 ipsine bir gateway verdim
```
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shut
exit
```

DHCP konfigürasyonu için öncelikle servisin açık olduğundan emin oldum
``service dhcp ``

exclude yapmak için bir aralık seçtim.
``ip dhcp excluded-address 192.168.10.1 192.168.10.50``

bir dhcp havuzu oluşturdum
```
ip dhcp pool emine-pool
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit
```

bilgisayarı bağladığım port switchin f0/0 portu idi. Bu portun vlan 10 üyesi olması gerekiyor yoksa bilgisayar DHCP sunucusuna ulaşamaz ve IP alamaz.

```
interface FastEthernet 0/1
 switchport mode access
 switchport access vlan 10
 no shutdown
 exit
```

bu adımları yaptıktan sonra ethernet kablosunun bir ucunu switchimin f0/0 potuna bir ucunu da bilgisayarımın ethernet girişine bağladığımda bilgisayarımdan kontrol yapmak için şu adımları uyguladım 
 bilgisayarın search barına view network connection yazıp enterladım ve burada bağlantımın oluşakta olduğunu gördüm 
 
<img width="327" height="144" alt="image" src="https://github.com/user-attachments/assets/e69f50cd-e39f-4e15-8cc7-399e060d840c" />

bağlantı tamamlandığında bilgisayarımın cmd arayüzüne girip ``ipconfig`` komutu ile kontrol etiğimde beklenen 192.168.10.254 adresinin tanımlandığını gördüm.

<img width="595" height="128" alt="image" src="https://github.com/user-attachments/assets/ac166ce3-1f70-40d3-8475-2cbb271a9ca6" />
