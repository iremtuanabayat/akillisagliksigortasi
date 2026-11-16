# akillisagliksigortasi
SAĞLIK MALİYETİ SİGARA KULLANIMI VE IBM
Bu projede bireylerin demografik özellikleri ve yaşam tarzı göstergelerinden yararlanarak yıllık sağlık sigortası masraflarını tahmin eden bir makine öğrenmesi çalışması gerçekleştirilmiştir. Veri setinde yaş, cinsiyet, vücut kitle indeksi (BMI), çocuk sayısı, sigara kullanımı ve yaşanılan bölge değişkenleri girdi; yıllık sağlık masrafı (charges) ise hedef değişken olarak ele alınmıştır.
Çalışmada önce veri seti incelenmiş, eksik değer ve tekrar eden kayıt kontrolleri yapılmış, bir adet yinelenen satır çıkarılmıştır. Keşifsel veri analiziyle değişkenlerin sağlık masrafıyla ilişkileri değerlendirilmiş; özellikle sigara kullanımı ile BMI kategorisinin birlikte ele alındığında masraflar arasında belirgin farklar görüldüğü gözlenmiştir. Ana tahmin görevi için Linear Regression modeli kurulmuş ve test verisinde R² = 0,8069, MAE = 4.177,05 $, RMSE = 5.956,34 $ ve MAPE = %41,40 sonuçları elde edilmiştir. Ek olarak yıllık masrafın 15.000 $ eşiğini aşıp aşmadığını belirlemek üzere max_depth=4 parametreli Decision Tree Classifier eğitilmiş; bu model test kümesinde %94,40 doğruluk ve %88,89 F1 skoru üretmiştir.
PROJENİN AMACI VE KAPSAMI
Projenin temel amacı, bireyin yaş, cinsiyet, BMI, çocuk sayısı, sigara kullanımı ve bölge bilgilerini kullanarak yıllık sağlık sigortası masrafı için tahmini bir dolar tutarı üretmektir.
KULLANILAN VERİ SETİ
Notebook içerisinde veri seti, Stedy tarafından yayımlanan Machine Learning with R datasets deposundaki insurance.csv dosyasından alınmıştır. Veri, Pandas kullanılarak okunmuş ve çalışma dizinine saglik_sigortasi.csv adıyla kaydedilmiştir.
İlk yüklemede veri seti 1.338 satır ve 7 sütundan oluşmaktadır. Yinelenen kayıt kontrolünde aynı özellik ve hedef değerlerine sahip bir çift kayıt tespit edilmiş; bu kayıtlardan biri çıkarılarak analizlerde 1.337 satır kullanılmıştır.
Özet bilgi	Değer
Temizleme sonrası gözlem sayısı	1.337
Değişken sayısı	7
Yaş aralığı	18–64
BMI aralığı	15,96–53,13
Çocuk sayısı aralığı	0–5
Yıllık masraf aralığı	1.121,87 $–63.770,43 $
Yıllık masraf ortalaması	13.279,12 $

Veri setinde eksik değer bulunmamıştır. Yaş, BMI ve charges sütunlarında sıfır veya negatif değer görülmemiştir. children sütunundaki 574 sıfır değeri, bireyin bakmakla yükümlü olduğu çocuk sayısının sıfır olması anlamına geldiğinden geçerli gözlem olarak korunmuştur. Kategorik sütunlarda cinsiyet için female/male, sigara için yes/no ve bölge için dört kategori gözlenmiştir.
KULLANILAN TEKNOLOJİLER
•	Python: Veri işleme ve makine öğrenmesi uygulamalarının geliştirilmesi.
•	Pandas: CSV dosyasının okunması, veri temizleme, gruplama ve tablo işlemleri.
•	NumPy: Sayısal işlemler ve RMSE hesabı.
•	Matplotlib: Çubuk grafikler ve saçılım grafikleri.
•	Scikit-learn: Eğitim-test ayrımı, doğrusal regresyon, karar ağacı ve performans metrikleri.
•	Google Colab: Notebook ortamında kodların çalıştırılması.
VERİ ÖN İŞLEME VE KEŞİFSEL VERİ ANALİZİ
5.1. Veri temizleme
Sütun veri tipleri kontrol edilmiştir. age ve children tam sayı; bmi ve charges ondalıklı sayısal; sex, smoker ve region ise kategorik/metin türündedir. Eksik değer sayıları tüm sütunlarda sıfırdır. Yinelenen kayıt sayısı bir olarak belirlenmiş ve bu kayıt çıkarılmıştır. Temizleme sonrasında veri çerçevesi 1.337 satır ve 7 sütun olarak kalmıştır.
5.2. Tanımlayıcı istatistikler
Temizlenmiş veri setinde yaş ortalaması 39,22, BMI ortalaması 30,66, çocuk sayısı ortalaması 1,10 ve yıllık sağlık masrafı ortalaması 13.279,12 $ olarak hesaplanmıştır. Yıllık masrafın standart sapması 12.110,36 $ olduğundan masrafların veri seti içinde geniş bir dağılım gösterdiği görülmektedir.
5.3. Kategorik ve gruplu analizler
İncelenen grup	Gözlem sayısı	Ortalama yıllık masraf
Sigara kullanmayan	1.063	8.440,66 $
Sigara kullanan	274	32.050,23 $
Kadın	662	12.569,58 $
Erkek	675	13.975,00 $

