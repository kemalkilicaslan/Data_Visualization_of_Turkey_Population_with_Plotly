# Türkiye 1927-2023 Erkek & Kadın ve Toplam Nüfus Verileri

Türkiye’de ilk nüfus sayımı 1927 yılında gerçekleştirilmiştir. Daha sonraki nüfus sayımları 1935 ile 1990 yılları arasında düzenli olarak sonu 0 ve 5 ile biten yıllarda uygulanmıştır. 1990 yılından sonra ise nüfus sayımının sonu sıfır ile biten yıllarda uygulanması kanunla belirlenmiş ve bu kapsamda ülkemizde son Genel Nüfus Sayımı 22 Ekim 2000 tarihinde yapılmıştır.

TÜİK'ten, 1927'yılından 2023 yılına kadar olan nüfus sayımı verileri(Toplam Nüfus, Erkek Nüfusu, Kadın Nüfusu) üzerinde görselleştirme işlemleri yapılmak üzere hazırlanmıştır.


```python
# Veri yükleme işlemi için gerekli kütüphaneler import ediliyor
import numpy as np
import pandas as pd

# Görselleştirme işlemleri için gerekli kütüphaneler import ediliyor
import plotly.graph_objs as go
import plotly.express as px
import plotly.io as pio
```


```python
# 'TR_population.csv' dosyası okunarak veri data değişkenine yüklenir.
data = pd.read_csv('TR_population.csv')
```


```python
# TÜİK'ten alınan nüfus verileri
data
```




|     | Yıl  | İl             | Toplam | Erkek  | Kadın   |
|-----|------|----------------|--------|--------|---------|
| 0   | 2023 | Adana          | 2270298| 1135046| 1135252 |
| 1   | 2023 | Adıyaman       | 604978 | 306779 | 298199  |
| 2   | 2023 | Afyonkarahisar | 751344 | 374705 | 376639  |
| 3   | 2023 | Ağrı           | 511238 | 265585 | 245653  |
| 4   | 2023 | Aksaray        | 438504 | 219006 | 219498  |
| ... | ...  | ...            | ...    | ...    | ...     |
| 2296| 1927 | Tokat          | 263063 | 124416 | 138647  |
| 2297| 1927 | Trabzon        | 290303 | 130926 | 159377  |
| 2298| 1927 | Van            | 75329  | 39525  | 35804   |
| 2299| 1927 | Yozgat         | 209497 | 98075  | 111422  |
| 2300| 1927 | Zonguldak      | 268909 | 127366 | 141543  |





```python
# Veri setinin bilgilerini gösterir
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2301 entries, 0 to 2300
    Data columns (total 5 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   Yıl     2301 non-null   int64 
     1   İl      2301 non-null   object
     2   Toplam  2301 non-null   int64 
     3   Erkek   2301 non-null   int64 
     4   Kadın   2301 non-null   int64 
    dtypes: int64(4), object(1)
    memory usage: 90.0+ KB



```python
# Veri setindeki sütun isimlerini listeler
data.columns
```




    Index(['Yıl', 'İl', 'Toplam', 'Erkek', 'Kadın'], dtype='object')



## Veri Görselleştirme
___
Plotly kütüphanesi ile bar grafiği kullanıldı çünkü matplotlib basit, kolay ve geniş kullanımıyla birlikte statiktir web siteleri üzerinde etkileşim gösteremez, plotly ise fare işaretleriyle (yakınlaştırma, uzaklaştırma vs.) detaylı bilgi ve kolay okunabilirliği ile bu veriseti üzerinde kullanımı tercih edilmiştir.

### 1927 Yılından 2023 yılına kadar olan Toplam, Erkek ve Kadın Nüfus Değişimleri
___

#### Çizgi Grafiği


```python
# 1927 yılından sonraki verileri içeren yeni bir veri çerçevesi oluşturulur
data_after_1927 = data[data['Yıl'] >= 1927]

# Yıllara göre toplam nüfus verilerinin toplanması ve veri çerçevesinin sıfırlanması
total_population_by_year = data_after_1927.groupby('Yıl')['Toplam'].sum().reset_index()

# Grafik oluşturulması ve başlığın ayarlanması
fig = px.line(total_population_by_year, x='Yıl', y='Toplam', title="1927'den 2023'e kadar Toplam Nüfus Değişimi")

# X ekseninin başlığının ayarlanması ve lineer şekilde işaretlenmesi
fig.update_xaxes(title='Yıl', tickmode='linear', dtick=1)

# Y ekseninin başlığının ayarlanması
fig.update_yaxes(title='Toplam Nüfus')

# Grafik çizgilerinin renginin siyah olarak ayarlanması
fig.update_traces(line=dict(color='black'))

# Grafik gösterimi
fig.show()

# 1927 yılından sonraki verilerin seçilmesi
data_after_1927 = data[data['Yıl'] >= 1927]

# Yıllara göre erkek ve kadın nüfuslarının toplanması
total_population_by_year = data_after_1927.groupby('Yıl').agg({'Erkek':'sum', 'Kadın':'sum'}).reset_index()

# Grafik oluşturulması ve başlığın ayarlanması
fig = px.line(total_population_by_year, x='Yıl', y=['Erkek', 'Kadın'], title="1927'den 2023'e kadar Erkek ve Kadın Nüfus Değişimi")

# X ekseninin başlığının ayarlanması ve lineer şekilde işaretlenmesi
fig.update_xaxes(title='Yıl', tickmode='linear', dtick=1)

# Y ekseninin başlığının ayarlanması
fig.update_yaxes(title='Nüfus')

# Erkek çizgisinin renginin mavi olarak ayarlanması
fig.update_traces(line=dict(color='blue'), selector=dict(name='Erkek'))

# Kadın çizgisinin renginin kırmızı olarak ayarlanması
fig.update_traces(line=dict(color='red'), selector=dict(name='Kadın'))

# Grafik gösterimi
fig.show()
```

![1927_den_2023_e_kadar_Toplam_Nufus_Degisimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1927_den_2023_e_kadar_Toplam_Nufus_Degisimi.png)

![1927_den_2023_e_kadar_Erkek_ve_Kadin_Nufus_Degisimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1927_den_2023_e_kadar_Erkek_ve_Kadin_Nufus_Degisimi.png)

## Türkiye'nin En Kalabalık İlk 3 İlinin Veri Görselleştirme İşlemleri
___


```python
# İstanbul'a ait verilerin seçilmesi
istanbul = data[data['İl'] == 'İstanbul']

# 'İl' sütunu düşürülerek sadece İstanbul verileri alınıyor
istanbul_data = istanbul.drop(columns=['İl'])

# İstanbul verilerini yıllara göre artan sıralama
istanbul_sorted_population = istanbul_data.sort_values(by='Yıl')

# İstanbul verilerinin 'istanbul_population.csv' adlı bir CSV dosyasına kaydedilmesi, index sütunu dahil edilmiyor
istanbul_sorted_population.to_csv('istanbul_sorted_population.csv', index=False)

# Ankara'ya ait verilerin seçilmesi
ankara = data[data['İl'] == 'Ankara']

# 'İl' sütunu düşürülerek sadece Ankara verileri alınıyor
ankara_sorted = ankara.drop(columns=['İl'])

# Ankara verilerini yıllara göre artan sıralama
ankara_sorted_population = ankara_sorted.sort_values(by='Yıl')

# Ankara verilerinin 'ankara_population.csv' adlı bir CSV dosyasına kaydedilmesi, index sütunu dahil edilmiyor
ankara_sorted_population.to_csv('ankara_sorted_population.csv', index=False)

# İzmir'e ait verilerin seçilmesi
izmir = data[data['İl'] == 'İzmir']

# 'İl' sütunu düşürülerek sadece İzmir verileri alınıyor
izmir_data = izmir.drop(columns=['İl'])

# İzmir verilerini yıllara göre artan sıralama
izmir_sorted_population = izmir_data.sort_values(by='Yıl')

# İzmir verilerinin 'izmir_population.csv' adlı bir CSV dosyasına kaydedilmesi, index sütunu dahil edilmiyor
izmir_sorted_population.to_csv('izmir_sorted_population.csv', index=False)
```

### İstanbul Nüfus Değişimi (1927-2023)
___


```python
# İstanbul verileri
print("                                       ")
print("               İstanbul                ")
print("_______________________________________")
istanbul_sorted_population
```

                                           
                   İstanbul                
    _______________________________________





|     | Yıl  | Toplam   | Erkek    | Kadın    |
|-----|------|----------|----------|----------|
| 2273| 1927 | 794444   | 404558   | 389886   |
| 2215| 1935 | 883599   | 457343   | 426256   |
| 2156| 1940 | 991237   | 538982   | 452255   |
| 2093| 1945 | 1078399  | 593494   | 484905   |
| 2030| 1950 | 1166477  | 615308   | 551169   |
| 1965| 1955 | 1533822  | 858565   | 675257   |
| 1898| 1960 | 1882092  | 1028130  | 853962   |
| 1831| 1965 | 2293823  | 1243900  | 1049923  |
| 1764| 1970 | 3019032  | 1624658  | 1394374  |
| 1697| 1975 | 3904588  | 2064250  | 1840338  |
| 1630| 1980 | 4741890  | 2482247  | 2259643  |
| 1563| 1985 | 5842985  | 3041847  | 2801138  |
| 1493| 1990 | 7309190  | 3798761  | 3510429  |
| 1416| 2000 | 10018735 | 5088535  | 4930200  |
| 1335| 2007 | 12573836 | 6291763  | 6282073  |
| 1254| 2008 | 12697164 | 6386772  | 6310392  |
| 1173| 2009 | 12915158 | 6498997  | 6416161  |
| 1092| 2010 | 13255685 | 6655094  | 6600591  |
| 1011| 2011 | 13624240 | 6845981  | 6778259  |
| 930 | 2012 | 13854740 | 6956908  | 6897832  |
| 849 | 2013 | 14160467 | 7115721  | 7044746  |
| 768 | 2014 | 14377018 | 7221158  | 7155860  |
| 687 | 2015 | 14657434 | 7360499  | 7296935  |
| 606 | 2016 | 14804116 | 7424390  | 7379726  |
| 525 | 2017 | 15029231 | 7529491  | 7499740  |
| 444 | 2018 | 15067724 | 7542231  | 7525493  |
| 363 | 2019 | 15519267 | 7790256  | 7729011  |
| 282 | 2020 | 15462452 | 7750836  | 7711616  |
| 201 | 2021 | 15840900 | 7933686  | 7907214  |
| 120 | 2022 | 15907951 | 7955820  | 7952131  |
| 39  | 2023 | 15655924 | 7806787  | 7849137  |





```python
# Veriler
yillar = istanbul_sorted_population['Yıl']
erkek_nufus = istanbul_sorted_population['Erkek']
kadin_nufus = istanbul_sorted_population['Kadın']

# Figür oluşturma
fig = go.Figure()

# Erkek nüfusun grafiği
fig.add_trace(go.Scatter(x=yillar, y=erkek_nufus, mode='lines', name='Erkek Nüfus'))

# Kadın nüfusun grafiği
fig.add_trace(go.Scatter(x=yillar, y=kadin_nufus, mode='lines', name='Kadın Nüfus'))

# Figürü güncelleme ve düzenleme
fig.update_layout(title='İstanbul Nüfus Değişimi (1927-2023)',
                   xaxis_title='Yıl',
                   yaxis_title='Nüfus',
                   legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01),
                   xaxis=dict(
                       tickmode='array',
                       tickvals=list(range(1927, 2024)),
                       ticktext=[str(year) for year in range(1927, 2024)]
                   ))

