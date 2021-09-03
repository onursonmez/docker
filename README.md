# docker

Roket hızda docker notlarım.<br /><br />

Docker nedir?<br />
Windows, Linux veya Mac işletim sistemlerine kurulabilen, ana işletim sistemi altına çaktırmadan bir ara linux işletim sistemi kurarak containerlarını
yöneten organizasyondur. Sanal makinelere göre çok hızlıdır. Çünkü ana işletim sisteminin kaynaklarını kullanır ve containerlar içinde işletim sistemi
olmadığı için çok hızlı açılıp, kapanır.<br /><br />

Container nedir?<br />
Sanal makinalar (VM) farklı olarak containerlar içinde "işletim sistemi olmadan" sadece libraries ve depencies'lar bulunur.
Böylece çok hızlı açılıp, kapanır. Her bir container içerisinde bir veya birden fazla uygulama (nginx, mongo, redis vb.) kurulabilir.
Container'lar DockerHost altında gruplanır ve her container'in kendine özel IP'si vardır. Docker host üzerinde bulunan containerlar stateless çalışır.
Yani üzerlerinde herhangi bir bilgi kayıt edilmez. Eğer container durursa içinde yapılan tüm değişiklikler uçar. Bilgilerin kalıcı olması için volume
kullanılmalıdır.<br /><br />

Dockerhub.com nedir?<br />
Resmi ve gayriresmi firmalar ve organizasyonlar tarafından uygulamalarının bir container içinde hazır olarak bekletildiği portaldır.
Bu hazır uygulamalara image denir. İhtiyacınız olan image'i tek tıklama ile indirip, çalıştırabilirsiniz. "Official image" etiketi bulunan image'lar
uygulamanın Docker tarafından yüklenildiği anlamına gelir. Verified Publisher etiketle image'lar o etiketin üreticisi tarafından resmi olarak 
yüklenildiği anlamına gelir.<br /><br />

Docker nasıl yönetilir?<br />
Docker kurulduğunda bir GUI arayüzü ile tüm containerlar yönetilebilir. Ancak istersek CLI ile komut satırından hızlı bir biçimde istediğimiz herşeyi yapabiliriz.<br /><br />

Docker network yapısı nasıldır?<br />
Docker'da bridge, none ve host olmak üzere 3 network vardır. Standart olarak kullandığımız bridge networktür. Docker host altında birbirine bağlı containerlerdan oluşur. None network dışarıdan ve içeriden ulaşılamayan networktür. Örneğin bir log uygulaması çalıştırıyoruz ve hiçbir şekilde dışarıdan veya içeriden erişmeyeceğiz. Bu durumda none network kullanırız.<br /><br />

Docker komutları nedir?<br /><br />

Dockerhub.com dan image'i bilgisayara indirme (indirir, çalıştırmaz)<br />
<code>docker pull mongo</code></code><br /><br />

İndirilen image'ları görme (tag kısmı versiyonudur)<br />
<code>docker images</code><br /><br />

Image çalıştırma (eğer daha önce yüklenmemişse önce yükler sonra çalıştırır)<br />
<code>docker run mongo</code><br /><br />

İnteraktif terminalde image nasıl çalıştırılır (örn. linux işletim sistemleri, çıkmak için exit yazmalıyız)<br />
<code>docker run -it ubuntu</code><br /><br />

Belli bir süre image nasıl çalıştırılır (5 sn)<br />
<code>docker run ubuntu sleep 5</code><br /><br />

Çalışan image'ları görüntüleme (names otomatik atılır, dilersek kendimiz name ve param belirterek kendi imagemizi oluşturabiliriz)<br />
<code>docker ps</code><br /><br />

Geçmişe dönük çalışan containerları görme<br />
<code>docker ps -a veya docker container ls</code><br /><br />

Çalışan ve çalışmayan tüm containerları görme<br />
<code>docker container ls -a</code><br /><br />

Containere isim verme (sonrasında docker run my_ubuntu şeklinde çalıştırılır)<br />
docker run -it --name my_ubuntu ubuntu</code><br /><br />

