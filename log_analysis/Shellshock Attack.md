pcap dosyasini indirdim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107203842.png)

Wireshark ile pcap dosyasini actim. 

pcap packet capture dosyasi oldugundan bu dosya tipini parse edebilen bir uygulama olan wiresharki sectim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107204821.png)

oncelikle goze carpanlar:
ip adresleri:
10.246.50.2 , 10.246.50.4, 10.246.50.6
http ssh ve icmp protokolleri uzerinden iletisim saglanmaya calisilmis.

1.soru

server isletim sistemini sorulmus.

ikinci http paketinde "internal server error" basligi bulunmakta. burada server ile ilgili bilgi olabilir diye dusundum ve paketi actim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107205023.png)

burada http protokolu uzerinde bir apache server calistigini ve isletim sisteminin ubuntu oldugunu gorebiliriz.

cevap: ubuntu

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107205117.png)

2.soru

serverin turunu ve versiyonunu sorulmus.

yine ayni pakette bu bilgilere erisebiliriz.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107205255.png)

cevap: Apache/2.2.22

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107205323.png)

3.soru

saldirganin hedef serverdaki calistirmak istedigi komut sorulmus.

paketlere baktigimizda server normal akisinda ilerlerken "internal server error" verdigini gormekteyiz. bundan once bir sey olmali ki bir hata olussun. bu sebeple ondan bir onceki http paketini actigimizda ve http protokolunu inceledigimizde user-agent in /bin/ping -c1 10.246.50.2 komutunu calistirdigini gorebiliriz.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107210247.png)

cevap: /bin/ping -c1 10.246.50.2

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107210232.png)

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107210313.png)

.