# Grafiği gösterme
fig.show()
```

![Istanbul_Nufus_Degisimi_1927_2023](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Izmir_Nufus_Degisimi_1927_2023.png)

___
**Yukarıdaki verilere ve grafiğe göre İstanbul ilinin erkek ve kadın nüfus farkının en çok olduğu yıl erkek nüfusunun kadın nüfusundan 288332 farkla 1990 yılıdır, aradaki nüfus farkının kapandığı yıl ise 3689 farkla 2022 yılıdır. Kadın nüfusunun erkek nüfusunu geçtiği ilk yıl ise 42350 farkla 2023 yılıdır. Bu yıllara ait pasta grafiği aşağıdaki gibidir.**
___

#### Pasta Grafiği


```python
# 1990 yılı verileri
istanbul_yil_1990 = 1990
istanbul_erkek_1990 = 3798761
istanbul_kadin_1990 = 3510429

istanbul_fig_1990 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[istanbul_erkek_1990, istanbul_kadin_1990])])
istanbul_fig_1990.update_traces(marker=dict(colors=['blue', 'red']))
istanbul_fig_1990.update_layout(
    title=f"İstanbul {istanbul_yil_1990} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="İstanbul İçin Erkek ve Kadın Nüfus Farkının En Çok Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbul_fig_1990.show()
```

![Istanbul_1990_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_1990_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# 2022 yılı verileri
istanbul_yil_2022 = 2022
istanbul_erkek_2022 = 7955820
istanbul_kadin_2022 = 7952131

istanbul_fig_2022 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[istanbul_erkek_2022, istanbul_kadin_2022])])
istanbul_fig_2022.update_traces(marker=dict(colors=['blue', 'red']))
istanbul_fig_2022.update_layout(
    title=f"İstanbul {istanbul_yil_2022} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="İstanbul İçin Erkek ve Kadın Nüfus Farkının En Az Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbul_fig_2022.show()
```

![Istanbul_2022_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_2022_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# 2023 yılı verileri
istanbul_yil_2023 = 2023
istanbul_erkek_2023 = 7806787
istanbul_kadin_2023 = 7849137

istanbul_fig_2023 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[istanbul_erkek_2023, istanbul_kadin_2023])])
istanbul_fig_2023.update_traces(marker=dict(colors=['blue', 'red']))
istanbul_fig_2023.update_layout(
    title=f"İstanbul {istanbul_yil_2023} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="İstanbul İçin Kadın Nüfusunun Erkek Nüfusunu Geçtiği İlk Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbul_fig_2023.show()
```

![Istanbul_2023_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_2023_Yili_Nufus_Cinsiyet_Dagilimi.png)

### Ankara Nüfus Değişimi (1927-2023)
___


```python
# Ankara verileri
print("                                      ")
print("                 Ankara               ")
print("______________________________________")
ankara_sorted_population
```

                                          
                     Ankara               
    ______________________________________





|     | Yıl  | Toplam   | Erkek    | Kadın    |
|-----|------|----------|----------|----------|
| 2249| 1927 | 404720   | 205368   | 199352   |
| 2192| 1935 | 534025   | 272843   | 261182   |
| 2129| 1940 | 602965   | 305626   | 297339   |
| 2066| 1945 | 695526   | 370768   | 324758   |
| 2003| 1950 | 819693   | 433371   | 386322   |
| 1938| 1955 | 1120864  | 604887   | 515977   |
| 1871| 1960 | 1321380  | 712768   | 608612   |
| 1804| 1965 | 1644302  | 872680   | 771622   |
| 1737| 1970 | 2041658  | 1067437  | 974221   |
| 1670| 1975 | 2585293  | 1359373  | 1225920  |
| 1603| 1980 | 2854689  | 1466584  | 1388105  |
| 1536| 1985 | 3306327  | 1702805  | 1603522  |
| 1464| 1990 | 3236626  | 1658276  | 1578350  |
| 1383| 2000 | 4007860  | 2027105  | 1980755  |
| 1302| 2007 | 4466756  | 2225033  | 2241723  |
| 1221| 2008 | 4548939  | 2267779  | 2281160  |
| 1140| 2009 | 4650802  | 2318633  | 2332169  |
| 1059| 2010 | 4771716  | 2379226  | 2392490  |
| 978 | 2011 | 4890893  | 2439058  | 2451835  |
| 897 | 2012 | 4965542  | 2474456  | 2491086  |
| 816 | 2013 | 5045083  | 2507525  | 2537558  |
| 735 | 2014 | 5150072  | 2562805  | 2587267  |
| 654 | 2015 | 5270575  | 2621235  | 2649340  |
| 573 | 2016 | 5346518  | 2653431  | 2693087  |
| 492 | 2017 | 5445026  | 2702492  | 2742534  |
| 411 | 2018 | 5503985  | 2728900  | 2775085  |
| 330 | 2019 | 5639076  | 2793850  | 2845226  |
| 249 | 2020 | 5663322  | 2805877  | 2857445  |
| 168 | 2021 | 5747325  | 2843409  | 2903916  |
| 87  | 2022 | 5782285  | 2856479  | 2925806  |
| 6   | 2023 | 5803482  | 2860361  | 2943121  |





```python
# Veriler
yillar_ankara = ankara_sorted_population['Yıl']
erkek_nufus_ankara = ankara_sorted_population['Erkek']
kadin_nufus_ankara = ankara_sorted_population['Kadın']

# Figür oluşturma
fig_ankara = go.Figure()

# Erkek nüfusun grafiği
fig_ankara.add_trace(go.Scatter(x=yillar_ankara, y=erkek_nufus_ankara, mode='lines', name='Erkek Nüfus'))

# Kadın nüfusun grafiği
fig_ankara.add_trace(go.Scatter(x=yillar_ankara, y=kadin_nufus_ankara, mode='lines', name='Kadın Nüfus'))

# Figürü güncelleme ve düzenleme
fig_ankara.update_layout(title='Ankara Nüfus Değişimi (1927-2023)',
                         xaxis_title='Yıl',
                         yaxis_title='Nüfus',
                         legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01),
                         xaxis=dict(
                             tickmode='array',
                             tickvals=list(range(1927, 2024)),
                             ticktext=[str(year) for year in range(1927, 2024)]
                         ))

# Grafiği gösterme
fig_ankara.show()
```

![Ankara_Nufus_Degisimi_1927_2023](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_Nufus_Degisimi_1927_2023.png)

___
**Yukarıdaki verilere ve grafiğe göre Ankara ilinin erkek ve kadın nüfus farkının en çok olduğu yıl erkek nüfusunun kadın nüfusundan 133453 farkla 1975 yılıdır, aradaki nüfus farkının en az olduğu yıl ise 6016 farkla 1927 yılıdır. Kadın nüfusunun erkek nüfusunu geçtiği ilk yıl 16690 farkla 2007 yılıdır, kadın nüfusunun erkek nüfusundan en fazla olduğu yıl ise 82760 farkla 2023 yılıdır. Bu yıllara ait pasta grafiği aşağıdaki gibidir.**
___


```python
# 1975 yılı verileri
ankara_yil_1975 = 1975
ankara_erkek_1975 = 1359373
ankara_kadin_1975 = 1225920

