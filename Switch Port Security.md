## Swich için port security ayarları yapmak

**Bu labda switchin bir portuna `` port-security`` uygulama adımlarımı anlatacağım.**

### 1. öncelikle violation restrict yaptım

   <img width="501" height="162" alt="image" src="https://github.com/user-attachments/assets/81706710-c0c0-46ad-b9c1-f63af12386c8" />

   kurduğum topolojide switchin arayzüne girdim ve
   ````
   enable
   conf t
   interface f0/1
   switchport mode access
   switchport port-security violation restrict
   switchport port-security mac-address  0006.2AD7.C09C
   switchport port-security
   ````
   bu kodlar port securityi açar ve tek mac adresini port-security olarak atar. eğer bu mac dışında başka mac adresli bir bilgisayar bağlanıp ping atmaya kalkışırsa paket yollamaya izin vermez. ama portu shutdown etmez.

   konfigürasyon ayarlanmış mı diye kontrol etmek için:
  ````
 Switch(config-if)#do show port-security interface f0/1
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
````
bir ping denemesi yapalım:

<img width="496" height="193" alt="image" src="https://github.com/user-attachments/assets/09361329-1b06-4f29-9a75-79a9987cc235" />

şimdi f0/1 portuna başka bir pc bağlayalım ve ping deneyelim:

<img width="448" height="207" alt="image" src="https://github.com/user-attachments/assets/1cd857f3-e050-43f2-95ad-9efd05168084" />

<img width="1680" height="808" alt="image" src="https://github.com/user-attachments/assets/d7a7e493-f6fa-4952-9fd4-cfab664bba1c" />

gördüğümüz üzere ping gitmedi ve uyarı geldi ama port kapandı mı ona bakalım:

<img width="670" height="196" alt="image" src="https://github.com/user-attachments/assets/c39338c8-393a-489e-9843-3e7274935f97" />

port kapanmamış.





### 2. yeni bir topolojide violation shutdown yapalım

topoloji

<img width="711" height="399" alt="image" src="https://github.com/user-attachments/assets/03e8ada8-3fd0-45ca-b0b2-612b768343c1" />

switchin f0/1 portuna PC0 Laptop1 ve Server0 ım mac adreslerini statik olarak tanımlayacağım. Laptop 2 nin mac adresini tanımlamayacağım daha sonra laptop 2 yi f0/1 portuna bağlayıp ping denemesi yapacağım.

switchte:

````
enable
conf t
interface f0/1
switchport mode access
switchport port-security mac-address 00D0.BCC2.9EA1 0001.63AA.017A 0010.11A3.CA4D
switchport port security maximum 3
//violation kuralı oluşturmama gerek yok çünkü default olarka zaten violation shutdown modundadır
switchport port-security  //port security'i enable etme komutu
````
konfigürasyonu kontrol edelim
````
Switch#show port-security interface f0/1
Port Security              : Enabled
Port Status                : Secure-down
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 3
Total MAC Addresses        : 3
Configured MAC Addresses   : 3
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0060.7087.45BD:1
Security Violation Count   : 0
````

laptop2 yi switche bağlayıp portun up olmasını bekleyelim:

<img width="355" height="342" alt="image" src="https://github.com/user-attachments/assets/519de951-0d45-4cf3-8967-8aa7ba948e9b" />

şimdi port up oluyor ama laptoptan ping atmak istersem ne oluyor bakalım:

ping timeout verdi:

<img width="506" height="177" alt="image" src="https://github.com/user-attachments/assets/621bcf14-e08c-496e-8fc4-55c5a63b3a87" />

switch cli arayüzünde hata aldığımızı gördük:

<img width="837" height="131" alt="image" src="https://github.com/user-attachments/assets/1b0952d1-6ec2-442e-b2e7-e661de1d55d5" />

port durumuna bakkalım:

<img width="659" height="110" alt="image" src="https://github.com/user-attachments/assets/2c5e8651-dbac-4f7c-9a6c-e0251ba9c11a" />


kayıtlı makinelerden biri olan PC0 ile bir ping denemesi yapalım:

<img width="733" height="145" alt="image" src="https://github.com/user-attachments/assets/8c7ef54b-b243-4984-a73b-50b402898013" />

pingin başarılı olduğunu gördük:

<img width="538" height="228" alt="image" src="https://github.com/user-attachments/assets/81a85047-df84-4d01-b933-13074109f084" />












   

