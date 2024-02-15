1. WebFilter Olusturma
	-Arayuz -> Security Profiles -> Create New diyerek yeni bir web filter olusturulur.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112000708.png)

	-web filter profilinde bizden istenen "sosyal medya ve youtube kategorisi engelleme" islemini gerceklestirelim. opsiyonel olarak diger kategoriler engellenebilir, izlenebilir veya kategorilere izin verilebilir.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112000921.png)

	-ben opsiyonel olarak bazi kategorilere engel ve monitor koydum.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112002244.png)

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112002300.png)

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112001046.png)

	
	-youtube engellemesi yapmak istedigimizden asagidaki Static URL Filter -> URL Filter'i etkin hale getirmeliyiz.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112001638.png)

	-daha sonra create new diyerek youtube wildcardi olusturmaliyiz. bu youtube domaini uzerindeki tum subdomaninleri kapsayarak engelleyecektir.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112001831.png)

	-ok deyip kayit edit web filterimizi kayit ediyoruz.

3. Application Filter Olusturma
   
	-Arayuz -> Security Profiles -> Application Control create new diyerek yeni bir application filter olusturulur.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112002636.png)

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112003731.png)

	-application filter a bir isim verilir ve bizden istenen "sosyal medya engeli" istegi icin "social media" monitor den block a cekilir. ok diyerek kayit edilir ve application filter olusturulur.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112003841.png)

5. Kural olusturarak webfilter ve application filter'i devreye sokma
   
	-oncelikle Network -> Interfaces kismindan internete hangi cihazin hangi arayuz uzerinden ciktigina bakilir. bizim senaryomuzda windows-lan cihazi WAN portundan internete cikmakta.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112004214.png)

	-daha sonra Policy&Objects -> IPv4 Policy kismindan create new diyerek bir kural olusturulur.
	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112004302.png)

	-kurala isim olarak kuralin icerigini ozet olarak verecek bir isim koyduk.
	"merts_socials_youtube_Deny"

	-incoming interface: kurali hangi yonden gelen trafik icin yaziyoruz bunu seciyoruz. internete cikan windows-lan aygitini sectik.

	-outgoing interface: kurali trafigin Hangi yöne giden trafik icin yaziyoruz bunu yaziyoruz.

	-source, destination, service = all. bu kisimlarda kaynak ve hedef cihazlardan; istenilen portlardan istenilen portlara, istenilen servisleri kullanarak cikis yapmasini istedigimiz icin all diyoruz.

	-internete cikis yapacagimiz icin nat ayarimizi aciyoruz.

	-security profiles kismindan Web Filter ve ApplicationControl u acip daha once olusturugumuz filtreleri ekliyoruz.

	-OK diyerek kuralimizi olusturuyoruz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112004717.png)

	-kuralimizi devreye aldik. simdi test edelim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112005704.png)

	-rdp ile makinelerimizden birine baglanalim.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112005833.png)

	-ekledigimiz filtreler dogrultusunda internete erisimimiz olmasina ragmen youtube'a ve sosyal medya sitelerine erisim saglayamiyoruz.

	![](https://github.com/metalfury/Techcareer-Cyber-Security-Bootcamp/blob/main/firewall_ops/src/Pasted_image_20231112011233.png)

