## Etherchannel nasıl yapılandırılır ve kontrol edilir?

### L2 Port Channel 
 Öncelikle cisco packet tracer'da bir [topoloji](https://github.com/emiNeoo/Network/blob/lab_dosyalari/etherchannel-lab%C4%B1.pkt) oluşturalım. 

 <img width="587" height="248" alt="image" src="https://github.com/user-attachments/assets/745d7617-967b-4247-b488-7c5edfd0fbd6" />

S1 switchinde port channel interface oluşturup bunu fiziksel arayüzlere ekleyelim.
tüm portların konfigürasyonun aynı olmasına dikkat edelim.

S1 
````
en
conf t
interface range g0/1-2
switchport mode trunk
channel-group 1 mode active
exit

# bundan sonra oluşturduğumuz mantıksal arayüze girip onu konfigüre edelim
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 1,2,3
````

konfigürasyonu incelemek istersen ``sh etherchannel port-channel`` komutuyla bakabilirsin.
````
S1#sh etherchannel port-channel 
                Channel-group listing:
                ----------------------

Group: 1
----------
                Port-channels in the group:
                ---------------------------

Port-channel: Po1    (Primary Aggregator)
------------

Age of the Port-channel   = 00d:02h:57m:26s
Logical slot/port   = 2/1       Number of ports = 2
GC                  = 0x00000000      HotStandBy port = null
Port state          = Port-channel 
Protocol            =   LACP
Port Security       = Disabled

Ports in the Port-channel:

Index   Load   Port     EC state        No of bits
------+------+------+------------------+-----------
  0     00     Gig0/2   Active             0
  0     00     Gig0/1   Active             0
Time since last port bundled:    00d:00h:28m:38s    Gig0/1
````

S2 Switch için de aynı şeyleri yapalım:
````
en
conf t
interface range g0/1-2
switchport mode trunk
channel-group 1 mode active
exit

interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 1,2,3
````

dikkat ettiysen her iki switchte de LACP mode olarak active seçtik. Peki bu modlar passive passive veya active passive olabilir miydi?
 active: bağlantı kurmak için aktif olarak paket gönderir
 passive: bağlantı isteğini bekler, kendi başına başlatmaz.
 bu durumda active-passive olur ama passive-passive olmaz diyebiliriz.


### L3 EtherChannel- Port Channel
 Bir switchi bir router'ın birden fazla arayüzüne bağladığımızda karşımıza çıkar.
 Routerın fiziksel arayüzlerin IP yapılandırılmaz. Elde edilen mantıksal porta IP yapılandırılması yeterli olur.

 Router Yapılandırılması:
 ````
conf t
int port-channel 1
ip address 20.2.2.2 255.255.255.0
````
şimdi fiziksel portları channel 1 e ekleyelim
````
int range g0/0-1
channel-group 1
````





 
