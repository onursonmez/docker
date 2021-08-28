# docker

Roket hızda docker notlarım.

Docker nedir?
Windows, Linux veya Mac işletim sistemlerine kurulabilen, ana işletim sistemi altına çaktırmadan bir ara linux işletim sistemi kurarak containerlarını
yöneten organizasyondur. Sanal makinelere göre çok hızlıdır. Çünkü ana işletim sisteminin kaynaklarını kullanır ve containerlar içinde işletim sistemi
olmadığı için çok hızlı açılıp, kapanır.

Container nedir?
Sanal makinalar (VM) farklı olarak containerlar içinde "işletim sistemi olmadan" sadece libraries ve depencies'lar bulunur.
Böylece çok hızlı açılıp, kapanır. Her bir container içerisinde bir veya birden fazla uygulama (nginx, mongo, redis vb.) kurulabilir.
Container'lar DockerHost altında gruplanır ve her container'in kendine özel IP'si vardır.

Dockerhub.com nedir?
Resmi ve gayriresmi firmalar ve organizasyonlar tarafından uygulamalarının bir container içinde hazır olarak bekletildiği portaldır.
Bu hazır uygulamalara image denir. İhtiyacınız olan image'i tek tıklama ile indirip, çalıştırabilirsiniz. "Official image" etiketi bulunan image'lar
uygulamanın Docker tarafından yüklenildiği anlamına gelir. Verified Publisher etiketle image'lar o etiketin üreticisi tarafından resmi olarak 
yüklenildiği anlamına gelir.

Docker nasıl yönetilir?
Docker kurulduğunda bir GUI arayüzü ile tüm containerlar yönetilebilir. Ancak istersek CLI ile komut satırından hızlı bir biçimde istediğimiz herşeyi yapabiliriz.

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
docker rm brave_kalam (container id ile)

Container silme çoklu (container id ile yan yana ilk üç harfleri yeterli)
docker rm f8d c4h fg9 sjf c7g

Container silme tek seferde (script ile)
docker container rm $(docker container ls -aq)

Containerdeki uygulamayı spesifik versiyon/sürüm ile çalıştırma (sürümler dockerhub.com da tags bölümündedir)
docker run redis:5

Bir containeri başka bir isimde klonlamak (redis'i my_redis ismi ile klonlayalım)
docker image tag redis my_redis

Containeri arka planda çalıştırmak (detach mode)
docker run -d redis

Arka planda çalıştırmak dockeri öne almak (attach mode, container id ile)
docker attach 56fe

