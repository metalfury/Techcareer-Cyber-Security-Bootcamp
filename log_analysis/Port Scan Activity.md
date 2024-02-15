pcap dosyasini indirdim.
wireshark ile pcap dosyasini actim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108003446.png)
   
elimizde 4 adet ip adresi var. bunlar:
   
10.42.42.25, 10.42.42.50, 10.42.42.56 ve 10.42.42.253
   
soru1
bizden ortami tarayan ip adresini istemis.
 
3way handshake de ilk once SYN paketi gonderilir.
 
sorgu denemesi yaptim ve su sorguyu calistirdim.
 
"tcp.flags.syn= =1" bu sorgu sonucunda tarama yapan ip adresinin yolladigi paketleri gozlemledim. IP adresi: 10.42.42.253
 
cevap: 10.42.42.253
 
![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108003651.png)
 
soru2
bizden tarama sonrasi bulunan ip adresini istemis.
 
sonrasinda sorguyu "ip.src= =10.42.42.253&&tcp.flags.ack= =1" olarak degistirdim. bu sayede ip taramasi yapan ip adresiyle 3way handshake i yakalayan, daha sonra bitiren bir ip adresi bulmaya calistim. sonuc 	olarak 10.42.42.50 ip adresini buldum.
 
![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108025538.png)
 
cevap: 10.42.42.50
 
![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108025600.png)
 
soru3

bizden apple cihazin mac adresi istenmis. halihazirda 4 adet ip adresi mevcut.
253 - quantacomput
50 - compalinform
56 - compalinform
25 - apple

iplere bakip paketlere sirayla tikladigim zaman 6 numarali paketin kaynak mac adresinin apple bir cihaza ait oldugunu gordum. 

Ethernet II, Src: Apple_92:6e:dc (00:16:cb:92:6e:dc), Dst: QuantaCo_82:1f:4a (00:23:8b:82:1f:4a) ip adresi: 10.42.42.25

cevap: 00:16:cb:92:6e:dc 

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108024056.png)

soru4

bizden windows sistemin ip adresi istenmis.

seceneklerimiz daraldi. 
253 tarama yapan ip adresi. 

25 apple cihazin ip adresi.

ip.src= =10.42.42.56 sorgusunu yaptim ve paketleri inceledim. ek olarak protokolleri inceledigimde gozume carpan bir paket olmadi. ICMP protokolu her cihazda calisabileceginden diger secenege gectim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108030033.png)

ip.src= =10.42.42.50 sorgusunu calistirdim. 6072 pakete geldigimde nbns protokolunu gordum. bu protokolun netbios name server protokolu oldugunu ve windows makinelerde calistigini ogrendim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108030223.png)

bu sebeple windows sistemin ip adresi 10.42.42.50 dir.
cevap: 10.42.42.50

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108030338.png)

.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231108030406.png)
