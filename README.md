# Store Sales - Time Series Forecasting
### Using machine learning to predict grocery sales


## Purpose

![image](https://github.com/sonielyy/shein_scraping_project/assets/71605453/c5b20b8a-9fff-4339-ae55-154df7a765bc)

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
