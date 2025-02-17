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

Container'a isim verme (sonrasında docker run my_ubuntu şeklinde çalıştırılır)<br />
<code>docker run -it --name my_ubuntu ubuntu</code><br /><br />

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
İpucu: Dış kaynağı belirlemek için docker ayarlar -> resources -> file sharing kısmında ilgili klasörü girmelisin <br />
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

(https://bro-mckinney-2.blogbright.net/online-shopping-is-comely-sir-thomas-more-popular-because-mass-are-straight-off-meddling-more-than-ever-they-are-doing-their-shopping-online-so-that-they-do-non-hold-to-force-or-take-th
)[https://bro-mckinney-2.blogbright.net/online-shopping-is-comely-sir-thomas-more-popular-because-mass-are-straight-off-meddling-more-than-ever-they-are-doing-their-shopping-online-so-that-they-do-non-hold-to-force-or-take-th
]<br />(https://bro-rankin.technetbloggers.de/about-everyone-is-in-real-time-aware-of-the-public-lavatory-and-miscellany-online-shopping-buns-fling-however-non-everyone-understands-how-to-scram-the-outflank-deals-on-trade-and-trans
)[https://bro-rankin.technetbloggers.de/about-everyone-is-in-real-time-aware-of-the-public-lavatory-and-miscellany-online-shopping-buns-fling-however-non-everyone-understands-how-to-scram-the-outflank-deals-on-trade-and-trans
]<br />(https://marshallmelvin13.bloggersdelight.dk/2025/02/11/be-safe-and-secure-when-you-shop-online/
)[https://marshallmelvin13.bloggersdelight.dk/2025/02/11/be-safe-and-secure-when-you-shop-online/
]<br />(https://diigo.com/0ytjyo
)[https://diigo.com/0ytjyo
]<br />(https://www.instapaper.com/p/15856375
)[https://www.instapaper.com/p/15856375
]<br />(https://wikimapia.org/external_link?url=https://fegge.com
)[https://wikimapia.org/external_link?url=https://fegge.com
]<br />(http://ezproxy.cityu.edu.hk/login?url=https://fegge.com
)[http://ezproxy.cityu.edu.hk/login?url=https://fegge.com
]<br />(https://bbs.pku.edu.cn/v2/jump-to.php?url=https://fegge.com
)[https://bbs.pku.edu.cn/v2/jump-to.php?url=https://fegge.com
]<br />(https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470757
)[https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470757
]<br />(https://www.longisland.com/profile/marshallhouston96
)[https://www.longisland.com/profile/marshallhouston96
]<br />(https://doodleordie.com/profile/grossmanandersson12
)[https://doodleordie.com/profile/grossmanandersson12
]<br />(http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897504
)[http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897504
]<br />(https://intensedebate.com/people/tarpmelvin21
)[https://intensedebate.com/people/tarpmelvin21
]<br />(https://community.umidigi.com/home.php?mod=space&uid=1456937
)[https://community.umidigi.com/home.php?mod=space&uid=1456937
]<br />(https://giles-bengtson-2.blogbright.net/online-shopping-the-tips-and-tricks-you-need-1739307098
)[https://giles-bengtson-2.blogbright.net/online-shopping-the-tips-and-tricks-you-need-1739307098
]<br />(https://giles-bengtson-3.technetbloggers.de/make-your-next-online-shopping-experience-fun-1739307098
)[https://giles-bengtson-3.technetbloggers.de/make-your-next-online-shopping-experience-fun-1739307098
]<br />(https://mcneilreddy85.bloggersdelight.dk/2025/02/11/shopping-online-butt-subscribe-a-great-deal-of-the-accent-that-traditional-shopping-could-crusade-stunned-of-the-project-you-wish-no-yearner-feature-to-postponement-in-yearn-lines-or-enquire-approxim/
)[https://mcneilreddy85.bloggersdelight.dk/2025/02/11/shopping-online-butt-subscribe-a-great-deal-of-the-accent-that-traditional-shopping-could-crusade-stunned-of-the-project-you-wish-no-yearner-feature-to-postponement-in-yearn-lines-or-enquire-approxim/
]<br />(https://www.instapaper.com/p/15856314
)[https://www.instapaper.com/p/15856314
]<br />(https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470565
)[https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470565
]<br />(https://www.longisland.com/profile/shepherdlara63
)[https://www.longisland.com/profile/shepherdlara63
]<br />(https://doodleordie.com/profile/shepherdlara51
)[https://doodleordie.com/profile/shepherdlara51
]<br />(https://intensedebate.com/people/reddyreddy35
)[https://intensedebate.com/people/reddyreddy35
]<br />(http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897412
)[http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897412
]<br />(https://olesen-soto.blogbright.net/in-that-location-is-a-large-consecrate-group-of-online-shoppers-extinct-there-and-for-soundly-reason-out-in-many-cases-you-plainly-cannot-shell-the-price-and-public-lavatory-of-shopping
)[https://olesen-soto.blogbright.net/in-that-location-is-a-large-consecrate-group-of-online-shoppers-extinct-there-and-for-soundly-reason-out-in-many-cases-you-plainly-cannot-shell-the-price-and-public-lavatory-of-shopping
]<br />(https://spivey-farley-3.technetbloggers.de/get-the-most-out-of-online-shopping-with-these-tips-1739305011
)[https://spivey-farley-3.technetbloggers.de/get-the-most-out-of-online-shopping-with-these-tips-1739305011
]<br />(https://cappslanghoff9.bloggersdelight.dk/2025/02/11/dont-worry-online-shopping-is-easier-than-you-think/
)[https://cappslanghoff9.bloggersdelight.dk/2025/02/11/dont-worry-online-shopping-is-easier-than-you-think/
]<br />(https://www.instapaper.com/p/15856270
)[https://www.instapaper.com/p/15856270
]<br />(https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470318
)[https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470318
]<br />(https://www.longisland.com/profile/conradsenbekker0
)[https://www.longisland.com/profile/conradsenbekker0
]<br />(https://doodleordie.com/profile/pachecobehrens0
)[https://doodleordie.com/profile/pachecobehrens0
]<br />(https://escatter11.fullerton.edu/nfs/show_user.php?userid=7974217
)[https://escatter11.fullerton.edu/nfs/show_user.php?userid=7974217
]<br />(https://intensedebate.com/people/weinerviborg0
)[https://intensedebate.com/people/weinerviborg0
]<br />(http://www.stes.tyc.edu.tw/xoops/
)[http://www.stes.tyc.edu.tw/xoops/
]<br />(https://olesen-soto.blogbright.net/i-wish-to-carry-through-money-when-i-patronise-online-simply-i-dont-have-intercourse-how-3f-you-are-not-alone-my-friend-as-nearly-the-great-unwashed-who-grease-ones-palms-on-the-cybersp
)[https://olesen-soto.blogbright.net/i-wish-to-carry-through-money-when-i-patronise-online-simply-i-dont-have-intercourse-how-3f-you-are-not-alone-my-friend-as-nearly-the-great-unwashed-who-grease-ones-palms-on-the-cybersp
]<br />(https://spivey-farley-3.technetbloggers.de/top-advice-for-shopping-on-the-internet-1739305012
)[https://spivey-farley-3.technetbloggers.de/top-advice-for-shopping-on-the-internet-1739305012
]<br />(https://cappslanghoff9.bloggersdelight.dk/2025/02/11/the-best-tips-for-shopping-over-the-internet/
)[https://cappslanghoff9.bloggersdelight.dk/2025/02/11/the-best-tips-for-shopping-over-the-internet/
]<br />(https://spivey-farley-3.technetbloggers.de/the-best-guide-to-read-when-learning-about-shopping-online-1739305012
)[https://spivey-farley-3.technetbloggers.de/the-best-guide-to-read-when-learning-about-shopping-online-1739305012
]<br />(https://olesen-soto.blogbright.net/it-doesnt-weigh-what-you-are-looking-for-to-buy-you-send-away-happen-it-online-online-shopping-allows-you-to-shop-on-handsome-corner-retailers-brand-websites-and-online-auctions-secondh
)[https://olesen-soto.blogbright.net/it-doesnt-weigh-what-you-are-looking-for-to-buy-you-send-away-happen-it-online-online-shopping-allows-you-to-shop-on-handsome-corner-retailers-brand-websites-and-online-auctions-secondh
]<br />(https://cappslanghoff9.bloggersdelight.dk/2025/02/11/top-advice-for-shopping-on-the-internet/
)[https://cappslanghoff9.bloggersdelight.dk/2025/02/11/top-advice-for-shopping-on-the-internet/
]<br />(https://macias-drachmann-3.technetbloggers.de/do-you-ilk-to-sponsor-3f-well-who-doesnt-shopping-is-a-pasttime-that-almost-hoi-polloi-alike-the-internet-has-made-it-much-easier-for-you-there-is-no-terminate-to-the-things-you-commode
)[https://macias-drachmann-3.technetbloggers.de/do-you-ilk-to-sponsor-3f-well-who-doesnt-shopping-is-a-pasttime-that-almost-hoi-polloi-alike-the-internet-has-made-it-much-easier-for-you-there-is-no-terminate-to-the-things-you-commode
]<br />(https://holdt-drachmann-2.blogbright.net/nearly-everyone-is-straightaway-aware-of-the-contraption-and-kind-online-shopping-prat-whirl-however-non-everyone-understands-how-to-mother-the-better-deals-on-trade-and-merchant-marine
)[https://holdt-drachmann-2.blogbright.net/nearly-everyone-is-straightaway-aware-of-the-contraption-and-kind-online-shopping-prat-whirl-however-non-everyone-understands-how-to-mother-the-better-deals-on-trade-and-merchant-marine
]<br />(https://www.instapaper.com/p/15856206
)[https://www.instapaper.com/p/15856206
]<br />(https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470098
)[https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10470098
]<br />(https://www.longisland.com/profile/jamison42vogel
)[https://www.longisland.com/profile/jamison42vogel
]<br />(https://doodleordie.com/profile/hammer54ashby
)[https://doodleordie.com/profile/hammer54ashby
]<br />(https://numberfields.asu.edu/NumberFields/show_user.php?userid=5106343
)[https://numberfields.asu.edu/NumberFields/show_user.php?userid=5106343
]<br />(http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897227
)[http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897227
]<br />(https://intensedebate.com/people/hatfield95ashby
)[https://intensedebate.com/people/hatfield95ashby
]<br />(https://hutchinson-dreier.technetbloggers.de/with-todays-dull-economy-its-c-h-best-to-save-money-any-elbow-room-you-commode-still-you-dont-make-to-finish-entirely-retail-activity-flush-if-you-are-observance-your-budget-you-termina
)[https://hutchinson-dreier.technetbloggers.de/with-todays-dull-economy-its-c-h-best-to-save-money-any-elbow-room-you-commode-still-you-dont-make-to-finish-entirely-retail-activity-flush-if-you-are-observance-your-budget-you-termina
]<br />(https://ray-stephens-3.blogbright.net/when-it-comes-to-online-shopping-how-toilet-i-relieve-money-3f-i-like-the-appliance-of-having-my-purchases-shipped-to-my-door-but-i-dont-need-to-earnings-through-the-pry-for-the-religio
)[https://ray-stephens-3.blogbright.net/when-it-comes-to-online-shopping-how-toilet-i-relieve-money-3f-i-like-the-appliance-of-having-my-purchases-shipped-to-my-door-but-i-dont-need-to-earnings-through-the-pry-for-the-religio
]<br />(https://diigo.com/0ytjeg
)[https://diigo.com/0ytjeg
]<br />(https://www.instapaper.com/p/15856155
)[https://www.instapaper.com/p/15856155
]<br />(https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10469867
)[https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10469867
]<br />(https://www.longisland.com/profile/davidsen36madden
)[https://www.longisland.com/profile/davidsen36madden
]<br />(https://doodleordie.com/profile/snider03sharp
)[https://doodleordie.com/profile/snider03sharp
]<br />(https://escatter11.fullerton.edu/nfs/show_user.php?userid=7973836
)[https://escatter11.fullerton.edu/nfs/show_user.php?userid=7973836
]<br />(http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897074
)[http://www.stes.tyc.edu.tw/xoops/modules/profile/userinfo.php?uid=2897074
]<br />(https://valentin-begum-5.technetbloggers.de/check-out-this-article-on-online-shopping-that-offers-many-great-tips-1739299516
)[https://valentin-begum-5.technetbloggers.de/check-out-this-article-on-online-shopping-that-offers-many-great-tips-1739299516
]<br />(https://valentin-begum-3.blogbright.net/dont-waste-your-money-take-our-online-shopping-advice-1739299517
)[https://valentin-begum-3.blogbright.net/dont-waste-your-money-take-our-online-shopping-advice-1739299517
]<br />(https://www.instapaper.com/p/15856059
)[https://www.instapaper.com/p/15856059
]<br />(https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10469656
)[https://vuf.minagricultura.gov.co/Lists/Informacin%20Servicios%20Web/DispForm.aspx?ID=10469656
]<br />(https://doodleordie.com/profile/drewthestrup1
)[https://doodleordie.com/profile/drewthestrup1
]<br />(https://www.longisland.com/profile/wilkersondrew4
)[https://www.longisland.com/profile/wilkersondrew4
]<br />(https://escatter11.fullerton.edu/nfs/show_user.php?userid=7973565
)[https://escatter11.fullerton.edu/nfs/show_user.php?userid=7973565
]<br />https://numberfields.asu.edu/NumberFields/show_user.php?userid=5106137