İsimlendirilmiş containeri çalıştırma (isimden veya container id den, container id'nin ilk 2-3 tanesini yazsak yeterli)<br />
<code>docker start my_ubuntu</code> (name id ile)<br />
<code>docker start b40</code> (container id ile)<br /><br />

İsimlendirilmiş containeri durdurma (isimden veya container id den, container id'nin ilk 2-3 tanesini yazsak yeterli)<br />
<code>docker stop my_ubuntu</code> (name id ile)<br />
<code>docker stop f7c</code> (container id ile)<br /><br />

Container silme tekli<br />
<code>docker rm f8d</code> (container id ile)<br />
<code>docker rm brave_kalam</code> (name id ile)<br /><br />

Container silme çoklu (container id ile yan yana ilk üç harfleri yeterli)<br />
<code>docker rm f8d c4h fg9 sjf c7g</code><br /><br />

Container silme tek seferde (script ile)<br />
<code>docker container rm $(docker container ls -aq)</code> veya<br />
<code>docker container prune</code><br /><br />

Containerdeki uygulamayı spesifik versiyon/sürüm ile çalıştırma (sürümler dockerhub.com da tags bölümündedir)<br />
<code>docker run redis:5</code><br /><br />

Bir containeri başka bir isimde klonlamak (redis'i my_redis ismi ile klonlayalım)<br />
<code>docker image tag redis my_redis</code><br /><br />

Containeri arka planda çalıştırmak (detach mode)<br />
<code>docker run -d redis</code><br /><br />

Arka planda çalıştırmak dockeri öne almak (attach mode, container id ile veya name ile)<br />
<code>docker attach 56fe</code><br /><br />

Detach modda çalışan container loglarını görmek (container id ile veya name ile)<br />
<code>docker container logs 56fe</code><br /><br />

Container port mapping<br />
<code>docker run -p DIS_PORT:IC_PORT redis</code><br /><br />

Container volume mapping (mongo içindeki /data/db verilerini docker host içinde /opt/data kısmında sakla)<br />
* Dış kaynağı belirlemek için docker ayarlar -> resources -> file sharing kısmında ilgili klasörü girmelisin <br />
<code>docker run -v DIS_SOURCE:IC_SOURCE mongo</code><br />
<code>docker run -v /opt/data:/data/db mongo</code><br /><br />

Çalışan container ile ilgili detaylı bilgi<br />
<code>docker inspect mongo</code><br /><br />

Container çalışırken env variable göndererek çalıştırmak<br />
<code>docker run -e MYSQL_ROOT_PASSWORD=my_secret_password mysql</code><br /><br />

Container arası link vermek<br />
<code>docker run --link mysql-server</code><br /><br />

İmajları listelemelek<br />
<code>docker images</code><br /><br />

İmaj silmek<br />
<code>docker rmi ubuntu</code><br /><br />

Link kullanımları<br /><br />

Mysql mysql-server adı ile dış ve iç 3306 portu ile volume kullanılarak, env variable ile, detach olarak çalışsın<br />
<code>docker run --name mysql-server -p 3306:3306 -v /opt/data:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=test123 -d mysql</code><br /><br />

Phpmyadmin pma container adı ile, 8000 dış porttan 80 iç porta, mysql-server containerine db aliası ile linkli olarak detach modda çalışsın<br />
<code>docker run --name pma -p 8000:80 --link mysql-server:db -d phpmyadmin/phpmyadmin</code><br /><br />

Yukarıdaki parametrelerle oluşturduğumuz isimlendirilmiş containeri run ile değil start ile çalıştırıyoruz<br />
<code>docker start mysql-server</code><br />
<code>docker start pma</code><br /><br />

Containeri bir network ile çalıştırma<br />
<code>docker run mongo --network=none</code><br /><br />

Networkleri listeleme<br />
<code>docker nerwork ls</code><br /><br />

Bir networku silme<br />
<code>docker network rm networkadi/id</code><br /><br />

Bridge network oluşturma<br />
<code>docker network create --driver bridge --subnet 170.1.0.1/24 --gateway 170.1.0.1 custom-network</code><br /><br />

Containeri custom network üzerinde çalıştırma (dışarıdan bu networkteki mongoya bağlanmayacağımız için port mapping yapmadık)<br />
<code>docker run --name mongo-server --net custom-network -d mongo</code><br /><br />

Containeri custom network üzerinde çalıştırma 2 (mongo ile birlikte çalışacak uygulamamızı da custom network de çalıstıralım ancak erişmek için ip mapping lazım)<br /><code>docker run --net custom-network -p 3000:3000 -d my-app</code><br /><br />