ankara_fig_1975 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[ankara_erkek_1975, ankara_kadin_1975])])
ankara_fig_1975.update_traces(marker=dict(colors=['blue', 'red']))
ankara_fig_1975.update_layout(
    title=f"Ankara {ankara_yil_1975} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="Ankara İçin Erkek ve Kadın Nüfus Farkının En Çok Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
ankara_fig_1975.show()
```

![Ankara_1975_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_1975_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# 1927 yılı verileri
ankara_yil_1927 = 1927
ankara_erkek_1927 = 205368
ankara_kadin_1927 = 199352

ankara_fig_1927 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[ankara_erkek_1927, ankara_kadin_1927])])
ankara_fig_1927.update_traces(marker=dict(colors=['blue', 'red']))
ankara_fig_1927.update_layout(
    title=f"Ankara {ankara_yil_1927} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="Ankara İçin Erkek ve Kadın Nüfus Farkının En Az Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
ankara_fig_1927.show()
```

![Ankara_1927_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_1927_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# 2007 yılı verileri
ankara_yil_2007 = 2007
ankara_erkek_2007 = 2225033
ankara_kadin_2007 = 2241723

ankara_fig_2007 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[ankara_erkek_2007, ankara_kadin_2007])])
ankara_fig_2007.update_traces(marker=dict(colors=['blue', 'red']))
ankara_fig_2007.update_layout(
    title=f"Ankara {ankara_yil_2007} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="Ankara İçin Kadın Nüfusunun Erkek Nüfusunu Geçtiği İlk Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
ankara_fig_2007.show()
```

![Ankara_2007_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_2007_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# 2023 yılı verileri
ankara_yil_2023 = 2023
ankara_erkek_2023 = 2860361
ankara_kadin_2023 = 2943121

ankara_fig_2023 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[ankara_erkek_2023, ankara_kadin_2023])])
ankara_fig_2023.update_traces(marker=dict(colors=['blue', 'red']))
ankara_fig_2023.update_layout(
    title=f"Ankara {ankara_yil_2023} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="Ankara İçin Kadın Nüfusunun Erkek Nüfusundan En Fazla Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
ankara_fig_2023.show()
```

![Ankara_2023_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_2023_Yili_Nufus_Cinsiyet_Dagilimi.png)

### İzmir Nüfus Değişimi (1927-2023)
___


```python
# İzmir verileri
print("                                      ")
print("                İzmir                 ")
print("______________________________________")
izmir_sorted_population
```

                                          
                    İzmir                 
    ______________________________________





|     | Yıl  | Toplam   | Erkek    | Kadın    |
|-----|------|----------|----------|----------|
| 2274| 1927 | 526005   | 269262   | 256743   |
| 2216| 1935 | 596850   | 304969   | 291881   |
| 2157| 1940 | 640107   | 325459   | 314648   |
| 2094| 1945 | 673581   | 342951   | 330630   |
| 2031| 1950 | 768411   | 388744   | 379667   |
| 1966| 1955 | 910496   | 469816   | 440680   |
| 1899| 1960 | 1063490  | 552498   | 510992   |
| 1832| 1965 | 1234667  | 641118   | 593549   |
| 1765| 1970 | 1427173  | 739429   | 687744   |
| 1698| 1975 | 1673966  | 868403   | 805563   |
| 1631| 1980 | 1976763  | 1021989  | 954774   |
| 1564| 1985 | 2317829  | 1198236  | 1119593  |
| 1494| 1990 | 2694770  | 1379778  | 1314992  |
| 1417| 2000 | 3370866  | 1698819  | 1672047  |
| 1336| 2007 | 3739353  | 1872579  | 1866774  |
| 1255| 2008 | 3795978  | 1897792  | 1898186  |
| 1174| 2009 | 3868308  | 1933681  | 1934627  |
| 1093| 2010 | 3948848  | 1985368  | 1963480  |
| 1012| 2011 | 3965232  | 1979088  | 1986144  |
| 931 | 2012 | 4005459  | 1999246  | 2006213  |
| 850 | 2013 | 4061074  | 2027334  | 2033740  |
| 769 | 2014 | 4113072  | 2050424  | 2062648  |
| 688 | 2015 | 4168415  | 2078224  | 2090191  |
| 607 | 2016 | 4223545  | 2104632  | 2118913  |
| 526 | 2017 | 4279677  | 2133548  | 2146129  |
| 445 | 2018 | 4320519  | 2152585  | 2167934  |
| 364 | 2019 | 4367251  | 2174319  | 2192932  |
| 283 | 2020 | 4394694  | 2187226  | 2207468  |
| 202 | 2021 | 4425789  | 2199287  | 2226502  |
| 121 | 2022 | 4462056  | 2215716  | 2246340  |
| 40  | 2023 | 4479525  | 2221180  | 2258345  |





```python
# Veriler
yillar_izmir = izmir_sorted_population['Yıl']
erkek_nufus_izmir = izmir_sorted_population['Erkek']
kadin_nufus_izmir = izmir_sorted_population['Kadın']

# Figür oluşturma
fig_izmir = go.Figure()

# Erkek nüfusun grafiği
fig_izmir.add_trace(go.Scatter(x=yillar_izmir, y=erkek_nufus_izmir, mode='lines', name='Erkek Nüfus'))

# Kadın nüfusun grafiği
fig_izmir.add_trace(go.Scatter(x=yillar_izmir, y=kadin_nufus_izmir, mode='lines', name='Kadın Nüfus'))

# Figürü güncelleme ve düzenleme
fig_izmir.update_layout(title='İzmir Nüfus Değişimi (1927-2023)',
                        xaxis_title='Yıl',
                        yaxis_title='Nüfus',
                        legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01),
                        xaxis=dict(
                            tickmode='array',
                            tickvals=list(range(1927, 2024)),
                            ticktext=[str(year) for year in range(1927, 2024)]
                        ))

# Grafiği gösterme
fig_izmir.show()
```

![Izmir_Nufus_Degisimi_1927_2023](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Izmir_Nufus_Degisimi_1927_2023.png)

___
**Yukarıdaki verilere ve grafiğe göre İzmir ilinin erkek ve kadın nüfus farkının en çok olduğu yıl erkek nüfusunun kadın nüfusundan 78643 farkla 1985 yılıdır, aradaki nüfus farkının en az olduğu ve ilk defa kadın nüfusunun erkek nüfusunu geçtiği yıl ise 394 farkla 2008 yılıdır. Kadın nüfusunun erkek nüfusundan en fazla olduğu yıl ise 37165 farkla 2023 yılıdır. Bu yıllara ait pasta grafiği aşağıdaki gibidir.**
___


```python
# 1985 yılı verileri
izmir_yil_1985 = 1985
izmir_erkek_1985 = 1198236
izmir_kadin_1985 = 1119593

izmir_fig_1985 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[izmir_erkek_1985, izmir_kadin_1985])])
izmir_fig_1985.update_traces(marker=dict(colors=['blue', 'red']))
izmir_fig_1985.update_layout(
    title=f"İzmir {izmir_yil_1985} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="İzmir İçin Erkek ve Kadın Nüfus Farkının En Çok Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
izmir_fig_1985.show()
```


![Izmir_1985_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Izmir_1985_Yili_Nufus_Cinsiyet_Dagilimi.png)


___


```python
# 2008 yılı verileri
izmir_yil_2008 = 2008
izmir_erkek_2008 = 1897792
izmir_kadin_2008 = 1898186

izmir_fig_2008 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[izmir_erkek_2008, izmir_kadin_2008])])
izmir_fig_2008.update_traces(marker=dict(colors=['blue', 'red']))
izmir_fig_2008.update_layout(
    title=f"İzmir {izmir_yil_2008} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="İzmir İçin Kadın Nüfusunun Erkek Nüfusunu Geçtiği İlk ve Erkek Nüfus ile Kadın Nüfus Farkının En Az Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
izmir_fig_2008.show()
```

![Izmir_2008_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Izmir_2008_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# 2023 yılı verileri
izmir_yil_2023 = 2023
izmir_erkek_2023 = 2221180
izmir_kadin_2023 = 2258345

izmir_fig_2023 = go.Figure(data=[go.Pie(labels=['Erkek', 'Kadın'], 
                                   values=[izmir_erkek_2023, izmir_kadin_2023])])
izmir_fig_2023.update_traces(marker=dict(colors=['blue', 'red']))
izmir_fig_2023.update_layout(
    title=f"İzmir {izmir_yil_2023} Yılı Nüfus Cinsiyet Dağılımı",
    annotations=[
        dict(
            text="İzmir İçin Kadın Nüfusunun Erkek Nüfusundan En Fazla Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
izmir_fig_2023.show()
```

![Izmir_2023_Yili_Nufus_Cinsiyet_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Izmir_2023_Yili_Nufus_Cinsiyet_Dagilimi.png)

___


```python
# İstanbul, Ankara ve İzmir verileri
toplam_nufus_istanbul = pd.read_csv('istanbul_sorted_population.csv')
toplam_nufus_ankara = pd.read_csv('ankara_sorted_population.csv')
toplam_nufus_izmir = pd.read_csv('izmir_sorted_population.csv')

# İstanbul, Ankara ve İzmir için figür oluşturma
fig = go.Figure()

# İstanbul için grafiği ekleme
fig.add_trace(go.Scatter(x=toplam_nufus_istanbul['Yıl'], y=toplam_nufus_istanbul['Toplam'], mode='lines', name='İstanbul', line=dict(color='gray')))

# Ankara için grafiği ekleme
fig.add_trace(go.Scatter(x=toplam_nufus_ankara['Yıl'], y=toplam_nufus_ankara['Toplam'], mode='lines', name='Ankara', line=dict(color='brown')))

# İzmir için grafiği ekleme
fig.add_trace(go.Scatter(x=toplam_nufus_izmir['Yıl'], y=toplam_nufus_izmir['Toplam'], mode='lines', name='İzmir', line=dict(color='orange')))

# Grafiği düzenleme
fig.update_layout(title='1927-2023 İstanbul, Ankara ve İzmir Toplam Nüfus Değişimi',
                   xaxis_title='Yıl',
                   yaxis_title='Toplam Nüfus',
                   xaxis=dict(
                       tickmode='array',
                       tickvals=list(range(1927, 2024)),
                       ticktext=[str(year) for year in range(1927, 2024)]
                   ),
                   legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01))

# Grafiği gösterme
fig.show()
```

![1927_2023_Istanbul_Ankara_ve_Izmir_Toplam_Nufus_Degisimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1927_2023_Istanbul_Ankara_ve_Izmir_Toplam_Nufus_Degisimi.png)

### İstanbul, Ankara ve İzmir Toplam Nüfusu Değişimi (1927-2023)

___
**İstanbul nüfusunun Ankara nüfusundan 10125666  farkla ve İzmir nüfusundan 11445895 farkla en fazla olduğu yıl 2022 yılıdır, Ankara nüfusunun İzmir nüfusundan en fazla olduğu yıl ise 1323957 farkla 2023 yılıdır, İstanbul ile Ankara  nüfusu arasındaki en az fark ise 1950 yılında 346784, İstanbu ile İzmir nüfusu arasındaki en az fark 268439 ile 1927 yılıdır. Ankara İzmir arasındaki nüfus farkının en az olduğu yıl ise 21945 kişi ile 1945 yılıdır.**
___

#### Donut Grafiği


```python
istanbulankara_yil_2022 = 2022
istanbul_2022 = 15907951
ankara_2022 = 5782285

labels = ['İstanbul', 'Ankara']
values = [istanbul_2022, ankara_2022]

# Use `hole` to create a donut-like pie chart
istanbulankara_fig_2022 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
istanbulankara_fig_2022.update_traces(marker=dict(colors=['gray', 'brown']))
istanbulankara_fig_2022.update_layout(
    title=f"İstanbul ve Ankara {istanbulankara_yil_2022} Yılı Nüfus Dağılımı",
    annotations=[
        dict(
            text="İstanbul nüfusunun Ankara nüfusundan En Fazla Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbulankara_fig_2022.show()
```

![Istanbul_ve_Ankara_2022_Yili_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_ve_Ankara_2022_Yili_Nufus_Dagilimi.png)

___


```python
# 1950 yılı verileri
istanbulankara_yil_1950 = 1950
istanbul_1950 = 1166477
ankara_1950 = 819693

labels = ['İstanbul', 'Ankara']
values = [istanbul_1950, ankara_1950]

# Use `hole` to create a donut-like pie chart
istanbulankara_fig_1950 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
istanbulankara_fig_1950.update_traces(marker=dict(colors=['gray', 'brown']))
istanbulankara_fig_1950.update_layout(
    title=f"İstanbul ve Ankara {istanbulankara_yil_1950} Yılı Nüfus Dağılımı",
    annotations=[
        dict(
            text="İstanbul nüfusunun Ankara nüfusundan En Az Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbulankara_fig_1950.show()
```

![Istanbul_ve_Ankara_1950_Yili_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_ve_Ankara_1950_Yili_Nufus_Dagilimi.png)

___


```python
istanbulizmir_yil_2022 = 2022
istanbul_2022 = 15907951
izmir_2022 = 4462056

labels = ['İstanbul', 'İzmir']
values = [istanbul_2022, izmir_2022]

# Use `hole` to create a donut-like pie chart
istanbulizmir_fig_2022 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
istanbulizmir_fig_2022.update_traces(marker=dict(colors=['gray', 'orange']))
istanbulizmir_fig_2022.update_layout(
    title=f"İstanbul ve İzmir {istanbulizmir_yil_2022} Yılı Nüfus Dağılımı",
    annotations=[
        dict(
            text="İstanbul nüfusunun İzmir nüfusundan En Fazla Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbulizmir_fig_2022.show()
```

![Istanbul_ve_Izmir_2022_Yili_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_ve_Izmir_2022_Yili_Nufus_Dagilimi.png)

___


```python
# 1927 yılı verileri
istanbulizmir_yil_1927 = 1927
istanbul_1927 = 794444
izmir_1927 = 526005

labels = ['İstanbul', 'İzmir']
values = [istanbul_1927, izmir_1927]

# Use `hole` to create a donut-like pie chart
istanbulizmir_fig_1927 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
istanbulizmir_fig_1927.update_traces(marker=dict(colors=['gray', 'orange']))
istanbulizmir_fig_1927.update_layout(
    title=f"İstanbul ve İzmir {istanbulizmir_yil_1927} Yılı Nüfus Dağılımı",
    annotations=[
        dict(
            text="İstanbul nüfusunun İzmir nüfusundan En Az Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbulizmir_fig_1927.show()
```

![Istanbul_ve_Izmir_1927_Yili_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Istanbul_ve_Izmir_1927_Yili_Nufus_Dagilimi.png)

___


```python
# 2023 yılı verileri
ankaraizmir_yil_2023 = 2023
ankara_2023 = 5803482
izmir_2023 = 4479525

labels = ['Ankara', 'İzmir']
values = [ankara_2023, izmir_2023]

# Use `hole` to create a donut-like pie chart
ankaraizmir_fig_2023 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
ankaraizmir_fig_2023.update_traces(marker=dict(colors=['brown', 'orange']))
ankaraizmir_fig_2023.update_layout(
    title=f"Ankara ve İzmir {ankaraizmir_yil_2023} Yılı Nüfus Dağılımı",
    annotations=[
        dict(
            text="Ankara nüfusunun İzmir nüfusundan En Fazla Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
ankaraizmir_fig_2023.show()
```

![Ankara_ve_Izmir_2023_Yili_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_ve_Izmir_2023_Yili_Nufus_Dagilimi.png)

___


```python
# 1945 yılı verileri
ankaraizmir_yil_1945 = 1945
ankara_1945 = 695526
izmir_1945 = 673581

labels = ['Ankara', 'İzmir']
values = [ankara_1945, izmir_1945]

# Use `hole` to create a donut-like pie chart
istanbulankara_fig_1945 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
istanbulankara_fig_1945.update_traces(marker=dict(colors=['brown', 'orange']))
istanbulankara_fig_1945.update_layout(
    title=f"Ankara ve İzmir {ankaraizmir_yil_1945} Yılı Nüfus Dağılımı",
    annotations=[
        dict(
            text="Ankara nüfusunun İzmir nüfusundan En Az Olduğu Yıl",
            showarrow=False,
            x=0.5,
            y=-0.3,
            font=dict(size=14)
        )
    ]
)
istanbulankara_fig_1945.show()
```

![Ankara_ve_Izmir_1945_Yili_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Ankara_ve_Izmir_1945_Yili_Nufus_Dagilimi.png)

### Yukarıdaki 5 kritik yıl için en kalabalık bu üç şehrin pasta grafiği ile görselleştirilmesi

___


```python
# 1927 Verileri
yil1927 = 1927
istanbul1927 = 794444
ankara1927 = 404720
izmir1927 = 526005

labels = ['İstanbul', 'Ankara', 'İzmir']
values = [istanbul1927, ankara1927, izmir1927]

fig1927 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
fig1927.update_traces(marker=dict(colors=['gray', 'brown', 'orange']))
fig1927.update_layout(title=f"{yil1927} Yılı İstanbul, Ankara ve İzmir Nüfus Dağılımı")
fig1927.show()
```

![1927_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1927_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi.png)

___


```python
# 1945 Verileri
yil1945 = 1945
istanbul1945 = 1078399
ankara1945 = 695526
izmir1945 = 673581

labels = ['İstanbul', 'Ankara', 'İzmir']
values = [istanbul1945, ankara1945, izmir1945]

fig1945 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
fig1945.update_traces(marker=dict(colors=['gray', 'brown', 'orange']))
fig1945.update_layout(title=f"{yil1945} Yılı İstanbul, Ankara ve İzmir Nüfus Dağılımı")
fig1945.show()
```

![1945_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1945_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi.png)

___


```python
# 1950 Verileri
yil1950 = 1950
istanbul1950 = 1166477
ankara1950 = 819693
izmir1950 = 768411

labels = ['İstanbul', 'Ankara', 'İzmir']
values = [istanbul1950, ankara1950, izmir1950]

fig1950 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
fig1950.update_traces(marker=dict(colors=['gray', 'brown', 'orange']))
fig1950.update_layout(title=f"{yil1950} Yılı İstanbul, Ankara ve İzmir Nüfus Dağılımı")
fig1950.show()
```

![1950_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1950_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi.png)

___


```python
# 2022 Verileri
yil2022 = 2022
istanbul2022 = 15907951
ankara2022 = 5782285
izmir2022 = 4462056

labels = ['İstanbul', 'Ankara', 'İzmir']
values = [istanbul2022, ankara2022, izmir2022]

# Donut chart oluşturma
fig2022 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
fig2022.update_traces(marker=dict(colors=['gray', 'brown', 'orange']))
fig2022.update_layout(title=f"{yil2022} Yılı İstanbul, Ankara ve İzmir Nüfus Dağılımı")
fig2022.show()
```

![2022_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2022_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi.png)

___


```python
# 2023 Verileri
yil2023 = 2023
istanbul2023 = 15655924
ankara2023 = 5803482
izmir2023 = 4479525

labels = ['İstanbul', 'Ankara', 'İzmir']
values = [istanbul2023, ankara2023, izmir2023]

# Donut chart oluşturma
fig2023 = go.Figure(data=[go.Pie(labels=labels, values=values, hole=0.4)])
fig2023.update_traces(marker=dict(colors=['gray', 'brown', 'orange']))
fig2023.update_layout(title=f"{yil2023} Yılı İstanbul, Ankara ve İzmir Nüfus Dağılımı")
fig2023.show()
```

![2023_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Istanbul_Ankara_ve_Izmir_Nufus_Dagilimi.png)

### İstanbul, Ankara ve İzmir Erkek Nüfusu Değişimi (1927-2023)


```python
# İstanbul, Ankara ve İzmir verileri
erkek_nufus_istanbul = pd.read_csv('istanbul_sorted_population.csv')
erkek_nufus_ankara = pd.read_csv('ankara_sorted_population.csv')
erkek_nufus_izmir = pd.read_csv('izmir_sorted_population.csv')

# Figür oluşturma
fig = go.Figure()

# İstanbul için grafiği ekleme
fig.add_trace(go.Scatter(x=erkek_nufus_istanbul['Yıl'], y=erkek_nufus_istanbul['Erkek'], mode='lines', name='İstanbul', line=dict(color='gray')))

# Ankara için grafiği ekleme
fig.add_trace(go.Scatter(x=erkek_nufus_ankara['Yıl'], y=erkek_nufus_ankara['Erkek'], mode='lines', name='Ankara', line=dict(color='brown')))

# İzmir için grafiği ekleme
fig.add_trace(go.Scatter(x=erkek_nufus_izmir['Yıl'], y=erkek_nufus_izmir['Erkek'], mode='lines', name='İzmir', line=dict(color='orange')))

# Grafiği düzenleme
fig.update_layout(title='1927-2023 İstanbul, Ankara ve İzmir Erkek Nüfus Değişimi',
                   xaxis_title='Yıl',
                   yaxis_title='Erkek Nüfus',
                   xaxis=dict(
                       tickmode='array',
                       tickvals=list(range(1927, 2024)),
                       ticktext=[str(year) for year in range(1927, 2024)]
                   ),
                   legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01))

# Grafiği gösterme
fig.show()
```

![1927_2023_Istanbul_Ankara_ve_Izmir_Erkek_Nufus_Degisimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1927_2023_Istanbul_Ankara_ve_Izmir_Erkek_Nufus_Degisimi.png)

___

### İstanbul, Ankara ve İzmir Kadın Nüfusu Değişimi (1927-2023)


```python
# İstanbul, Ankara ve İzmir verileri
kadın_nufus_istanbul = pd.read_csv('istanbul_sorted_population.csv')
kadın_nufus_ankara = pd.read_csv('ankara_sorted_population.csv')
kadın_nufus_izmir = pd.read_csv('izmir_sorted_population.csv')

# Figür oluşturma
fig = go.Figure()

# İstanbul için grafiği ekleme
fig.add_trace(go.Scatter(x=kadın_nufus_istanbul['Yıl'], y=kadın_nufus_istanbul['Kadın'], mode='lines', name='İstanbul', line=dict(color='gray')))

# Ankara için grafiği ekleme
fig.add_trace(go.Scatter(x=kadın_nufus_ankara['Yıl'], y=kadın_nufus_ankara['Kadın'], mode='lines', name='Ankara', line=dict(color='brown')))

# İzmir için grafiği ekleme
fig.add_trace(go.Scatter(x=kadın_nufus_izmir['Yıl'], y=kadın_nufus_izmir['Kadın'], mode='lines', name='İzmir', line=dict(color='orange')))

# Grafiği düzenleme
fig.update_layout(title='1927-2023 İstanbul, Ankara ve İzmir Kadın Nüfus Değişimi',
                   xaxis_title='Yıl',
                   yaxis_title='Kadın Nüfus',
                   xaxis=dict(
                       tickmode='array',
                       tickvals=list(range(1927, 2024)),
                       ticktext=[str(year) for year in range(1927, 2024)]
                   ),
                   legend=dict(yanchor="top", y=0.99, xanchor="left", x=0.01))

# Grafiği gösterme
fig.show()
```

![1927_2023_Istanbul_Ankara_ve_Izmir_Kadin_Nufus_Degisimi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/1927_2023_Istanbul_Ankara_ve_Izmir_Kadin_Nufus_Degisimi.png)

___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___

# Türkiye 2023 Yılı Erkek & Kadın ve Toplam Nüfus Verileri

Türkiye’de ilk nüfus sayımı 1927 yılında gerçekleştirilmiştir. Daha sonraki nüfus sayımları 1935 ile 1990 yılları arasında düzenli olarak sonu 0 ve 5 ile biten yıllarda uygulanmıştır. 1990 yılından sonra ise nüfus sayımının sonu sıfır ile biten yıllarda uygulanması kanunla belirlenmiş ve bu kapsamda ülkemizde son Genel Nüfus Sayımı 22 Ekim 2000 tarihinde yapılmıştır.

TÜİK'ten, 1927'yılından 2023 yılına kadar olan nüfus sayımı verileri(Toplam Nüfus, Erkek Nüfusu, Kadın Nüfusu) üzerinde görselleştirme işlemleri yapılmak üzere hazırlanmıştır.


```python
# Veri yükleme işlemi için gerekli kütüphaneler import ediliyor
import numpy as np
import pandas as pd

# Görselleştirme işlemleri için gerekli kütüphaneler import ediliyor
import plotly.graph_objs as go
import plotly.express as px
import plotly.io as pio
import geopandas as gpd
import folium
```


```python
# 'TR_population.csv' dosyası okunarak veri data değişkenine yüklenir.
data = pd.read_csv('TR_population.csv')

# 'TR_map.json' dosyası okunarak geometrik veri geo_data değişkenine yüklenir.
geo_data = gpd.read_file('TR_map.json')
```


```python
# TÜİK'ten alınan nüfus verileri
data
```




|     | Yıl  | İl             | Toplam | Erkek  | Kadın  |
|-----|------|----------------|--------|--------|--------|
| 0   | 2023 | Adana          | 2270298| 1135046| 1135252|
| 1   | 2023 | Adıyaman       | 604978 | 306779 | 298199 |
| 2   | 2023 | Afyonkarahisar | 751344 | 374705 | 376639 |
| 3   | 2023 | Ağrı           | 511238 | 265585 | 245653 |
| 4   | 2023 | Aksaray        | 438504 | 219006 | 219498 |
| ... | ...  | ...            | ...    | ...    | ...    |
| 2296| 1927 | Tokat          | 263063 | 124416 | 138647 |
| 2297| 1927 | Trabzon        | 290303 | 130926 | 159377 |
| 2298| 1927 | Van            | 75329  | 39525  | 35804  |
| 2299| 1927 | Yozgat         | 209497 | 98075  | 111422 |
| 2300| 1927 | Zonguldak      | 268909 | 127366 | 141543 |





```python
# Türkiye'nin 81 ilinin koordinatlarını içeren veriler
geo_data
```




|     | id | name           | geometry                                          |
|-----|----|----------------|---------------------------------------------------|
| 0   | 01 | Adana          | POLYGON ((36.06160 37.00968, 36.03897 37.01673... |
| 1   | 02 | Adıyaman       | POLYGON ((39.18896 38.04305, 39.14989 38.02073... |
| 2   | 03 | Afyonkarahisar | POLYGON ((30.44179 39.20629, 30.47724 39.19960... |
| 3   | 04 | Ağrı           | POLYGON ((43.31842 39.96932, 43.34095 39.91260... |
| 4   | 05 | Amasya         | POLYGON ((35.08926 41.08692, 35.40004 41.02070... |
| ... | ...| ...            | ...                                               |
| 76  | 77 | Yalova         | POLYGON ((29.42842 40.69088, 29.42935 40.60145... |
| 77  | 78 | Karabük        | POLYGON ((32.81798 41.57635, 32.85219 41.54775... |
| 78  | 79 | Kilis          | POLYGON ((37.52214 36.67360, 37.44612 36.63420... |
| 79  | 80 | Osmaniye       | POLYGON ((36.72120 37.27119, 36.67614 37.22961... |
| 80  | 81 | Düzce          | POLYGON ((31.34642 41.15073, 31.38458 41.12323... |






```python
# Veri setinin bilgilerini gösterir
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2301 entries, 0 to 2300
    Data columns (total 5 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   Yıl     2301 non-null   int64 
     1   İl      2301 non-null   object
     2   Toplam  2301 non-null   int64 
     3   Erkek   2301 non-null   int64 
     4   Kadın   2301 non-null   int64 
    dtypes: int64(4), object(1)
    memory usage: 90.0+ KB



```python
# Coğrafi veri setinin bilgilerini gösterir
geo_data.info()
```

    <class 'geopandas.geodataframe.GeoDataFrame'>
    RangeIndex: 81 entries, 0 to 80
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype   
    ---  ------    --------------  -----   
     0   id        81 non-null     object  
     1   name      81 non-null     object  
     2   geometry  81 non-null     geometry
    dtypes: geometry(1), object(2)
    memory usage: 2.0+ KB



```python
# Veri setindeki sütun isimlerini listeler
data.columns
```




    Index(['Yıl', 'İl', 'Toplam', 'Erkek', 'Kadın'], dtype='object')




```python
# Coğrafi veri setindeki sütun isimlerini listeler
geo_data.columns
```




    Index(['id', 'name', 'geometry'], dtype='object')



___

##2023 Verileri


```python
# 2023 yılına ait verileri seçer
data2023 = data[data['Yıl'] == 2023]

# Seçilen verileri gösterir
data2023
```




|    | Yıl  | İl             | Toplam  | Erkek   | Kadın   |
|----|------|----------------|---------|---------|---------|
| 0  | 2023 | Adana          | 2270298 | 1135046 | 1135252 |
| 1  | 2023 | Adıyaman       | 604978  | 306779  | 298199  |
| 2  | 2023 | Afyonkarahisar | 751344  | 374705  | 376639  |
| 3  | 2023 | Ağrı           | 511238  | 265585  | 245653  |
| 4  | 2023 | Aksaray        | 438504  | 219006  | 219498  |
| ...| ...  | ...            | ...     | ...     | ...     |
| 76 | 2023 | Uşak           | 377001  | 187253  | 189748  |
| 77 | 2023 | Van            | 1127612 | 574993  | 552619  |
| 78 | 2023 | Yalova         | 304780  | 152100  | 152680  |
| 79 | 2023 | Yozgat         | 420699  | 210051  | 210648  |
| 80 | 2023 | Zonguldak      | 591492  | 294001  | 297491  |




___
**Alfabetik sırada olan 2023 verilerini plaka kodlarına göre eşleme ve sıralama**

geo_data verisetinde verilen il ve plaka kodu id'lerine göre data2023 veri setindeki illeri plaka kodlarına yani id'ye göre sıralamak için iki veri setini birleştirme işlemi ardından artan sıralmaya göre illeri sıralama işlemi yapılmaktadır.
___


```python
# İlleri alfabetik sıraya göre sıralar ve plaka kodlarına göre eşleştirir
merged_data2023 = pd.merge(geo_data, data2023, left_on='name', right_on='İl', how='inner')

# Sıralanmış verileri plaka koduna göre sıralar
sorted_data2023 = merged_data2023.sort_values(by='id')

# Sıralanmış verileri gösterir
sorted_data2023
```




|     | id | name           | geometry                                          |  Yıl | İl             | Toplam   |  Erkek  |  Kadın  |
|-----|----|----------------|---------------------------------------------------|------|----------------|----------|---------|---------|
| 0   | 01 | Adana          | POLYGON ((36.06160 37.00968, 36.03897 37.01673... | 2023 | Adana          | 2270298  | 1135046 | 1135252 |
| 1   | 02 | Adıyaman       | POLYGON ((39.18896 38.04305, 39.14989 38.02073... | 2023 | Adıyaman       | 604978   | 306779  | 298199  |
| 2   | 03 | Afyonkarahisar | POLYGON ((30.44179 39.20629, 30.47724 39.19960... | 2023 | Afyonkarahisar | 751344   | 374705  | 376639  |
| 3   | 04 | Ağrı           | POLYGON ((43.31842 39.96932, 43.34095 39.91260... | 2023 | Ağrı           | 511238   | 265585  | 245653  |
| 4   | 05 | Amasya         | POLYGON ((35.08926 41.08692, 35.40004 41.02070... | 2023 | Amasya         | 339529   | 168290  | 171239  |
| ... | ...| ...            | ...                                               | ...  | ...            | ...      | ...     | ...     |
| 76  | 77 | Yalova         | POLYGON ((29.42842 40.69088, 29.42935 40.60145... | 2023 | Yalova         | 304780   | 152100  | 152680  |
| 77  | 78 | Karabük        | POLYGON ((32.81798 41.57635, 32.85219 41.54775... | 2023 | Karabük        | 255242   | 128370  | 126872  |
| 78  | 79 | Kilis          | POLYGON ((37.52214 36.67360, 37.44612 36.63420... | 2023 | Kilis          | 155179   | 78198   | 76981   |
| 79  | 80 | Osmaniye       | POLYGON ((36.72120 37.27119, 36.67614 37.22961... | 2023 | Osmaniye       | 557666   | 280450  | 277216  |
| 80  | 81 | Düzce          | POLYGON ((31.34642 41.15073, 31.38458 41.12323... | 2023 | Düzce          | 409865   | 204734  | 205131  |





```python
# Gereksiz sütunları kaldırır
new_data2023 = sorted_data2023.drop(columns=['id', 'name', 'geometry', 'Yıl'])

# new_data2023 veri setini CSV dosyasına kaydeder
new_data2023.to_csv('new_data2023.csv', index=False)

# new_data2023 veri setini tekrar okur
new_data2023 = pd.read_csv('new_data2023.csv')

# new_data2023 veri setini gösterir
new_data2023
```




|    | İl             | Toplam  | Erkek   | Kadın   |
|----|----------------|---------|---------|---------|
| 0  | Adana          | 2270298 | 1135046 | 1135252 |
| 1  | Adıyaman       | 604978  | 306779  | 298199  |
| 2  | Afyonkarahisar | 751344  | 374705  | 376639  |
| 3  | Ağrı           | 511238  | 265585  | 245653  |
| 4  | Amasya         | 339529  | 168290  | 171239  |
| ...| ...            | ...     | ...     | ...     |
| 76 | Yalova         | 304780  | 152100  | 152680  |
| 77 | Karabük        | 255242  | 128370  | 126872  |
| 78 | Kilis          | 155179  | 78198   | 76981   |
| 79 | Osmaniye       | 557666  | 280450  | 277216  |
| 80 | Düzce          | 409865  | 204734  | 205131  |





```python
# new_data2023 veri setinin bilgilerini gösterir
new_data2023.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 81 entries, 0 to 80
    Data columns (total 4 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   İl      81 non-null     object
     1   Toplam  81 non-null     int64 
     2   Erkek   81 non-null     int64 
     3   Kadın   81 non-null     int64 
    dtypes: int64(3), object(1)
    memory usage: 2.7+ KB



```python
# new_data2023 veri setindeki sütun isimlerini listeler
new_data2023.columns
```




    Index(['İl', 'Toplam', 'Erkek', 'Kadın'], dtype='object')




```python
# new_data2023 veri setinin istatistiklerini gösterir
new_data2023.describe().T
```




|          | count | mean        | std         | min    | 25%     | 50%     | 75%      | max       |
|----------|-------|-------------|-------------|--------|---------|---------|----------|-----------|
| Toplam   | 81.0  | 1.053980e+06| 1.901677e+06| 86047.0| 304780.0| 557666.0| 1116618.0| 15655924.0|
| Erkek    | 81.0  | 5.275811e+05| 9.471827e+05| 43603.0| 154235.0| 280450.0| 568064.0 | 7806787.0 |
| Kadın    | 81.0  | 5.263988e+05| 9.545368e+05| 42207.0| 152680.0| 271568.0| 548554.0 | 7849137.0 |




## Veri Görselleştirme

Plotly kütüphanesi ile bar grafiği kullanıldı çünkü matplotlib basit, kolay ve geniş kullanımıyla birlikte statiktir web siteleri üzerinde etkileşim gösteremez, plotly ise fare işaretleriyle (yakınlaştırma, uzaklaştırma vs.) detaylı bilgi ve kolay okunabilirliği ile bu veriseti üzerinde kullanımı tercih edilmiştir.

### 2023 yılı için veri görselleştirme işlemleri

#### Çubuk Grafiği


```python
# Milyon notasyonu için lambda fonksiyonu kullanılıyor
millions = lambda x: f"{x / 10**6:.0f}M"

# Toplam nüfus için bar grafiği oluşturuluyor
total_pop = go.Bar(x=new_data2023['İl'], y=new_data2023['Toplam'], marker_color='black', name='Toplam Nüfus')

# Erkek nüfusu için bar grafiği oluşturuluyor
male_pop = go.Bar(x=new_data2023['İl'], y=new_data2023['Erkek'], marker_color='blue', name='Erkek Nüfusu')

# Kadın nüfusu için bar grafiği oluşturuluyor
female_pop = go.Bar(x=new_data2023['İl'], y=new_data2023['Kadın'], marker_color='red', name='Kadın Nüfusu')

# Tüm bar grafikleri içeren bir şekil (figür) oluşturuluyor
fig = go.Figure(data=[total_pop, male_pop, female_pop])

# Şeklin düzeni güncelleniyor
fig.update_layout(

    # Başlık tanımlanıyor.
    title='2023 Yılı Toplam, Erkek, Kadın Nüfusu',

    # X ekseni etiketi tanımlanıyor
    xaxis=dict(title='İl'),

    # Y ekseni etiketi tanımlanıyor ve özel bir format belirleniyor
    yaxis=dict(
        title='Nüfus',

        # Kesirli sayıların gösterilme biçimi belirleniyor
        tickformat='.0f',

        # İşaretlenen değerler belirleniyor
        tickvals=[0, 1000000, 2000000, 3000000, 4000000, 5000000, 6000000, 7000000, 8000000, 15000000],

        # İşaretlenen değerlere karşılık gelen metinler belirleniyor
        ticktext=[millions(0), millions(1000000), millions(2000000), millions(3000000), millions(4000000), millions(5000000), millions(6000000), millions(7000000), millions(8000000), millions(15000000)]
    ),

    # Bar grafiklerinin modu belirleniyor
    barmode='group',

    # X ekseni etiketlerinin açısı belirleniyor
    xaxis_tickangle=-90,

    # Şeklin yüksekliği belirleniyor
    height=600,

    # Şeklin genişliği belirleniyor
    width=1550,
)

# Grafik gösterimi
fig.show()
```

![2023_Yili_Toplam_Erkek_Kadin_Nufusu](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Toplam_Erkek_Kadin_Nufusu.png)

```python
# Milyon notasyonu için lambda fonksiyonu kullanılıyor
millions = lambda x: f"{x / 10**6:.0f}M"

# Toplam nüfus için bar grafiği oluşturuluyor
total_pop = go.Bar(x=new_data2023['İl'], y=new_data2023['Toplam'], marker_color='black')

# Toplam nüfus grafiğinin düzeni tanımlanıyor
layout_total_pop = go.Layout(

    # Başlık tanımlanıyor
    title='2023 Yılı Toplam Nüfus',

    # X ekseni etiketi tanımlanıyor
    xaxis=dict(title='İl'),
    yaxis=dict(

        # Y ekseni etiketi tanımlanıyor
        title='Nüfus',

        # Kesirli sayıların gösterilme biçimi belirleniyor
        tickformat='.0f',

        # İşaretlenen değerler belirleniyor
        tickvals=[0, 1000000, 2000000, 3000000, 4000000, 5000000, 6000000, 15000000],

        # İşaretlenen değerlere karşılık gelen metinler belirleniyor
        ticktext=[millions(0), millions(1000000), millions(2000000), millions(3000000), millions(4000000), millions(5000000), millions(6000000), millions(15000000)]
    ),

    # X ekseni etiketlerinin açısı belirleniyor
    xaxis_tickangle=-90,

    # Grafiğin yüksekliği belirleniyor
    height=600,

    # Grafiğin genişliği belirleniyor
    width=1550
)

# Erkek nüfusu için bar grafiği oluşturuluyor
male_pop = go.Bar(x=new_data2023['İl'], y=new_data2023['Erkek'], marker_color='blue')

# Erkek nüfusu grafiğinin düzeni tanımlanıyor
layout_male_pop = go.Layout(

    # Başlık tanımlanıyor
    title='2023 Yılı Erkek Nüfusu',

    # X ekseni etiketi tanımlanıyor
    xaxis=dict(title='İl'),
    yaxis=dict(

        # Y ekseni etiketi tanımlanıyor
        title='Nüfus',

        # Kesirli sayıların gösterilme biçimi belirleniyor
        tickformat='.0f',

        # İşaretlenen değerler belirleniyor
        tickvals=[0, 1000000, 2000000, 3000000, 4000000, 5000000, 6000000, 7000000, 8000000, 10000000],

        # İşaretlenen değerlere karşılık gelen metinler belirleniyor
        ticktext=[millions(0), millions(1000000), millions(2000000), millions(3000000), millions(4000000), millions(5000000), millions(6000000), millions(7000000), millions(8000000), millions(10000000)]
    ),

    # X ekseni etiketlerinin açısı belirleniyor
    xaxis_tickangle=-90,

    # Grafiğin yüksekliği belirleniyor
    height=600,

    # Grafiğin genişliği belirleniyor
    width=1550
)

# Kadın nüfusu için bar grafiği oluşturuluyor
female_pop = go.Bar(x=new_data2023['İl'], y=new_data2023['Kadın'], marker_color='red')

# Kadın nüfusu grafiğinin düzeni tanımlanıyor
layout_female_pop = go.Layout(

    # Başlık tanımlanıyor.
    title='2023 Yılı Kadın Nüfusu',

    # X ekseni etiketi tanımlanıyor
    xaxis=dict(title='İl'),
    yaxis=dict(

        # Y ekseni etiketi tanımlanıyor
        title='Nüfus',

        # Kesirli sayıların gösterilme biçimi belirleniyor
        tickformat='.0f',

        # İşaretlenen değerler belirleniyor
        tickvals=[0, 1000000, 2000000, 3000000, 4000000, 5000000, 6000000, 7000000, 8000000, 10000000],

        # İşaretlenen değerlere karşılık gelen metinler belirleniyor
        ticktext=[millions(0), millions(1000000), millions(2000000), millions(3000000), millions(4000000), millions(5000000), millions(6000000), millions(7000000), millions(8000000), millions(10000000)]
    ),

    # X ekseni etiketlerinin açısı belirleniyor
    xaxis_tickangle=-90,

    # Grafiğin yüksekliği belirleniyor
    height=600,

    # Grafiğin genişliği belirleniyor
    width=1550
)

# Toplam nüfus grafiği gösteriliyor
pio.show(go.Figure(data=total_pop, layout=layout_total_pop))

# Erkek nüfusu grafiği gösteriliyor
pio.show(go.Figure(data=male_pop, layout=layout_male_pop))

# Kadın nüfusu grafiği gösteriliyor
pio.show(go.Figure(data=female_pop, layout=layout_female_pop))
```

![2023_Yili_Toplam_Nufus](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Toplam_Nufus.png)

![2023_Yili_Erkek_Nufusu](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Erkek_Nufusu.png)

![2023_Yili_Kadin_Nufusu](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Kadin_Nufusu.png)


```python
# Yeni sütun oluşturularak erkek nüfusun toplam nüfusa oranı hesaplanıyor
new_data2023['Erkek_oran'] = new_data2023['Erkek'] / new_data2023['Toplam']

# Yeni sütun oluşturularak kadın nüfusun toplam nüfusa oranı hesaplanıyor
new_data2023['Kadın_oran'] = new_data2023['Kadın'] / new_data2023['Toplam']

# İl, erkek nüfus, kadın nüfus, erkek nüfus oranı ve kadın nüfus oranı sütunlarını içeren bir DataFrame oluşturuluyor
new_data2023[['İl', 'Erkek', 'Kadın', 'Erkek_oran', 'Kadın_oran']]
```




| İl             | Erkek  | Kadın  | Erkek_oran | Kadın_oran |
|----------------|--------|--------|------------|------------|
| Adana          | 1135046| 1135252| 0.499955   | 0.500045   |
| Adıyaman       | 306779 | 298199 | 0.507091   | 0.492909   |
| Afyonkarahisar | 374705 | 376639 | 0.498713   | 0.501287   |
| Ağrı           | 265585 | 245653 | 0.519494   | 0.480506   |
| Amasya         | 168290 | 171239 | 0.495657   | 0.504343   |
| ...            | ...    | ...    | ...        | ...        |
| Yalova         | 152100 | 152680 | 0.499048   | 0.500952   |
| Karabük        | 128370 | 126872 | 0.502934   | 0.497066   |
| Kilis          | 78198  | 76981  | 0.503921   | 0.496079   |
| Osmaniye       | 280450 | 277216 | 0.502900   | 0.497100   |
| Düzce          | 204734 | 205131 | 0.499516   | 0.500484   |




#### Yığın Çubuk Grafiği


```python
# Erkek nüfusun toplam nüfusa oranı hesaplanıyor
new_data2023['Erkek_oran'] = new_data2023['Erkek'] / new_data2023['Toplam']

# Kadın nüfusun toplam nüfusa oranı hesaplanıyor.
new_data2023['Kadın_oran'] = new_data2023['Kadın'] / new_data2023['Toplam']

# İl isimleri sütunu oluşturuluyor
il_isimleri = new_data2023['İl']

# Erkek nüfus oranları sütunu oluşturuluyor
erkek_oranlar = new_data2023['Erkek_oran']

# Kadın nüfus oranları sütunu oluşturuluyor
kadın_oranlar = new_data2023['Kadın_oran']

# Erkek nüfus oranlarının string formatına dönüştürülmesi
erkek_oranlar_str = ['{:.2%}'.format(oran) for oran in erkek_oranlar]

# Kadın nüfus oranlarının string formatına dönüştürülmesi
kadın_oranlar_str = ['{:.2%}'.format(oran) for oran in kadın_oranlar]

# Boş bir figür oluşturuluyor
fig = go.Figure()

# Erkek nüfus oranlarını içeren bar grafiği oluşturuluyor
fig.add_trace(go.Bar(
    x=il_isimleri,
    y=erkek_oranlar,
    name='Erkek Oranı',
    marker_color='blue',

    # Grafiğe fare geldiğinde gösterilecek bilgi formatı belirleniyor
    hovertemplate='<b>%{x}</b><br><extra>Erkek Oranı: %{y:.2%}</extra>'
))

# Kadın nüfus oranlarını içeren bar grafiği oluşturuluyor
fig.add_trace(go.Bar(
    x=il_isimleri,
    y=kadın_oranlar,
    name='Kadın Oranı',
    marker_color='red',

    # Grafiğe fare geldiğinde gösterilecek bilgi formatı belirleniyor
    hovertemplate='<b>%{x}</b><br><extra>Kadın Oranı: %{y:.2%}</extra>'
))

# Grafiğin düzeni güncelleniyor
fig.update_layout(

    # Başlık tanımlanıyor
    title='Erkek ve Kadın Nüfus Yüzde (%) Oranları',

    # X ekseni etiketlerinin açısı belirleniyor
    xaxis_tickangle=-90,
    xaxis=dict(

        # X ekseni etiketi tanımlanıyor
        title='İller',

        # İşaretlenen değerlerin lineer olarak oluşturulmasını sağlar
        tickmode='linear'
    ),
    yaxis=dict(

        # Y ekseni etiketi tanımlanıyor.
        title='Nüfus Oranı'
    ),

    # Bar modu belirleniyor. 'stack' grafiği yığılmış hale getirir.
    barmode='stack'
)

# Grafik gösterimi
fig.show()
```

![Erkek_ve_Kadin_Nufus_Yuzde_Oranlari](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/Erkek_ve_Kadin_Nufus_Yuzde_Oranlari.png)

```python
# 2023 Yılı toplam nüfusa göre azalan sıralama
total_population_sorted2023 = new_data2023.sort_values(by='Toplam', ascending=False)

# Sonuçları csv olarak kaydetme
total_population_sorted2023.to_csv('total_population_sorted2023.csv', index=False)

# Sonuçları gösterme
total_population_sorted2023
```




| İl        | Toplam   | Erkek   | Kadın   | Erkek_oran | Kadın_oran |
|-----------|----------|---------|---------|------------|------------|
| İstanbul  | 15655924 | 7806787 | 7849137 | 0.498647   | 0.501353   |
| Ankara    | 5803482  | 2860361 | 2943121 | 0.492870   | 0.507130   |
| İzmir     | 4479525  | 2221180 | 2258345 | 0.495852   | 0.504148   |
| Bursa     | 3214571  | 1605941 | 1608630 | 0.499582   | 0.500418   |
| Antalya   | 2696249  | 1357198 | 1339051 | 0.503365   | 0.496635   |
| ...       | ...      | ...     | ...     | ...        | ...        |
| Kilis     | 155179   | 78198   | 76981   | 0.503921   | 0.496079   |
| Gümüşhane | 148539   | 74581   | 73958   | 0.502097   | 0.497903   |
| Ardahan   | 92819    | 48239   | 44580   | 0.519710   | 0.480290   |
| Tunceli   | 89317    | 47110   | 42207   | 0.527447   | 0.472553   |
| Bayburt   | 86047    | 43603   | 42444   | 0.506735   | 0.493265   |





```python
# Milyon notasyonu için lambda fonksiyonu kullanılıyor
millions = lambda x: f"{x / 10**6:.0f}M"

# Toplam nüfus için bar grafiği oluşturuluyor
total_pop = go.Bar(

    # X ekseni için il isimleri belirleniyor
    x=total_population_sorted2023['İl'],

    # Y ekseni için il nüfusları belirleniyor
    y=total_population_sorted2023['Toplam'],

    # Bar renkleri belirleniyor
    marker_color='black',

    # Grafiğin adı belirleniyor
    name='Toplam Nüfus'
)

# Boş bir figür oluşturuluyor ve sadece total_pop bar grafiği ekleniyor
fig = go.Figure(data=[total_pop])

# Şeklin düzeni güncelleniyor
fig.update_layout(

    # Başlık tanımlanıyor.
    title='2023 Yılı Toplam Nüfusunun En Kalabalık İlinden En Seyrek İline Sütun Grafiği',

    # X ekseni etiketi tanımlanıyor
    xaxis=dict(title='İl'),
    yaxis=dict(

        # Y ekseni etiketi tanımlanıyor
        title='Nüfus',

        # Y ekseni değerlerinin gösterim formatı belirleniyor
        tickformat='.0f',

        # İşaretlenen değerler belirleniyor
        tickvals=[0, 1000000, 2000000, 3000000, 4000000, 5000000, 6000000, 10000000, 15000000],

        # İşaretlenen değerlere karşılık gelen metinler belirleniyor
        ticktext=[millions(0), millions(1000000), millions(2000000), millions(3000000), millions(4000000), millions(5000000), millions(6000000), millions(10000000), millions(15000000)]
    ),

    # Bar modu belirleniyor
    barmode='group',

    # X ekseni etiketlerinin açısı belirleniyor
    xaxis_tickangle=-90,

    # Grafiğin yüksekliği belirleniyor
    height=600,

    # Grafiğin genişliği belirleniyor
    width=1550
)

# Grafik gösterimi
fig.show()
```

![2023_Yili_Toplam_Nufusunun_En_Kalabalik_Ilinden_En_Seyrek_Iline_Sutun_Grafigi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Toplam_Nufusunun_En_Kalabalik_Ilinden_En_Seyrek_Iline_Sutun_Grafigi.png)

### Erkek ve Kadın Nüfus Oranının En Yüsek Olduğu İller


```python
# 2023 yılı erkek nüfus oranına göre azalan sıralama
erkek_oran_sorted2023 = new_data2023.sort_values(by='Erkek_oran', ascending=False)

# Sonuçları gösterme
erkek_oran_sorted2023
```




| İl        | Toplam | Erkek   | Kadın   | Erkek_oran | Kadın_oran |
|-----------|--------|---------|---------|------------|------------|
| Hakkari   | 287625 | 154235  | 133390  | 0.536236   | 0.463764   |
| Tunceli   | 89317  | 47110   | 42207   | 0.527447   | 0.472553   |
| Şırnak    | 570745 | 299177  | 271568  | 0.524187   | 0.475813   |
| Iğdır     | 209738 | 109710  | 100028  | 0.523081   | 0.476919   |
| Siirt     | 347412 | 180736  | 166676  | 0.520235   | 0.479765   |
| ...       | ...    | ...     | ...     | ...        | ...        |
| Elazığ    | 604411 | 298845  | 305566  | 0.494440   | 0.505560   |
| Kütahya   | 575674 | 284542  | 291132  | 0.494276   | 0.505724   |
| Kastamonu | 388990 | 192048  | 196942  | 0.493709   | 0.506291   |
| Nevşehir  | 315994 | 155897  | 160097  | 0.493354   | 0.506646   |
| Ankara    | 5803482| 2860361 | 2943121 | 0.492870   | 0.507130   |





```python
# 2023 yılı kadın nüfus oranına göre azalan sıralama
kadın_oran_sorted2023 = new_data2023.sort_values(by='Kadın_oran', ascending=False)

# Sonuçları gösterme
kadın_oran_sorted2023
```




| İl        | Toplam  | Erkek  | Kadın  | Erkek_oran | Kadın_oran |
|-----------|---------|--------|--------|------------|------------|
| Ankara    | 5803482 | 2860361| 2943121| 0.492870   | 0.507130   |
| Nevşehir  | 315994  | 155897 | 160097 | 0.493354   | 0.506646   |
| Kastamonu | 388990  | 192048 | 196942 | 0.493709   | 0.506291   |
| Kütahya   | 575674  | 284542 | 291132 | 0.494276   | 0.505724   |
| Elazığ    | 604411  | 298845 | 305566 | 0.494440   | 0.505560   |
| ...       | ...     | ...    | ...    | ...        | ...        |
| Siirt     | 347412  | 180736 | 166676 | 0.520235   | 0.479765   |
| Iğdır     | 209738  | 109710 | 100028 | 0.523081   | 0.476919   |
| Şırnak    | 570745  | 299177 | 271568 | 0.524187   | 0.475813   |
| Tunceli   | 89317   | 47110  | 42207  | 0.527447   | 0.472553   |
| Hakkari   | 287625  | 154235 | 133390 | 0.536236   | 0.463764   |





```python
# Fonksiyon tanımlanıyor, bu fonksiyon verilerden bir sütunu kullanarak yığılmış sütun grafikleri oluşturur
def create_stacked_bar_chart(data, title, sorted_column, color1, color2, reverse=False):
    # Veriler sıralanıyor, varsayılan olarak artan sırada olacak şekilde
    sorted_data = data.sort_values(by=sorted_column, ascending=reverse)

    # Boş bir figür oluşturuluyor
    fig = go.Figure()

    # İlk bar grafiği (Erkek Oranı) oluşturuluyor ve figüre ekleniyor
    fig.add_trace(go.Bar(
        x=sorted_data['İl'],  # X ekseninde il isimleri kullanılıyor

        # Y ekseninde erkek oranları kullanılıyor (ters sıralama değilse), aksi takdirde kadın oranları kullanılıyor
        y=sorted_data['Erkek_oran'] if not reverse else sorted_data['Kadın_oran'],

        # Grafiğin adı belirleniyor
        name='Erkek Oranı' if not reverse else 'Kadın Oranı',

        # Bar rengi belirleniyor
        marker_color=color1,

        # Fare üzerine gelindiğinde gösterilecek bilgi formatı belirleniyor
        hovertemplate=f'<b>%{{x}}</b><br><extra>Erkek Oranı: %{{y:.2%}}</extra>'
    ))

    # İkinci bar grafiği (Kadın Oranı) oluşturuluyor ve figüre ekleniyor
    fig.add_trace(go.Bar(
        x=sorted_data['İl'],  # X ekseninde il isimleri kullanılıyor

        # Y ekseninde kadın oranları kullanılıyor (ters sıralama değilse), aksi takdirde erkek oranları kullanılıyor
        y=sorted_data['Kadın_oran'] if not reverse else sorted_data['Erkek_oran'],

        # Grafiğin adı belirleniyor
        name='Kadın Oranı' if not reverse else 'Erkek Oranı',

        # Bar rengi belirleniyor
        marker_color=color2,

        # Fare üzerine gelindiğinde gösterilecek bilgi formatı belirleniyor
        hovertemplate=f'<b>%{{x}}</b><br><extra>Kadın Oranı: %{{y:.2%}}</extra>'
    ))

    # Figürün düzeni güncelleniyor
    fig.update_layout(

        # Başlık tanımlanıyor
        title=title,

        # X eksenindeki etiketlerin açısı belirleniyor
        xaxis_tickangle=-90,
        xaxis=dict(

            # X ekseninin başlığı belirleniyor
            title='İller',

            # X eksenindeki etiketlerin düzeni lineer olarak belirleniyor
            tickmode='linear'
        ),
        yaxis=dict(

            # Y ekseninin başlığı belirleniyor
            title='Nüfus Oranı'
        ),

        # Bar modu yığılmış (stacked) olarak belirleniyor
        barmode='stack'
    )

    # Grafik gösterimi
    fig.show()

# Erkek nüfus oranının azalan sırayla grafiğini oluşturmak için fonksiyon çağrılıyor.
create_stacked_bar_chart(new_data2023, '2023 Yılı Erkek Nüfus Oranının Azalan Grafiği', 'Erkek_oran', 'blue', 'red')

# Kadın nüfus oranının azalan sırayla grafiğini oluşturmak için fonksiyon çağrılıyor.
create_stacked_bar_chart(new_data2023, '2023 Yılı Kadın Nüfus Oranının Azalan Grafiği', 'Kadın_oran', 'red', 'blue')
```

![2023_Yili_Erkek_Nufus_Oraninin_Azalan_Grafigi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Erkek_Nufus_Oraninin_Azalan_Grafigi.png)

![2023_Yili_Kadin_Nufus_Oraninin_Azalan_Grafigi](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Kadin_Nufus_Oraninin_Azalan_Grafigi.png)

### 2023 Yılı Nüfusu en yüksek ve en düşük 10 ilde erkek ve kadın oranlarını görselleştirelim

#### Gruplanmış Çubuk Grafiği


```python
# Fonksiyon tanımlanıyor, bu fonksiyon verilerden bir sütunu kullanarak sütun grafikleri oluşturur
def create_bar_chart(data, title):
    # Boş bir figür oluşturuluyor
    fig = go.Figure()

    # İlk bar grafiği (Erkek Oranı) oluşturuluyor ve figüre ekleniyor
    fig.add_trace(go.Bar(

        # X ekseninde il isimleri kullanılıyor
        x=data['İl'],

        # Y ekseninde erkek oranları kullanılıyor
        y=data['Erkek_oran'],

        # Grafiğin adı belirleniyor
        name='Erkek Oranı',

        # Bar rengi belirleniyor
        marker_color='blue',

        # Fare üzerine gelindiğinde gösterilecek bilgi formatı belirleniyor
        hovertemplate='<b>%{x}</b><br><extra>Erkek Oranı: %{y:.2%}</extra>'
    ))

    # İkinci bar grafiği (Kadın Oranı) oluşturuluyor ve figüre ekleniyor
    fig.add_trace(go.Bar(

        # X ekseninde il isimleri kullanılıyor
        x=data['İl'],

        # Y ekseninde kadın oranları kullanılıyor
        y=data['Kadın_oran'],

        # Grafiğin adı belirleniyor
        name='Kadın Oranı',

        # Bar rengi belirleniyor
        marker_color='red',

        # Fare üzerine gelindiğinde gösterilecek bilgi formatı belirleniyor
        hovertemplate='<b>%{x}</b><br><extra>Kadın Oranı: %{y:.2%}</extra>'
    ))

    # Figürün düzeni güncelleniyor
    fig.update_layout(

        # Başlık tanımlanıyor
        title=title,

        # X eksenindeki etiketlerin açısı belirleniyor
        xaxis_tickangle=-90,
        xaxis=dict(

            # X ekseninin başlığı belirleniyor
            title='İller',

            # X eksenindeki etiketlerin düzeni lineer olarak belirleniyor
            tickmode='linear'
        ),
        yaxis=dict(

            # Y ekseninin başlığı belirleniyor
            title='Oran'
        ),

        # Bar modu belirleniyor
        barmode='group'
    )

    # Grafik gösterimi
    fig.show()

# En yüksek 10 nüfusa sahip ilin erkek ve kadın oranlarını içeren bir DataFrame oluşturuluyor
top_10_cities = total_population_sorted2023.head(10).copy()
top_10_cities['Erkek_oran'] = top_10_cities['Erkek'] / top_10_cities['Toplam']
top_10_cities['Kadın_oran'] = top_10_cities['Kadın'] / top_10_cities['Toplam']

# En yüksek 10 nüfusa sahip ilin erkek ve kadın oranlarını gösteren bir grafik oluşturuluyor
create_bar_chart(top_10_cities, '2023 Yılı Nüfusu En Yüksek 10 İlin Erkek ve Kadın Oranları')

# En düşük 10 nüfusa sahip ilin erkek ve kadın oranlarını içeren bir DataFrame oluşturuluyor
bottom_10_cities = total_population_sorted2023.tail(10).copy()
bottom_10_cities['Erkek_oran'] = bottom_10_cities['Erkek'] / bottom_10_cities['Toplam']
bottom_10_cities['Kadın_oran'] = bottom_10_cities['Kadın'] / bottom_10_cities['Toplam']

# En düşük 10 nüfusa sahip ilin erkek ve kadın oranlarını gösteren bir grafik oluşturuluyor
create_bar_chart(bottom_10_cities, '2023 Yılı Nüfusu En Düşük 10 İlin Erkek ve Kadın Oranları')
```

![2023_Yili_Nufusu_En_Yuksek_10_Ilin_Erkek_ve_Kadin_Oranlari](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Nufusu_En_Yuksek_10_Ilin_Erkek_ve_Kadin_Oranlari.png)

![2023_Yili_Nufusu_En_Dusuk_10_Ilin_Erkek_ve_Kadin_Oranlari](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/2023_Yili_Nufusu_En_Dusuk_10_Ilin_Erkek_ve_Kadin_Oranlari.png)

```python
# 2023 yılı verilerimizi hatırlayalım
new_data2023
```




|    | İl             | Toplam   | Erkek   | Kadın   | Erkek_oran | Kadın_oran |
|----|----------------|----------|---------|---------|------------|------------|
| 0  | Adana          | 2270298  | 1135046 | 1135252 | 0.499955   | 0.500045   |
| 1  | Adıyaman       | 604978   | 306779  | 298199  | 0.507091   | 0.492909   |
| 2  | Afyonkarahisar | 751344   | 374705  | 376639  | 0.498713   | 0.501287   |
| 3  | Ağrı           | 511238   | 265585  | 245653  | 0.519494   | 0.480506   |
| 4  | Amasya         | 339529   | 168290  | 171239  | 0.495657   | 0.504343   |
| ...| ...            | ...      | ...     | ...     | ...        | ...        |
| 76 | Yalova         | 304780   | 152100  | 152680  | 0.499048   | 0.500952   |
| 77 | Karabük        | 255242   | 128370  | 126872  | 0.502934   | 0.497066   |
| 78 | Kilis          | 155179   | 78198   | 76981   | 0.503921   | 0.496079   |
| 79 | Osmaniye       | 557666   | 280450  | 277216  | 0.5029     | 0.4971     |
| 80 | Düzce          | 409865   | 204734  | 205131  | 0.499516   | 0.500484   |




### 2023 Yılı Toplam Nüfusun oluşturduğu Dağılım

#### Koroplet Haritalama


```python
# Türkiye haritası verisi TR_map.json dosyasından okunuyor
tr_map = gpd.read_file('TR_map.json')

# İl nüfus verisi total_population_sorted2023.csv dosyasından okunuyor
il_nufus = pd.read_csv('total_population_sorted2023.csv')

# Boş bir Folium haritası oluşturuluyor ve başlangıç konumu ve zoom seviyesi belirleniyor
turkey_map = folium.Map(location=[38.9683188, 35.3560241], zoom_start=6)

# Choropleth (alanlara göre renklendirme) katmanı oluşturuluyor ve Türkiye haritasına ekleniyor
folium.Choropleth(

    # Coğrafi veri dosyası belirtiliyo
    geo_data='TR_map.json',

    # Katman adı belirleniyor
    name='choropleth',

    # Veri seti belirtiliyor
    data=il_nufus,

    # Renklendirmede kullanılacak sütunlar belirleniyor
    columns=['İl', 'Toplam'],

    # Eşleşme anahtarı belirleniyor
    key_on='feature.properties.name',

    # Alan renkleri belirleniyor
    fill_color='Greys',

    # Doluluk opaklığı belirleniyor
    fill_opacity=0.6,

    # Çizgi opaklığı belirleniyor
    line_opacity=0.2,

    # Renk skalası için açıklama belirleniyor
    legend_name='2023 Yılı Toplam Nüfusu',

# Oluşturulan katman Türkiye haritasına ekleniyor
).add_to(turkey_map)

# Türkiye haritasındaki her il için döngü oluşturuluyor
for idx, row in tr_map.iterrows():

    # İl adı alınıyor
    il_ad = row['name']

    # İl adının, il nüfus verisindeki sırası belirleniyor
    nufus_sira = il_nufus[il_nufus['İl'] == il_ad].index[0] + 1

    # İl adının, il nüfus verisindeki toplam nüfusu alınıyor
    nufus_toplam = il_nufus[il_nufus['İl'] == il_ad]['Toplam'].iloc[0]

    # Her il için bir işaretçi (marker) oluşturuluyor ve Türkiye haritasına ekleniyor
    folium.Marker(

        # İşaretçinin konumu belirleniyor
        location=[row.geometry.centroid.y, row.geometry.centroid.x],

        # Fare üzerine gelindiğinde gösterilecek bilgi belirleniyor
        tooltip=f"{il_ad} <br> Nüfus: {nufus_toplam} <br>En kalabalık {nufus_sira}.şehir",

        # İşaretçi simgesi belirleniyor
        icon=folium.Icon(icon='map-pin', prefix='fa', color='black')

    # Oluşturulan işaretçi Türkiye haritasına ekleniyor
    ).add_to(turkey_map)

# Oluşturulan Türkiye haritası gösteriliyor
turkey_map
```

![turkey_map](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/turkey_map.png)

### 2023 Yılı Erkek Nüfusunun oluşturduğu Dağılım


```python
# 'Toplam', 'Kadın', 'Erkek_oran' ve 'Kadın_oran' sütunlarını içermeyen bir kopya DataFrame oluşturuluyor
erkeknüfus = total_population_sorted2023.drop(columns=['Toplam','Kadın','Erkek_oran','Kadın_oran'])

# 'Erkek' sütununa göre azalan şekilde sıralama yapılıyor
erkeknüfus = erkeknüfus.sort_values(by='Erkek', ascending=False)

# Oluşturulan DataFrame 'erkeknüfus.csv' adlı bir CSV dosyasına kaydediliyor ve index sütunu dahil edilmiyor
erkeknüfus.to_csv('erkeknüfus.csv', index=False)

# Oluşturulan DataFrame ekrana yazdırılıyor
erkeknüfus
```




|    | İl        | Erkek   |
|----|-----------|---------|
| 33 | İstanbul  | 7806787 |
|  5 | Ankara    | 2860361 |
| 34 | İzmir     | 2221180 |
| 15 | Bursa     | 1605941 |
|  6 | Antalya   | 1357198 |
| ...| ...       | ...     |
| 78 | Kilis     | 78198   |
| 28 | Gümüşhane | 74581   |
| 74 | Ardahan   | 48239   |
| 61 | Tunceli   | 47110   |
| 68 | Bayburt   | 43603   |





```python
# 'TR_map.json' dosyasından Türkiye haritası verisi okunuyor
tr_map = gpd.read_file('TR_map.json')

# 'erkeknüfus.csv' dosyasından il nüfus verisi okunuyor
il_nufus = pd.read_csv('erkeknüfus.csv')

# Boş bir Folium haritası oluşturuluyor ve başlangıç konumu ve zoom seviyesi belirleniyor
turkey_map_man = folium.Map(location=[38.9683188, 35.3560241], zoom_start=6)

# Choropleth (alanlara göre renklendirme) katmanı oluşturuluyor ve Türkiye haritasına ekleniyor
folium.Choropleth(

    # Coğrafi veri dosyası belirtiliyor
    geo_data='TR_map.json',

    # Katman adı belirleniyor
    name='choropleth',

    # Veri seti belirtiliyor
    data=il_nufus,

    # Renklendirmede kullanılacak sütunlar belirleniyor
    columns=['İl', 'Erkek'],

    # Eşleşme anahtarı belirleniyor
    key_on='feature.properties.name',

    # Alan renkleri belirleniyor
    fill_color='Blues',

    # Doluluk opaklığı belirleniyor
    fill_opacity=0.6,

    # Çizgi opaklığı belirleniyor
    line_opacity=0.2,

    # Renk skalası için açıklama belirleniyor
    legend_name='2023 Yılı Erkek Nüfusu',

# Oluşturulan choropleth katmanı Türkiye haritasına ekleniyor
).add_to(turkey_map_man)

# Türkiye haritasındaki her il için döngü oluşturuluyor
for idx, row in tr_map.iterrows():
    # İl adı alınıyor
    il_ad = row['name']

    # İl adının, il nüfus verisindeki sırası belirleniyor
    nufus_sira = il_nufus[il_nufus['İl'] == il_ad].index[0] + 1

    # İl adının, il nüfus verisindeki toplam nüfusu alınıyor
    nufus_toplam = il_nufus[il_nufus['İl'] == il_ad]['Erkek'].iloc[0]

    # Her il için bir işaretçi (marker) oluşturuluyor ve Türkiye haritasına ekleniyor
    folium.Marker(

        # İşaretçinin konumu belirleniyor
        location=[row.geometry.centroid.y, row.geometry.centroid.x],

        # Fare üzerine gelindiğinde gösterilecek bilgi belirleniyor
        tooltip=f"{il_ad} <br> Nüfus: {nufus_toplam} <br>Erkek nüfusu en kalabalık {nufus_sira}.şehir",

        # İşaretçi simgesi belirleniyor
        icon=folium.Icon(icon='map-pin', prefix='fa', color='darkblue')

    # Oluşturulan işaretçi Türkiye haritasına ekleniyor
    ).add_to(turkey_map_man)

# Oluşturulan Türkiye haritası gösteriliyor
turkey_map_man
```

![turkey_map_man](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/turkey_map_man.png)

### 2023 Yılı Kadın Nüfusunun oluşturduğu Dağılım


```python
# 'Toplam', 'Erkek', 'Erkek_oran' ve 'Kadın_oran' sütunlarını içermeyen bir kopya DataFrame oluşturuluyor
kadınnüfus = total_population_sorted2023.drop(columns=['Toplam','Erkek','Erkek_oran','Kadın_oran'])

# 'Kadın' sütununa göre azalan şekilde sıralama yapılıyor
kadınnüfus = kadınnüfus.sort_values(by='Kadın', ascending=False)

# Oluşturulan DataFrame 'kadınnüfus.csv' adlı bir CSV dosyasına kaydediliyor ve index sütunu dahil edilmiyor
kadınnüfus.to_csv('kadınnüfus.csv', index=False)

# Oluşturulan DataFrame ekrana yazdırılıyor
kadınnüfus
```




|    | İl         | Kadın   |
|----|------------|---------|
| 33 | İstanbul   | 7849137 |
| 5  | Ankara     | 2943121 |
| 34 | İzmir      | 2258345 |
| 15 | Bursa      | 1608630 |
| 6  | Antalya    | 1339051 |
| ...| ...        | ...     |
| 78 | Kilis      | 76981   |
| 28 | Gümüşhane  | 73958   |
| 74 | Ardahan    | 44580   |
| 68 | Bayburt    | 42444   |
| 61 | Tunceli    | 42207   |





```python
# 'TR_map.json' dosyasından Türkiye haritası verisi okunuyor
tr_map = gpd.read_file('TR_map.json')

# 'kadınnüfus.csv' dosyasından il nüfus verisi okunuyor
il_nufus = pd.read_csv('kadınnüfus.csv')

# Boş bir Folium haritası oluşturuluyor ve başlangıç konumu ve zoom seviyesi belirleniyor
turkey_map_woman = folium.Map(location=[38.9683188, 35.3560241], zoom_start=6)

# Choropleth (alanlara göre renklendirme) katmanı oluşturuluyor ve Türkiye haritasına ekleniyor
folium.Choropleth(

    # Coğrafi veri dosyası belirtiliyor
    geo_data='TR_map.json',

    # Katman adı belirleniyor
    name='choropleth',

    # Veri seti belirtiliyor
    data=il_nufus,

    # Renklendirmede kullanılacak sütunlar belirleniyor
    columns=['İl', 'Kadın'],

    # Eşleşme anahtarı belirleniyor
    key_on='feature.properties.name',

    # Alan renkleri belirleniyor
    fill_color='YlOrRd',

    # Doluluk opaklığı belirleniyor
    fill_opacity=0.6,

    # Çizgi opaklığı belirleniyor
    line_opacity=0.2,

    # Renk skalası için açıklama belirleniyor
    legend_name='2023 Yılı Kadın Nüfusu',

# Oluşturulan choropleth katmanı Türkiye haritasına ekleniyor
).add_to(turkey_map_woman)

# Türkiye haritasındaki her il için döngü oluşturuluyor
for idx, row in tr_map.iterrows():
    # İl adı alınıyor
    il_ad = row['name']

    # İl adının, il nüfus verisindeki sırası belirleniyor
    nufus_sira = il_nufus[il_nufus['İl'] == il_ad].index[0] + 1

    # İl adının, il nüfus verisindeki toplam nüfusu alınıyor
    nufus_toplam = il_nufus[il_nufus['İl'] == il_ad]['Kadın'].iloc[0]

    # Her il için bir işaretçi (marker) oluşturuluyor ve Türkiye haritasına ekleniyor
    folium.Marker(

        # İşaretçinin konumu belirleniyor
        location=[row.geometry.centroid.y, row.geometry.centroid.x],

        # Fare üzerine gelindiğinde gösterilecek bilgi belirleniyor
        tooltip=f"{il_ad} <br> Nüfus: {nufus_toplam} <br> Kadın nüfusu en kalabalık {nufus_sira}.şehir",

        # İşaretçi simgesi belirleniyor
        icon=folium.Icon(icon='map-pin', prefix='fa', color='red')

    # Oluşturulan işaretçi Türkiye haritasına ekleniyor
    ).add_to(turkey_map_woman)

# Oluşturulan Türkiye haritası gösteriliyor
turkey_map_woman
```

![turkey_map_woman](https://github.com/kemalkilicaslan/Data_Visualization_of_Turkey_Population_with_Plotly/blob/main/turkey_map_woman.png)