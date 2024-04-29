# Windows Server Failover Cluster Kurulumu ve Yapilandirmasi
<br>
<div><span style="text-decoration: underline;"><strong></strong></span><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ1.png"></div>
<div><br>
</div>
<div>MS SQL Always On mimarisi gereği minimum iki sunucudan oluşmaktadır. Windows Server işletim sistemi ailesi rölelerinden olan failover cluster üzerine kurgulanmaktadır. Always on kurulumu için aşağıdakilere ihtiyacınız olacaktır.
</div>
<ul>
    <li>2 adet Windows Server 2016 yada 2019 kurulu sunucu </li>
    <li>Windows Server işletim sistemleri üzerinde Failover Cluster role kurulumu </li>
    <li>DNS suffix yapılandırması </li>
    <li>Minimum 3 adet IP adresi: 1 IP birinci sunucunuz için, 1 IP ikinci sunucunuz için, 1 IP Failover cluster için ihtiyacınız olacaktır.
    </li>
</ul>
<div>İlk olarak yapılandırma işlemlerine kurulmuş olan Windows Server işletim sistemli sunucularımızın DNS suffix yapılandırması ile başlayınız. Kurulum ve yapılandırma süreçlerinde iki sunucu ile eş zamanlı çalışma yapmamız gereken durumlar olduğu için iki
sunucuyu aynı anda göstereceğim. Sol taraf da bulunan ekran 1. Sunucuya sağ tarafta bulunan ekran 2. Sunucuya aittir. Control Panel\All Control Panel Items\System penceresinde sol bölümde bulunan menüden "Advanced system settings" tıklayınız.
<img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ2.png">
<br>
</div>
<div>"System Properties" penceresinde "Computer Name" sekmesinde "Change" butonuna basınız. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ3.png">
<br>
</div>
<div>"Computer name / domain changes" penceresinde "more" butonuna basınız. "DNS Suffix and netbios computer name" penceresinde "Primary DNS suffix of this computer" bölümüne failover clusterimiz için bir domain belirliyoruz. Bunun için önerim domain.local
şeklinde bir tanımlama yapmanızdır. Talimat kapsamında ben "test.local" olarak tanımlamamı yaparak devam edeceğim.</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ4.png">
<br>
</div>
<div>İşlemler sonrası DNS suffix etkin olması için sistemin yeninden başlatılması gerekmektedir. Sisteminiz yeninden başladıktan sonra IPv6'nın kapalı olduğunu kontrol ediniz. Not: IPv6 kapatmamızın sebebi failover cluster kurulumundan sonra tanımladığımız
DNS suffix sonrası oluşan FQDN lerin dns çözümlemelerini IPv4 üzerinden yapacağımız için. Uygulama IPv6 kullanıyorsa FQDNlere A kaydı tanımlarken IPv6 girilmesi gerekmektedir. Talimat kapsamında IPv6 kapalı işlem yapılmıştır. Windows Server işletim sistemimizde
failover cluster kurulumunu hızlıca tamamlamak için PowerShell açınız ve aşağıdaki komutu çalıştırınız. Install-WindowsFeature Failover-Clustering -IncludeManagementTools <br>
</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ5.png"><br>
</div>
<div>PowerShell üzerinde failover cluster kurulumunuz tamamlanmıştır. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ6.png">
<br>
</div>
<div>Kurulum sonrası failover clusteriniz public IP üzerinden DNS A kaydı ekleyerek çalıştırabilirsiniz. Clsuter güvenliği için önerilen local IP adresi ile çalışması ve public erişiminin doğrudan olmamasıdır. Bu tip durumlarda DNS tanımlamaları için Windows
Host dosyasını kullanabilirsiniz. FQDN adresleri: Server1: FC-Ozan.test.local Server2: FC-Ozan2.test.local Cluster FQDN: cluster.test.local Faiover cluster kurulumumuz tamamladıktan sonra yapılandırmamız gerekiyor. Yapılandırma sırasında bağlantı sorunu yaşamamak
için host dosyasına belirlediğimiz FQDN adreslerini ver IP adreslerini giriyoruz. Birinci sunucumuza tanımlama yaparken ikinici sunucumuzun FQDN adresini ve IP adresini giriyoruz. İkinci sunucumuza tanımlama yaparken birinci sunucmuzun FQDN adresini ve IP
adresini giriyoruz. Her iki sunucumuza cluster FQDN adresleri ve IP adreslerini giriyoruz.</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ7.png">
<br>
</div>
<div>Failover Cluster Manager çalıştırınız. Failover cluster kurulumu işlemlerini yapınızda bulunan birinci sunucuda yapmanız yeterlidir. Bu bölümdeki işlemler sadece birinci sunucuda yapılmıştır.</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ8.png">
<br>
</div>
<div>"Failover Cluster Manager" penceresinde sağ bölümde bulunan menüden "Create Cluster" basınız. Create Cluster Wizard penceresinde Before you begin adımını next butonuna basarak geçiniz. Select Servers bölümünden FQDN adresleri ile birinci ve ikinci sunucularınızı
ekleyiniz. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ9.png">
<br>
</div>
<div>Validation warning adımında "yes" opsiyonunu seçerek Next butonuna basınız. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ10.png">
<br>
</div>
<div>Validation ekranında next butonuna basarak devam ediniz. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ11.png"><br>
</div>
<div>Validating işlemi bittikten sonra finish butonuna basınız.</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ12.png">
<br>
</div>
<div>Clsuter kurulumunun "Access point for administering the cluster" adımında failover clusterimiz için isim ve IP tanımlamasını yapınız. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ13.png"><br>
</div>
<div>Next butonuna basınız.Confirmation ekranında ayarlarınızı kontrol edin ve next butonuna tekrar basınız. "Creating New Cluster" işleminiz başlayacaktır. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ14.png">
<br>
</div>
<div>Summary ekranında işlem özetinizi görüntülüyorsunuz.</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ15.png">
<br>
</div>
<div>Failover clusterimizda bulunan 2. Node etkin duruma getirmek için ikinci sunucumuzda işlemlere başlıyoruz. <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ16.png">
<br>
</div>
<div>Failover Cluster Manager da sağ bölümde bulunan Connect to cluster butonuna basınız. Cluster FQDN adresini yazınız ok butonuna basınız <br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ17.png">
<br>
</div>
<div>Kısa süre içerisinde cluster ile bağlantısı tamamlanacaktır. <br>
</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ18.png">
<br>
</div>
<div>Cluster kurulum işlemimiz tamamlanmıştır. <br>
</div>
<div><br>
</div>
<div><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/09/092319_1640_WindowsServ19.png"></div>
