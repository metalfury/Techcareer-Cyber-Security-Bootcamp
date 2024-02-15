Challenge dosyasini indirdim.

Log dosyasini notepad++ ile actim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107172508.png)

Buradaki;

access.log dosyasi bir apache sunucunun http loglarini iceren logdur.
192.168.199.1 : Client in ip adresi
20/Jun/2021:12:35:40 +0300 : Ilgili logun tarihi.
GET / HTTP/1.1": http istek kodu
200: http durum kodu
2506: boyut. byte cinsinden
Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:89.0) Gecko/20100101 Firefox/89.0" : client in cihaz bilgileri.
   
![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107173314.png)

1. soru:
bizden saldırganin kesif için hangi tarama aracını kullandigini sormus.

clientten farkli bir ip adresi aradigimizda bunun 192.168.199.2 oldugunu goruyoruz.
30 satiri ele alirsak:

   
![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231119180452.png)
   
192.168.199.2 - - [20/Jun/2021:12:36:24 +0300] "HEAD / HTTP/1.1" 200 - "-" "Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:Port Check)"

nikto araci ile tarama yapildigini gorebiliriz.

ilk sorunun cevabi : nikto

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107190427.png)

2. soru:
   
web tarama sonrasinda, dizin listeleme icin hangi teknik kullanilmistir sorusu sorulmus.

log dosyasini inceledigimda nikto araci kullaniminin 7566. satirda bittigini gozlemledim.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107192946.png)

bu satirdan sonra bwapp dizinine, bir "dizin wordlisti" yardimi ile wordlistteki dizin adlari ile bwapp dizininin alt dizinlerine denemeler yapildigini gozlemledim. bazi sayfalarin "404 300" bazilarinin "403 303" 	kodlari ile dondugunu goruyorum. 

192.168.199.2 - - [20/Jun/2021:12:37:50 +0300] "GET /bwapp/.cvsignore HTTP/1.1" 404 300 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)"
   
192.168.199.2 - - [20/Jun/2021:12:37:50 +0300] "GET /bwapp/.htpasswd_ HTTP/1.1" 403 303 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)"
   
ikinci sorumuzun cevabi: directory brute force

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107194547.png)

3. soru

ucuncu soruda bizden dizin listelemeden sonra hangi atak tipinin kullanildigi sorulmus.

bu senaryoya genel olarak baktigimizda bu kisinin dizinler icerisinden giris sayfasini bulmaya calistigini anlayabiliriz. 

ki 12414. satirda 192.168.199.2 - - [20/Jun/2021:12:41:41 +0300] "POST /bWAPP/login.php HTTP/1.1" 200 4086 "http://192.168.199.5/bWAPP/login.php" "Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0"

login.php sayfasina post istekleri yolladigini gormekteyiz. bu da bize olasi bir brute force atagi gerceklestirdigini soyleyebilir.

ucuncu sorunun cevabi: brute force

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107195330.png)

4. soru

bize ucuncu atagin basarili olup olmadigini sormus. 

12546 satira geldigimizde 302 http kodunu gormekteyiz.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231119201855.png)

bu kod bize gecici yonlendirme oldugunu belirtmekte ve yapilacak saldirilarin portal.php uzerinden yapilabilecegini anlatmakta. genel olarak 12546 satirdan sonrasina baktigimizda bruteforce ataginin basarili oldugunu ve istenilen komutlarin calistirilabildigini gorebilmekteyiz.

cevap: yes

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107201937.png)

5. soru

burada bizden dorduncu atagin ne oldugunu sormus.

12551 satira geldigimizde phpnin yanina kod eklemesi yapilarak denemeler yapildigini gormekteyiz.

bu tur ataklara code infection denmektedir

besinci sorunun cevabi: code injection

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107202032.png)

6. soru
    
bize 4. atagin payload ini sormus.

yani bir kod denemesi yaparken ekleyecegimiz parcayi sormakta.

12553 satira baktigimizda payload in "whoami" oldugunu gorebiliriz.
 
![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231119201926.png)
 
192.168.199.2 - - [20/Jun/2021:12:52:36 +0300] "GET /bWAPP/phpi.php?message=%22%22;%20system(%27whoami%27) HTTP/1.1" 200 12778 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0"

cevap: whoami 

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107202109.png)

7. soru
    
bu soruda bize saldirganin hedef makinede kalici olarak bir iz birakip birakmadigini sormus. bu bir backdoor veya bir kullanici olabilir.

12556 satirinda "192.168.199.2 - - [20/Jun/2021:12:53:13 +0300] "GET /bWAPP/phpi.php?message=%22%22;%20system(%27net%20user%20hacker%20Asd123!!%20/add%27) HTTP/1.1" 200 12755 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0" " saldirganin "hacker" "Asd123" kombinasyonunda bir kullanici olusturdugunu gorebiliriz.

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231119201956.png)

payload: %27net%20user%20hacker%20Asd123!!%20/add%27

![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107202304.png)


![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/log_analysis/src/Pasted_image_20231107202415.png)

.
