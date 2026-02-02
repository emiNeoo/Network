## STP Lab Çalışması 

Öncelikle üç switchlli loop oluşturması muhtemel bir topoloji oluşturdum sonrasında ise STP yapılanmasını kontrol ettim 

<img width="524" height="372" alt="image" src="https://github.com/user-attachments/assets/7d789d50-1dd0-4bab-ba24-6f0a545d92c3" />

switch 1 CLI

````
en
conf t
vlan 2
name vlan2
vlan 3
name vlan3
exit
interface range f0/1-2
switchport mode trunk
no shut
exit
exit
sh spanning-tree vlan 1
````

``sh panning tree vlan 1`` komutunun çıktısını bu şekilde gözlemleyebiliriz.

````
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0004.9AB4.088A
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0004.9AB4.088A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/1            Desg FWD 19        128.1    P2p
````

``this bridge is the root`` yazısından bu switchin root bridge olduğunu anlarız. portlarnın her ikisinin de designated olduğunu gözlemleyebiliriz.

diğer switchlerde de vlan ayarlarını yukarıdaki gibi yaptıktan ve portlarını trunk yaptıktann sonra aynı komutu çalıştıralım. 


````
Switch2#sh spanning-tree vlan 1
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0004.9AB4.088A
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0060.2F67.31D0
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/2            Altn BLK 19        128.2    P2p
````

````
Switch3#sh spanning-tree vlan 1
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0004.9AB4.088A
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     000A.F321.364A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/1            Root FWD 19        128.1    P2p
````

Switch 2 ve 3 ün root portlarını görebiliyoruz
Her üç switchin de Priority değerlerinin aynı olduğunu (default) görüyoruz
MAC adresi karşılaştırması yaptığımızda en küçük olanının switch 1 olduğunu sonuç olarak da onun root bridge olduğunu görürüz. 
Root bridge'i değiştirmek için ise priority değerini değiştirebiliriz.

Root bridge'i Switch2 yapmak istiyorsam onun prioritysini azaltmam gerekir 
Switch 2 CLI'ında :
````
conf t
spanning-tree vlan 2 priority 16384
````

````
VLAN0002
  Spanning tree enabled protocol ieee
  Root ID    Priority    16386
             Address     0060.2F67.31D0
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    16386  (priority 16384 sys-id-ext 2)
             Address     0060.2F67.31D0
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/2            Desg FWD 19        128.2    P2p
````

Switch 2'yi farklı bir komutla da root bridge yapabiliriz. Yine aynı şekilde Switch2 CLI'ında :

````
conf t
spanning-tree vlan 3 root primary
````

````
Switch2#sh spanning-tree vlan 3
VLAN0003
  Spanning tree enabled protocol ieee
  Root ID    Priority    24579
             Address     0060.2F67.31D0
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    24579  (priority 24576 sys-id-ext 3)
             Address     0060.2F67.31D0
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/2            Desg LIS 19        128.2    P2p
````


bir summary görmek istersek :

````
Switch#sh spanning-tree summary 
Switch is in pvst mode
Root bridge for: vlan2 vlan3
Extended system ID           is enabled
Portfast Default             is disabled
PortFast BPDU Guard Default  is disabled
Portfast BPDU Filter Default is disabled
Loopguard Default            is disabled
EtherChannel misconfig guard is disabled
UplinkFast                   is disabled
BackboneFast                 is disabled
Configured Pathcost method used is short

Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
VLAN0001                     1         0        0          1          2
VLAN0002                     0         0        0          2          2
VLAN0003                     0         0        0          2          2

---------------------- -------- --------- -------- ---------- ----------
3 vlans                      1         0        0          5          6
````