Sigara kullanan grubun ortalama yıllık masrafı, sigara kullanmayan grubun ortalamasından belirgin biçimde yüksektir. Bu veri setinde sigara kullananların ortalama masrafı yaklaşık 3,8 kat seviyesindedir. Bu sonuç bir ilişkiyi gösterir; tek başına nedensellik kanıtı olarak yorumlanmamalıdır.
BMI kategorisi	Kayıt sayısı	Ortalama masraf
Zayıf (<18,5)	21	8.657,62 $
Normal (18,5–24,9)	221	10.404,90 $
Fazla kilolu (25–29,9)	380	11.006,81 $
Obez (30+)	715	15.510,92 $

Yaş grupları incelendiğinde 18–29 yaş grubunda ortalama masraf 9.200,62 $, 30–44 yaş grubunda 12.490,91 $ ve 45–64 yaş grubunda 17.070,49 $ olarak hesaplanmıştır. Bu veri setinde yaş grubu yükseldikçe ortalama masrafın da yükseldiği gözlenmiştir.
5.4. BMI ve sigara kullanımının birlikte incelenmesi
BMI kategorisi	Sigara kullanmayan	Sigara kullanan
Zayıf	5.485,06 $	18.809,82 $
Normal	7.616,21 $	19.942,22 $
Fazla kilolu	8.348,37 $	22.379,03 $
Obez	8.822,17 $	41.355,87 $

Çapraz analizde özellikle obez kategorisinde sigara kullanan grubun ortalama masrafı 41.355,87 $, sigara kullanmayan grubun ortalama masrafı ise 8.822,17 $ olarak bulunmuştur. Bu gözlem, BMI ve sigara kullanımının birlikte değerlendirilmesinin önemli olabileceğine işaret etmektedir. Bununla birlikte analiz, gözlemsel veri üzerinden yapılmıştır ve doğrudan nedensel etki göstermez.
5.5. Korelasyon incelemesi
Değişken	charges ile korelasyon
age	0,298
bmi	0,198
children	0,067

Sayısal değişkenler arasında charges ile en yüksek korelasyon age değişkeninde görülmüştür (0,298). BMI ile korelasyon 0,198; children ile korelasyon 0,067 olarak hesaplanmıştır. Korelasyon, değişkenler arasındaki doğrusal ilişkinin yönü ve gücü hakkında bilgi verir; tek başına neden-sonuç ilişkisi kurmaz.
 MODEL PERFORMANSI VE BULGULAR
7.1. Doğrusal regresyon sonuçları
Metrik	Sonuç	Yorum
Eğitim R²	%72,99	Eğitim verisindeki açıklanan varyans oranı
Test R²	%80,69	Test verisindeki açıklanan varyans oranı
MAE	4.177,05 $	Tahminlerin ortalama mutlak hata büyüklüğü
RMSE	5.956,34 $	Büyük hatalara daha fazla ağırlık veren hata ölçüsü
MAPE	%41,40	Ortalama mutlak yüzdesel hata

