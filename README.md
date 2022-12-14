# Film_Tavsiye_Algoritmasi

<br/>

TF-IDF ve Cosine Similarity kullanarak seçilen bir filme göre benzer filmlerin bulunması sağlanıyor.

<br/>

# GELİŞTİRMELER

<br/>

```
İlk iyileştirme olarak, datasetimizdeki filmlerin clean kısmındaki kelimelerimizin büyük ve küçük 
harfler içerdiğini tespit ettim. Bu durum algoritmada 2 kelime aynı kelime olsa bile birisi büyük 
harfle başladığı için farklılığa sebep olabileceğinden dolayı bütün kelimeleri küçük harfe 
çevirerek daha doğru bir sonuç elde etmeye çalıştım.
```

<br/>

```
1 ve 2 harfli kelimeleri stopwords saydığım için bu kelimeleri yok ettim. Kelimeleri yok etmemin 
sebebi 1 veya 2 harfli olan kelimelerin çok fazla ortak filmde çıkması ve bu kelimelerin ayırt 
edici bir özelliği olmadığıydı. Bu kelimeleri elekten geçirerek kelime havuzunu daha anlamlı 
hale getirmeye çalıştım.
```

<br/>

```
Kod içerisinde doc frekanslarını sıralayarak incelediğimde bazı kelimelerin anlam ifade 
etmemesine rağmen çok fazla frekansa sahip olduğunu gördüm. Bu yüzden bir frekans eşiği 
koymaya karar verdim. Çok sayıda filmde ortak geçen kelimeler istediğimiz sonucu almamızda 
yanıltıcı oluyorlardı. En çok frekansa sahip olan kelimelerden bazıları şunlardı = film, 
olmak, ada, bir, ama, etmek, son, yaş, mak, mek, ken.Bu gibi kelimeler çok sayıda filmde 
geçiyor ve anlamsızlar. Verceğim eşik değerini belirlemek için her filmin clean datasına 
dahil ettiğimiz türlerin ne kadar frekansa sahip olduklarına bakarak karar verdim. Bakmış 
olduğum sonuçlara göre 5261 filmin bulunduğu datasetindeki tür frekansları büyükten küçüğe 
şu şekildeydi  "dram" -> 2353 , "Komedi" -> 1471,   Aksiyon -> 980 ve Macera -> 976 
şeklindeydi. Bu sonuçları göz önünde bulundururak film sayısının yarısını eşik değeri 
olarak belirledim. Frekensı film sayısının yarısından büyük olan kelimeleri elekten 
geçirdim ve kelimelerin doc frekanslarını 0 a eşitledim.
```

<br/>

```
Clean verilerimiz içerisine filmin adını ve yönetmenlerini de eklemenin benzer filmler 
çıkmasına katkıda bulunacağını düşünerek bu verileri de clean dataya eklemeye karar verdim.
Ancak yönetmen isimlerinin ve soyisimlerinin bir kalıp olarak alınmasının daha doğru 
sonuçlar çıkaracağını düşündüm çünkü iki filmin yönetmeninin ismi veya soyismi aynı olursa 
bu filmlere algoritma benzer diyebilirdi. Bu yüzden yönetmenlerin ismini birleştirerek 
clean dataya eklemeyi tercih ettim.
Örnek= Yönetmen ->joe russo, clean dataya eklendiği şekil ->joerusso . 
```

<br/>

```
Eski algoritmayı incelediğimde tf değeri için sadece kelimenin dökümanda geçme sayısı 
alınmış fakat yaptığım araştırmadan tf değerinin,
kelimenin dökümanda geçme sayısı / dökümandaki toplam kelime sayısı formülüyle 
hesaplandığını gördüm. Bu yüzden for döngüsü kullanarak her bir filmin clean datasının 
içerisinde kaç kelime olduğunu hesapladım ve bunu bir liste olarak tuttum.Daha sonra 
tf i hesaplarken her filmin clean datasındaki toplam kelime sayısını bölen olarak 
kullandım. Tf i bu şekilde hesaplamazsak içerisinde fazla kelime olan clean daha büyük 
ihtimalle daha fazla önerilecektir. Fakat tf i hesaplarken o filmin clean datasındaki 
kelime sayısına bölersek daha adaletli bir tf değeri oluşturmuş olduğumuzu düşünüyorum.
```

