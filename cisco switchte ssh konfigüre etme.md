## Görev
Bana verilen görevde Catalyst 3560 switchine ssh konfigüre etmem ve sonrasında host cihazımda putty arayüzünden ssh test etmem istendi

1. cisco cihaz ile bilgisayarımı power kablosu ile bağladım ve putty terminalinden switch arayüzüne ulaştım.
2. cisco cihazda şu yapılandırmaları yaptım:
```
enable
conf t
username emine password emine
ip domain name emine
line vty 0 15
crypto key generate rsa
line vty0 15 transport input ssh
```

bu adımları yapıp ethernet kablosu ile switch ve bilgisyarımı bağladım ve putty terminalini açıp ssh seçeneğiyle swithe vermiş olduğum ip i girdim ve okeylediğimde bağlantının sağlandığını gördüm.