Test R² değerinin 0,8069 olması, modelin test kümesindeki charges değişkeni varyansının yaklaşık %80,69'unu açıkladığını gösterir. MAE, tahminlerin gerçek değerlerden ortalama mutlak 4.177,05 $ uzaklıkta olduğunu; RMSE ise büyük hatalara daha fazla ağırlık verildiğinde hata büyüklüğünün 5.956,34 $ olduğunu göstermektedir. MAPE %41,40 olarak hesaplanmıştır.
Eğitim R² değerinin %72,99, test R² değerinin %80,69 olması bu tek bölünmede test performansının daha yüksek olduğunu göstermektedir. Bu sonuç tek başına modelin kesin olarak aşırı öğrenme yapmadığını kanıtlamaz. Daha güvenilir genelleme değerlendirmesi için çapraz doğrulama ve farklı veri bölünmeleriyle tekrar test önerilir.
7.2. Yeni profil tahminleri
Profil	Yaş	BMI	Çocuk	Sigara	Tahmini yıllık masraf
Genç, normal kilolu	22	21,5	0	Hayır	1.220,06 $
Orta yaş, hafif kilolu	38	26,8	2	Hayır	7.453,27 $
Genç, normal kilolu	30	23,0	0	Evet	26.102,43 $
İleri yaş, obez	55	35,2	1	Evet	36.447,54 $

Model, veri setinde bulunmayan dört örnek profil için tahmin üretmiştir. Bu örneklerde sigara kullanımı bulunan profiller için daha yüksek tahminler elde edilmiştir. Bunlar model çıktılarıdır; gerçek kişilerin sağlık giderleri veya sigorta primleri olarak değerlendirilmemelidir.
7.3. Karar ağacı sınıflandırma sonuçları
Metrik	Test sonucu
Accuracy / Doğruluk	%94,40
Precision / Kesinlik	%96,77
Recall / Duyarlılık	%82,19
F1-score	%88,89

Sınıflandırma raporunda düşük risk sınıfı için precision 0,94, recall 0,99 ve F1 0,96; yüksek risk sınıfı için precision 0,97, recall 0,82 ve F1 0,89 olarak verilmiştir. Test kümesinde 195 düşük riskli ve 73 yüksek riskli gözlem bulunmaktadır. Yüksek risk sınıfındaki recall değerinin %82,19 olması, gerçek yüksek masraflı gözlemlerin bir bölümünün model tarafından kaçırıldığını gösterir.
Notebook'taki bir açıklama hücresinde sınıflandırma sonuçları için farklı metrik değerleri yazılmıştır. Bu raporda, aynı notebook'ta kod hücresinin doğrudan ürettiği güncel çıktı esas alınmış; doğruluk %94,40 ve F1 skoru %88,89 olarak raporlanmıştır.
SONUÇ VE DEĞERLENDİRME
Bu projede sağlık sigortası yıllık masraflarını tahmin etmek amacıyla veri analizi, veri temizleme, kategorik değişkenlerin sayısallaştırılması, regresyon ve sınıflandırma aşamaları gerçekleştirilmiştir. 1.337 gözlemden oluşan temizlenmiş veri setinde sigara kullanımı, yaş grubu ve BMI kategorileri bakımından masraf farklılıkları incelenmiştir. Özellikle sigara kullanan grupta ortalama masrafın daha yüksek olması ve BMI ile sigara kullanımının birlikte incelendiği analizde obez kategorisindeki farkın belirginleşmesi dikkat çekmiştir.
Ana LinearRegression modeli test verisinde %80,69 R² elde etmiştir. Bu sonuç, modelin veri setindeki masraf değişkenliğini önemli ölçüde açıklayabildiğini göstermektedir. Bununla birlikte MAE'nin 4.177,05 $, RMSE'nin 5.956,34 $ ve MAPE'nin %41,40 olması tahmin hatalarının hâlâ dikkate değer düzeyde olduğunu ortaya koymaktadır. Ek DecisionTreeClassifier modeli, 15.000 $ eşiğine göre sınıflandırmada %94,40 doğruluk ve %88,89 F1 skoru elde etmiştir; yüksek risk sınıfı recall değerinin %82,19 olması bu sınıfta iyileştirme alanı bulunduğunu göstermektedir.
Sonuç olarak çalışma, temel düzeyde bir sağlık sigortası maliyet tahmin ve risk sınıflandırma prototipi ortaya koymuştur. Model çıktıları veri setinin kapsamı ve kullanılan özelliklerle sınırlıdır; klinik karar, gerçek sigorta primi belirleme veya bireysel sağlık değerlendirmesi için tek başına kullanılmamalıdır.
SUNUM DOSYASI https://docs.google.com/presentation/d/14TllQCJmy9qcauPg6K3mnfivmCr5xa9U/edit?usp=drive_link&ouid=110090045431523821391&rtpof=true&sd=true
PYTHON DOSYASI 
https://colab.research.google.com/drive/15dzUci6rKdS59kV-P2OmlzpXr204KGVw?usp=sharing
