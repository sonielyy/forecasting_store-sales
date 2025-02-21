# Store Sales - Time Series Forecasting
### Makine öğrenmesi kullanarak satış tahmini


## Amaç

"Yarışmacılar, farklı mağazalarda satılan binlerce ürünün satış tahminlerini daha doğru bir şekilde yapmaya çalışacaklar. Modeliniz şu verileri içeren bir eğitim seti kullanarak eğitilecektir:

- Tarih bilgileri
- Mağaza ve ürün bilgileri
- Promosyonlar
- Gerçekleşen satışlar
Bu yarışma, perakende sektöründeki talep tahminini iyileştirmeye yönelik ve özellikle zaman serisi analizine giriş yapmak isteyenler için iyi bir fırsattır."

![image](https://github.com/user-attachments/assets/2741d035-6d44-4efd-967d-515faaa2976a)

Bu, özellikle süpermarketler için kritik bir konu çünkü:

Yanlış tahmin yapıldığında fazla ürün sipariş edilirse bozulmuş stoklar nedeniyle israf meydana gelir.
Yetersiz sipariş verilirse, popüler ürünler hızla tükenerek hem müşteri memnuniyetsizliğine hem de satış kayıplarına yol açar.
Mevcut geleneksel tahmin yöntemleri, genellikle veriye dayalı değildir ve otomatikleştirilmesi zordur. Bu tahmin sürecini geliştirmek, perakendecilere daha verimli stok yönetimi sağlayabilir.

## Veri Hakkında

1. train.csv (Eğitim Verisi)
Bu dosya, modelinizi eğitmek için kullanacağınız asıl zaman serisi verilerini içerir.

- date → Satışın gerçekleştiği tarih
- store_nbr → Ürünün satıldığı mağazayı tanımlayan numara
- family → Ürün kategorisini belirten etiket (örneğin, süt ürünleri, içecekler, temizlik ürünleri gibi)
- sales → O gün satılan toplam ürün miktarı (Hedef değişken, tahmin edilmesi gereken değer)
- onpromotion → O mağazada o gün promosyonda olan ürün sayısı

Önemli noktalar:
- sales (satışlar) sürekli bir değişkendir. Bazı ürünler kesirli birimlerde satılabildiği için (örneğin, 1.5 kg peynir) bu değişken tam sayı olmak zorunda değildir.
onpromotion değişkeni, promosyonların satışlar üzerindeki etkisini analiz etmede kritik bir rol oynayabilir.

2. test.csv (Test Verisi)
Bu dosyada eğitim verisindekiyle aynı özellikler bulunur, ancak sales sütunu yoktur. Modelinizin bu verilerdeki satışları tahmin etmesi beklenir.

Önemli noktalar:
- Test setindeki tarihler, eğitim setinin son tarihinden sonraki 15 günü kapsar.
- Modeliniz, önceki verileri kullanarak gelecekteki 15 günün satışlarını tahmin etmelidir.

3. sample_submission.csv (Örnek Gönderim Dosyası)
Bu dosya, Kaggle’a gönderilecek tahmin dosyanızın doğru formatta olup olmadığını görmek için bir örnektir.
Burada id, test setindeki her satıra karşılık gelen benzersiz bir kimliktir. sales sütunu ise modelinizin tahmin ettiği satış değerlerini içermelidir.

4. stores.csv (Mağaza Bilgileri)
Bu dosya, her mağaza hakkında ek bilgiler içerir:

- store_nbr → Mağaza kimliği (train.csv ve test.csv ile eşleşir)
- city → Mağazanın bulunduğu şehir
- state → Mağazanın bulunduğu eyalet/bölge
- type → Mağaza tipi (büyük, küçük, hipermarket vs.)
- cluster → Benzer özelliklere sahip mağazalar gruplara ayrılmıştır
Mağazaların bulundukları şehir, eyalet ve tür gibi bilgileri kullanarak bölgesel ve yapısal etkileri analiz etmek mümkün olabilir.

5. oil.csv (Günlük Petrol Fiyatları)
Bu dosya, Ekvador’un ekonomi açısından petrol fiyatlarına bağımlı olması nedeniyle önemlidir.
- date → Tarih
- dcoilwtico → Günlük petrol fiyatı (WTI Crude Oil)

Önemli noktalar:
- Petrol fiyatlarındaki dalgalanmalar, ülkenin ekonomik durumunu etkileyerek insanların harcama alışkanlıklarını değiştirebilir.
Özellikle ekonomik kriz veya petrol fiyatı düşüşleri market satışlarını etkileyebilir.

6. holidays_events.csv (Resmi Tatiller ve Özel Günler)
Bu dosya, belirli günlerin satışlar üzerindeki etkisini analiz etmek için önemli bilgiler içerir.

- date → Tarih
- type → Tatil veya etkinlik türü (Holiday, Event, Transfer, Bridge, Work Day, Additional)
- locale → Tatilin geçerli olduğu yer (National = ülke genelinde, Regional = belirli bir bölgede, Local = belirli bir şehirde)
- locale_name → Tatilin/etkinliğin gerçekleştiği bölge veya şehir adı
- description → Tatilin veya etkinliğin adı
- transferred → Eğer tatil başka bir güne ertelendi ise burada True yazar

Önemli noktalar:
- Resmi tatiller, süpermarket satışlarını artırabilir veya azaltabilir. Örneğin, Noel ve Yılbaşı öncesi günlerde satışlar zirve yaparken, bazı resmi tatillerde marketler kapalı olabilir.
Transfer edilmiş tatiller, asıl tatilin farklı bir güne kaydırıldığını gösterir. Bu yüzden tatilin gerçekten hangi gün kutlandığını doğru analiz etmek gerekir.
Bridge (Köprü Günleri), tatili hafta sonuna bağlayan ekstra tatil günleridir ve alışveriş davranışlarını değiştirebilir.

Ek Notlar
Maaş Günleri:
- Ekvador’da devlet memurlarının maaşları her ayın 15’inde ve son gününde ödeniyor.
- İnsanlar maaş aldıklarında daha fazla harcama yapma eğiliminde olabilir. Bu günlerde satışlarda artış beklenebilir.

Deprem Etkisi (16 Nisan 2016 - Büyüklük 7.8)
- Deprem, birkaç hafta boyunca süpermarket satışlarını büyük ölçüde etkilemiş olabilir.
- İnsanlar su, konserve yiyecek ve temel ihtiyaç maddelerine yöneldiğinden, bazı ürün kategorilerinde olağan dışı satış artışları olabilir.

Model geliştirirken bu anomalileri dikkate almak önemli olabilir.

## Tools


## Data Preparation

[Veri Kaynağı](https://us.shein.com/recommend/Women-New-in-sc-100161222.html?adp=35242185&categoryJump=true&ici=us_tab03navbar03menu01dir02&src_identifier=fc%3DWomen%20Clothing%60sc%3DWomen%20Clothing%60tc%3DShop%20by%20category%60oc%3DNew%20in%60ps%3Dtab03navbar03menu01dir02%60jc%3DitemPicking_100161222&src_module=topcat&src_tab_page_id=page_home1718006855109)

```Python
service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)
...
# Item Elements
name_elements = driver.find_elements(By.CLASS_NAME, 'product-card__goods-title-container')
sleep(2)

# Price Elements
price_elements = driver.find_elements(By.CLASS_NAME, 'product-card__prices-info')
sleep(2)
```

Ön işlemenin sonunda 7 sütun ve 113 satırdan oluşan temiz bir tablo oluşturuldu. Bu tablo xlsx formatında Python ortamından dışarı aktarıldı.

![image](https://github.com/sonielyy/shein_scraping_project/assets/71605453/0e0be087-22fb-45be-8ddd-b12378e15b25)

## Data Visualization

### General Products

![image](https://github.com/sonielyy/shein_scraping_project/assets/71605453/82fa5b82-bf0c-45a3-872b-6e3fc874026c)

113 ürün ilanını incelediğimizde:

### Product Prices

![image](https://github.com/sonielyy/shein_scraping_project/assets/71605453/8eb12862-9f6d-40e3-b6da-18ae251970d3)

Ortalama fiyatlar üzerinden ürün tipi ve markete göre yorum yapacak olursak:

## Summary

Analizi tamamladığımızda, 117 SHEIN ürünü için şu çıkarımları yapabiliyoruz:

- Jeans, elbise, hırka ve pantolonlar ortalama fiyatı en yüksek ürünler oluyor. Bu ürünler marketlerde sayıca az bulunuyor. Öte taraftan, bu ürünlerin indirim oranları diğer ürünlere göre yüksek. Bu da ortalama yüksek fiyatlarını daha cazip yapmak için bir strateji olabilir.
- Üst giyim ürün türlerini incelediğimizde(T-shirt, vest, top,..) $10'ın üstünde bir fiyat bulmak zor. Üstelik, SHEIN üzerinde sayıca en fazla ürün bu gruba ait. Fazla satılmalarından dolayı indirim oranları da bu ürünler için az(incelediğimizde %10'dan fazla indirim pek yok). Ürün incelemeleri de ortalama +500'den fazla diyebiliriz.
- SHEIN LUNE iki yönüyle öne çıkan bir market: Satılan ürün sayısı bakımından en fazla satan ilk 3 market arasında ve diğer marketlere göre en ucuz ortalama fiyat bu markette.
- SHEIN Essnce ise ortalamaya baktığımızda en fazla indirim oranına sahip. Ek olarak, bu markette de 10'dan fazla ürün bulunuyor.
- SHEIN WYWH ise en fazla indirim ve yüksek incelemeye sahip bir market. Ürün sayısının az olması da bir gerçek.
- SHEIN Qutie ve SHEIN Coolane de en ucuz marketler, fakat gelen incelemelere baktığımızda +100 değerlerini görmemiz, bu marketlerden alım yaparken tekrar düşünülmesi gerektiğini belirtiyor.
