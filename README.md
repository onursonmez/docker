# docker

Roket hızda docker notlarım.\\

Docker nedir?\
Windows, Linux veya Mac işletim sistemlerine kurulabilen, ana işletim sistemi altına çaktırmadan bir ara linux işletim sistemi kurarak containerlarını
yöneten organizasyondur. Sanal makinelere göre çok hızlıdır. Çünkü ana işletim sisteminin kaynaklarını kullanır ve containerlar içinde işletim sistemi
olmadığı için çok hızlı açılıp, kapanır.\\

Container nedir?\
Sanal makinalar (VM) farklı olarak containerlar içinde "işletim sistemi olmadan" sadece libraries ve depencies'lar bulunur.
Böylece çok hızlı açılıp, kapanır. Her bir container içerisinde bir veya birden fazla uygulama (nginx, mongo, redis vb.) kurulabilir.
Container'lar DockerHost altında gruplanır ve her container'in kendine özel IP'si vardır. Docker host üzerinde bulunan containerlar stateless çalışır.
Yani üzerlerinde herhangi bir bilgi kayıt edilmez. Eğer container durursa içinde yapılan tüm değişiklikler uçar. Bilgilerin kalıcı olması için volume
kullanılmalıdır.

Dockerhub.com nedir?
Resmi ve gayriresmi firmalar ve organizasyonlar tarafından uygulamalarının bir container içinde hazır olarak bekletildiği portaldır.
Bu hazır uygulamalara image denir. İhtiyacınız olan image'i tek tıklama ile indirip, çalıştırabilirsiniz. "Official image" etiketi bulunan image'lar
uygulamanın Docker tarafından yüklenildiği anlamına gelir. Verified Publisher etiketle image'lar o etiketin üreticisi tarafından resmi olarak 
yüklenildiği anlamına gelir.

Docker nasıl yönetilir?
Docker kurulduğunda bir GUI arayüzü ile tüm containerlar yönetilebilir. Ancak istersek CLI ile komut satırından hızlı bir biçimde istediğimiz herşeyi yapabiliriz.

Docker network yapısı nasıldır?
Docker'da bridge, none ve host olmak üzere 3 network vardır. Standart olarak kullandığımız bridge networktür. Docker host altında birbirine bağlı containerlerdan oluşur. None network dışarıdan ve içeriden ulaşılamayan networktür. Örneğin bir log uygulaması çalıştırıyoruz ve hiçbir şekilde dışarıdan veya içeriden erişmeyeceğiz. Bu durumda none network kullanırız.

Docker komutları nedir?

Dockerhub.com dan image'i bilgisayara indirme (indirir, çalıştırmaz)
docker pull mongo

İndirilen image'ları görme (tag kısmı versiyonudur)
docker images

Image çalıştırma (eğer daha önce yüklenmemişse önce yükler sonra çalıştırır)
docker run mongo

İnteraktif terminalde image nasıl çalıştırılır (örn. linux işletim sistemleri, çıkmak için exit yazmalıyız)
docker run -it ubuntu

Belli bir süre image nasıl çalıştırılır (5 sn)
docker run ubuntu sleep 5

Çalışan image'ları görüntüleme (names otomatik atılır, dilersek kendimiz name ve param belirterek kendi imagemizi oluşturabiliriz)
docker ps

Geçmişe dönük çalışan containerları görme
docker ps -a veya docker container ls

Çalışan ve çalışmayan tüm containerları görme
docker container ls -a

Containere isim verme (sonrasında docker run my_ubuntu şeklinde çalıştırılır)
docker run -it --name my_ubuntu ubuntu

İsimlendirilmiş containeri çalıştırma (isimden veya container id den, container id'nin ilk 2-3 tanesini yazsak yeterli)
docker start my_ubuntu (name id ile)
docker start b40 (container id ile)

İsimlendirilmiş containeri durdurma (isimden veya container id den, container id'nin ilk 2-3 tanesini yazsak yeterli)
docker stop my_ubuntu (name id ile)
docker stop f7c (container id ile)

Container silme tekli
docker rm f8d (container id ile)
docker rm brave_kalam (name id ile)

Container silme çoklu (container id ile yan yana ilk üç harfleri yeterli)
docker rm f8d c4h fg9 sjf c7g

Container silme tek seferde (script ile)
docker container rm $(docker container ls -aq)
veya docker container prune

Containerdeki uygulamayı spesifik versiyon/sürüm ile çalıştırma (sürümler dockerhub.com da tags bölümündedir)
docker run redis:5

Bir containeri başka bir isimde klonlamak (redis'i my_redis ismi ile klonlayalım)
docker image tag redis my_redis

Containeri arka planda çalıştırmak (detach mode)
docker run -d redis

Arka planda çalıştırmak dockeri öne almak (attach mode, container id ile veya name ile)
docker attach 56fe

Detach modda çalışan container loglarını görmek (container id ile veya name ile)
docker container logs 56fe

Container port mapping
docker run -p DIS_PORT:IC_PORT redis

Container volume mapping (mongo içindeki /data/db verilerini docker host içinde /opt/data kısmında sakla)
* Dış kaynağı belirlemek için docker ayarlar -> resources -> file sharing kısmında ilgili klasörü girmelisin 
docker run -v DIS_SOURCE:IC_SOURCE mongo
docker run -v /opt/data:/data/db mongo

Çalışan container ile ilgili detaylı bilgi
docker inspect mongo

Container çalışırken env variable göndererek çalıştırmak
docker run -e MYSQL_ROOT_PASSWORD=my_secret_password mysql

Container arası link vermek
docker run --link mysql-server

İmajları listelemelek
docker images

İmaj silmek
docker rmi ubuntu

Link kullanımları

Mysql mysql-server adı ile dış ve iç 3306 portu ile volume kullanılarak, env variable ile, detach olarak çalışsın
docker run --name mysql-server -p 3306:3306 -v /opt/data:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=test123 -d mysql

Phpmyadmin pma container adı ile, 8000 dış porttan 80 iç porta, mysql-server containerine db aliası ile linkli olarak detach modda çalışsın
docker run --name pma -p 8000:80 --link mysql-server:db -d phpmyadmin/phpmyadmin

Yukarıdaki parametrelerle oluşturduğumuz isimlendirilmiş containeri run ile değil start ile çalıştırıyoruz
docker start mysql-server
docker start pma

Containeri bir network ile çalıştırma
docker run mongo --network=none

Networkleri listeleme
docker nerwork ls

Bir networku silme
docker network rm networkadi/id

Bridge network oluşturma
docker network create --driver bridge --subnet 170.1.0.1/24 --gateway 170.1.0.1 custom-network

Containeri custom network üzerinde çalıştırma (dışarıdan bu networkteki mongoya bağlanmayacağımız için port mapping yapmadık)
docker run --name mongo-server --net custom-network -d mongo

Containeri custom network üzerinde çalıştırma 2 (mongo ile birlikte çalışacak uygulamamızı da custom network de çalıstıralım ancak erişmek için ip mapping lazım)
docker run --net custom-network -p 3000:3000 -d my-app