<br/>

```
İdf hesabının yapıldığı kısımda eklenmiş olan (+1) değerlerinin idf-tf değerini olumsuz 
etkilediğini düşündüm. Çünkü doc frekansı düşük olan veya 0 olan bir kelime geldiğinde 
biz bu kelimeye +1 ekleyerek idf değerini yükseltmiş oluyoruz. +1 değerlerini kaldırdığımda 
0 gelen frekanslar olduğu için ZeroDivisionError hatası aldım. Bu yüzden bir 
try catch yapısı kullanarak catch kısmına düşüldüğünde idf değerini 0 a eşitleyerek 
bu sorunu giderdim.
```
<br/>

## ÖZET

<br/>

**Bu geliştirmeleri yaptıktan sonra eski ve yeni algoritmaya aynı filmleri göndererek iki algoritma arasındaki farkı ölçmeye çalıştım. Yapmış olduğum testlerde yeni algoritmanın eski algoritmaya göre daha iyi sonuçlar verdiğini gözlemledim. Geliştirilmiş algoritmanın kötü yanıysa eski algoritmaya göre biraz daha yavaş çalışmasıydı. Bunun en büyük sebebi de iç içe for döngüsü kullanarak her filmdeki kelimeleri saydırmamdı.**

<br/>

## TEST 1

<br/>

Ali Kundilli filmi için gelişmiş ve eski algoritma arasındaki farka baktığımızda gelişmiş algoritmadaki 4. ve 5. filmin  , eski algoritmadaki 4. ve 5. filme göre daha iyi bir öneri olduğunu söyleyebiliriz. Eski algoritmada 4. 5. film için yapılan öneriler "Dram" türündeyken, gelişmiş algoritmada Ali Kundilli gibi "Komedi"  türünde filmler önerilmiştir.

<br/>

### Eski Algoritma

<br/>

![eski_1](https://user-images.githubusercontent.com/77435563/198852219-e7f83514-7259-4ee9-bff5-683ffd3b1ed1.jpg)

<br/>

### Geliştirilmiş Algoritma

<br/>

![gelistirilmis_1](https://user-images.githubusercontent.com/77435563/198852266-b3fb2476-f380-405b-8a41-df0e828d6fbf.jpg)

<br/>

## TEST 2

<br/>

 7. Koğuştaki Mucize filmi için gelişmiş ve eski algoritma arasındaki farka baktığımızda gelişmiş algoritmadaki 3, 4 ve  5. filmin, eski algoritmadaki 3, 4 ve 5. filme göre daha iyi öneri olduğunu söyleyebiliriz. 7. Koğuştaki Mucize filminin kavuşmak ile alakalı olduğunu düşünürsek yeni algoritmadaki sonuçların kavuşmak temasına daha yakın olduğu görülüyor.

<br/>

### Eski Algoritma

<br/>

![eski_2](https://user-images.githubusercontent.com/77435563/198852364-aa297953-99dc-4a6a-99b8-18dd5402154b.jpg)

<br/>

### Geliştirilmiş Algoritma

<br/>

![gelistirilmiş_2](https://user-images.githubusercontent.com/77435563/198852380-4f8a680b-55df-48b1-a561-40cad0e8d631.jpg)

<br/>




## TEST3

<br/>

Hızlı ve Öfkeli filmi için gelişmiş ve eski algoritma arasındaki farka baktığımızda gelişmiş algoritmadaki bütün sonuçların aynı filmin diğer serileri olduğu gözükürken, Eski algoritma bu filmin serilerinin tamamını bulamamıştır.

<br/>

### Eski Algoritma

<br/>

![eski_3](https://user-images.githubusercontent.com/77435563/198852391-60043091-d860-4442-8179-7cecc86a9ddd.jpg)

<br/>

### Geliştirilmiş Algoritma

<br/>

![gelistirilmiş_3](https://user-images.githubusercontent.com/77435563/198852403-5e034f88-b282-4b3e-8d95-9d07b1ec5203.jpg)

<br/>


