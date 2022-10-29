# Film_Tavsiye_Algoritmasi
TF-IDF ve Cosine Similarity kullanarak seçilen bir filme göre benzer filmlerin bulunması sağlanıyor.

#GELİŞTİRMELER

```
İlk iyileştirme olarak, datasetimizdeki filmlerin clean kısmındaki kelimelerimizin büyük ve küçük harfler içerdiğini tespit ettim. Bu durum algoritmada 2 kelime aynı kelime olsa bile birisi büyük harfle başladığı için farklılığa sebep olabileceğinden dolayı bütün kelimeleri küçük harfe çevirerek daha doğru bir sonuç elde etmeye çalıştım.
```
<br/>
```
1 ve 2 harfli kelimeleri stopwords saydığım için bu kelimeleri yok ettim. Kelimeleri yok etmemin sebebi 1 veya 2 harfli olan kelimelerin çok fazla ortak filmde çıkması ve bu kelimelerin ayırt edici bir özelliği olmadığıydı. Bu kelimeleri elekten geçirerek kelime havuzunu daha anlamlı hale getirmeye çalıştım.
```
<br/>
```
Kod içerisinde doc frekanslarını sıralayarak incelediğimde bazı kelimelerin anlam ifade etmemesine rağmen çok fazla frekansa sahip olduğunu gördüm. Bu yüzden bir frekans eşiği koymaya karar verdim. Çok sayıda filmde ortak geçen kelimeler istediğimiz sonucu almamızda yanıltıcı oluyorlardı. En çok frekansa sahip olan kelimelerden bazıları şunlardı = film, olmak, ada, bir, ama, etmek, son, yaş, mak, mek, ken .Bu gibi kelimeler çok sayıda filmde geçiyor ve anlamsızlar. Verceğim eşik değerini belirlemek için her filmin clean datasına dahil ettiğimiz türlerin ne kadar frekansa sahip olduklarına bakarak karar verdim. Bakmış olduğum sonuçlara göre 5261 filmin bulunduğu datasetindeki tür frekansları büyükten küçüğe şu şekildeydi  "dram" -> 2353 , "Komedi" -> 1471,   Aksiyon -> 980 ve Macera -> 976 şeklindeydi. Bu sonuçları göz önünde bulundururak film sayısının yarısını eşik değeri olarak belirledim. Frekensı film sayısının yarısından büyük olan kelimeleri elekten geçirdim ve kelimelerin doc frekanslarını 0 a eşitledim.
```
<br/>
```
Clean verilerimiz içerisine filmin adını ve yönetmenlerini de eklemenin benzer filmler çıkmasına katkıda bulunacağını düşünerek bu verileri de clean dataya eklemeye karar verdim. Ancak yönetmen isimlerinin ve soyisimlerinin bir kalıp olarak alınmasının daha doğru sonuçlar çıkaracağını düşündüm çünkü iki filmin yönetmeninin ismi veya soyismi aynı olursa bu filmlere algoritma benzer diyebilirdi. Bu yüzden yönetmenlerin ismini birleştirerek clean dataya eklemeyi tercih ettim. Örnek= Yönetmen ->joe russo, clean dataya eklendiği şekil ->joerusso . 
```
<br/>
```
Eski algoritmayı incelediğimde tf değeri için sadece kelimenin dökümanda geçme sayısı alınmış fakat yaptığım araştırmadan tf değerinin, kelimenin dökümanda geçme sayısı / dökümandaki toplam kelime sayısı formülüyle hesaplandığını gördüm. Bu yüzden for döngüsü kullanarak her bir filmin clean datasının içerisinde kaç kelime olduğunu hesapladım ve bunu bir liste olarak tuttum. Daha sonra tf i hesaplarken her filmin clean datasındaki toplam kelime sayısını bölen olarak kullandım. Tf i bu şekilde hesaplamazsak içerisinde fazla kelime olan clean daha büyük ihtimalle daha fazla önerilecektir. Fakat tf i hesaplarken o filmin clean datasındaki kelime sayısına bölersek daha adaletli bir tf değeri oluşturmuş olduğumuzu düşünüyorum.
```
<br/>
```
İdf hesabının yapıldığı kısımda eklenmiş olan (+1) değerlerinin idf-tf değerini olumsuz etkilediğini düşündüm. Çünkü doc frekansı düşük olan veya 0 olan bir kelime geldiğinde biz bu kelimeye +1 ekleyerek idf değerini yükseltmiş oluyoruz. +1 değerlerini kaldırdığımda 0 gelen frekanslar olduğu için ZeroDivisionError hatası aldım. Bu yüzden bir try catch yapısı kullanarak catch kısmına düşüldüğünde idf değerini 0 a eşitleyerek bu sorunu giderdim.
```
<br/>
##ÖZET
<br/>
**Bu geliştirmeleri yaptıktan sonra eski ve yeni algoritmaya aynı filmleri göndererek iki algoritma arasındaki farkı ölçmeye çalıştım. Yapmış olduğum testlerde yeni algoritmanın eski algoritmaya göre daha iyi sonuçlar verdiğini gözlemledim. Geliştirilmiş algoritmanın kötü yanıysa eski algoritmaya göre biraz daha yavaş çalışmasıydı. Bunun en büyük sebebi de iç içe for döngüsü kullanarak her filmdeki kelimeleri saydırmamdı. **

