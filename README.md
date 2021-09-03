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
docker pull mongo<br /><br />

İndirilen image'ları görme (tag kısmı versiyonudur)<br />
docker images<br /><br />

Image çalıştırma (eğer daha önce yüklenmemişse önce yükler sonra çalıştırır)<br />
docker run mongo<br /><br />

İnteraktif terminalde image nasıl çalıştırılır (örn. linux işletim sistemleri, çıkmak için exit yazmalıyız)<br />
docker run -it ubuntu<br /><br />

Belli bir süre image nasıl çalıştırılır (5 sn)<br />
docker run ubuntu sleep 5<br /><br />

Çalışan image'ları görüntüleme (names otomatik atılır, dilersek kendimiz name ve param belirterek kendi imagemizi oluşturabiliriz)<br />
docker ps<br /><br />

Geçmişe dönük çalışan containerları görme<br />
docker ps -a veya docker container ls<br /><br />

Çalışan ve çalışmayan tüm containerları görme<br />
docker container ls -a<br /><br />

Containere isim verme (sonrasında docker run my_ubuntu şeklinde çalıştırılır)<br />
docker run -it --name my_ubuntu ubuntu<br /><br />

İsimlendirilmiş containeri çalıştırma (isimden veya container id den, container id'nin ilk 2-3 tanesini yazsak yeterli)<br />
docker start my_ubuntu (name id ile)<br />
docker start b40 (container id ile)<br /><br />

İsimlendirilmiş containeri durdurma (isimden veya container id den, container id'nin ilk 2-3 tanesini yazsak yeterli)<br />
docker stop my_ubuntu (name id ile)<br />
docker stop f7c (container id ile)<br /><br />

Container silme tekli<br />
docker rm f8d (container id ile)<br />
docker rm brave_kalam (name id ile)<br /><br />

Container silme çoklu (container id ile yan yana ilk üç harfleri yeterli)<br />
docker rm f8d c4h fg9 sjf c7g<br /><br />

Container silme tek seferde (script ile)<br />
docker container rm $(docker container ls -aq)<br />
veya docker container prune<br /><br />

Containerdeki uygulamayı spesifik versiyon/sürüm ile çalıştırma (sürümler dockerhub.com da tags bölümündedir)<br />
docker run redis:5<br /><br />

Bir containeri başka bir isimde klonlamak (redis'i my_redis ismi ile klonlayalım)<br />
docker image tag redis my_redis<br /><br />

Containeri arka planda çalıştırmak (detach mode)<br />
docker run -d redis<br /><br />

Arka planda çalıştırmak dockeri öne almak (attach mode, container id ile veya name ile)<br />
docker attach 56fe<br /><br />

Detach modda çalışan container loglarını görmek (container id ile veya name ile)<br />
docker container logs 56fe<br /><br />

Container port mapping<br />
docker run -p DIS_PORT:IC_PORT redis<br /><br />

Container volume mapping (mongo içindeki /data/db verilerini docker host içinde /opt/data kısmında sakla)<br />
* Dış kaynağı belirlemek için docker ayarlar -> resources -> file sharing kısmında ilgili klasörü girmelisin <br />
docker run -v DIS_SOURCE:IC_SOURCE mongo<br />
docker run -v /opt/data:/data/db mongo<br /><br />

Çalışan container ile ilgili detaylı bilgi<br />
docker inspect mongo<br /><br />

Container çalışırken env variable göndererek çalıştırmak<br />
docker run -e MYSQL_ROOT_PASSWORD=my_secret_password mysql<br /><br />

Container arası link vermek<br />
docker run --link mysql-server<br /><br />

İmajları listelemelek<br />
docker images<br /><br />

İmaj silmek<br />
docker rmi ubuntu<br /><br />

Link kullanımları<br /><br />

Mysql mysql-server adı ile dış ve iç 3306 portu ile volume kullanılarak, env variable ile, detach olarak çalışsın<br />
docker run --name mysql-server -p 3306:3306 -v /opt/data:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=test123 -d mysql<br /><br />

Phpmyadmin pma container adı ile, 8000 dış porttan 80 iç porta, mysql-server containerine db aliası ile linkli olarak detach modda çalışsın<br />
docker run --name pma -p 8000:80 --link mysql-server:db -d phpmyadmin/phpmyadmin<br /><br />

Yukarıdaki parametrelerle oluşturduğumuz isimlendirilmiş containeri run ile değil start ile çalıştırıyoruz<br />
docker start mysql-server<br />
docker start pma<br /><br />

Containeri bir network ile çalıştırma<br />
docker run mongo --network=none<br /><br />

Networkleri listeleme<br />
docker nerwork ls<br /><br />

Bir networku silme<br />
docker network rm networkadi/id<br /><br />

Bridge network oluşturma<br />
docker network create --driver bridge --subnet 170.1.0.1/24 --gateway 170.1.0.1 custom-network<br /><br />

Containeri custom network üzerinde çalıştırma (dışarıdan bu networkteki mongoya bağlanmayacağımız için port mapping yapmadık)<br />
docker run --name mongo-server --net custom-network -d mongo<br /><br />

Containeri custom network üzerinde çalıştırma 2 (mongo ile birlikte çalışacak uygulamamızı da custom network de çalıstıralım ancak erişmek için ip mapping lazım)<br />
docker run --net custom-network -p 3000:3000 -d my-app<br /><br />
