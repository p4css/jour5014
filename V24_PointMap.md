

## Mapping Youbike Location

這個練習的目標是讀取台北市Youbike2.0的站台資料，並繪製點位圖來標記站台滿車或缺車的情形。

以下這段程式碼是用於從台北市政府提供的YouBike實時數據API取得資料，並將其轉換為R語言中的資料框架(data frame)格式，最後選擇資料框架的前六個變數來展示。步驟如下：

1.  載入`httr`和`jsonlite`套件：這兩個套件在R中常用於處理HTTP請求和JSON資料。`httr`套件用於發送網路請求，而`jsonlite`套件則用於解析JSON格式的資料。

2.  設定URL：將`url`變數設為指向台北市政府提供的YouBike實時數據API的網址。

3.  使用`GET`函數發送請求：透過`httr`套件的`GET`函數向設定的URL發送HTTP GET請求，以取得YouBike的實時數據。

4.  解析JSON資料：使用`jsonlite`套件的`fromJSON`函數，將從API獲取到的JSON格式資料解析成R的資料框架。`content`函數用於獲取HTTP回傳的內容，並指定內容格式為"text"，編碼方式為"utf-8"，以確保中文等非ASCII字符能正確顯示。

5.  要注意的欄位名稱包含sna（站台名稱）、tot（總車格數）、sbi（現有車數）與lat-lng的經緯度資料。


``` r
library(httr)
library(jsonlite)
url <- "https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json"
ubike.df <- fromJSON(content(GET(url),"text", encoding = "utf-8"))
head(ubike.df) %>% select(1:6)
```

``` output
##         sno                            sna  sarea                mday
## 1 500101001      YouBike2.0_捷運科技大樓站 大安區 2025-04-06 14:02:27
## 2 500101002 YouBike2.0_復興南路二段273號前 大安區 2025-04-06 13:50:29
## 3 500101003  YouBike2.0_國北教大實小東側門 大安區 2025-04-06 13:54:31
## 4 500101004        YouBike2.0_和平公園東側 大安區 2025-04-06 13:49:31
## 5 500101005  YouBike2.0_辛亥復興路口西北側 大安區 2025-04-06 14:02:27
## 6 500101006 YouBike2.0_復興南路二段280號前 大安區 2025-04-06 13:44:27
##                      ar    sareaen
## 1   復興南路二段235號前 Daan Dist.
## 2 復興南路二段273號西側 Daan Dist.
## 3   和平東路二段96巷7號 Daan Dist.
## 4 和平東路二段118巷33號 Daan Dist.
## 5     復興南路二段368號 Daan Dist.
## 6     復興南路二段280號 Daan Dist.
```

``` r
save(ubike.df, file="data/ubike.rda")
```

### Creating a new variable

-   為每個站產生一個`sbi`除以`tot`的新變數，來作為ubike站滿不滿的計量單位。
-   這段程式碼是在前一步的基礎上進一步創建新的 `Fullness` 欄位。該欄位的值根據 `ratio` 欄位的數值判斷，若 `ratio` 大於等於 0.9，則將 `Fullness` 設置為 "Full"；若 `ratio` 小於 0.3，則設置為 "Empty"；否則設置為 "Available"。

`if_else` 函數的用法如下：

``` r
if_else(condition, true_value, false_value)
```

-   `condition`：一個邏輯向量，用於指定條件判斷。
-   `true_value`：當 `condition` 為真時，返回的值。
-   `false_value`：當 `condition` 為假時，返回的值。

`if_else` 函數與 R 原生的 `ifelse` 函數相似，但有一些重要區別。`if_else` 函數在處理缺失值時更加嚴格，要求 `true_value` 和 `false_value` 具有相同的長度和類型，這有助於避免一些潛在的錯誤。


``` r
# ratio <- sbi/tot
load("data/ubike.rda")
ubike.df <- ubike.df %>%
  mutate(ratio = available_rent_bikes/total) %>%
  mutate(Fullness = if_else(ratio >= 0.9, "Full", if_else(ratio < 0.3, "Empty", "Available")))
```

### Mapping with sf


```r
library(ggplot2)
library(sf)

sf_tpe <-
  st_read(dsn = "data/111年9月行政區人口統計_鄉鎮市區_臺北市_SHP/", 
          layer = "111年9月行政區人口統計_鄉鎮市區", quiet = T) %>%
  mutate(across(where(is.character), ~iconv(., from = "BIG5", to = "UTF8"))) %>%
  mutate(across(where(is.double), ~if_else(is.na(.),as.double(0),.))) %>%
  st_set_crs(3826) %>% st_transform(4326) %>%
  filter(COUNTY == "臺北市")

sf_tpe %>% head()
```

```{.output}
## Simple feature collection with 6 features and 9 fields
## Geometry type: MULTIPOLYGON
## Dimension:     XY
## Bounding box:  xmin: 121.5041 ymin: 25.00776 xmax: 121.592 ymax: 25.09241
## Geodetic CRS:  WGS 84
##    TOWN_ID   TOWN COUNTY_ID COUNTY  H_CNT  P_CNT  M_CNT  F_CNT INFO_TIME
## 1 63000010 松山區     63000 臺北市  78977 187552  87625  99927   111Y09M
## 2 63000020 信義區     63000 臺北市  87407 201951  95884 106067   111Y09M
## 3 63000030 大安區     63000 臺北市 117243 280332 130596 149736   111Y09M
## 4 63000040 中山區     63000 臺北市  98825 210156  97114 113042   111Y09M
## 5 63000050 中正區     63000 臺北市  64491 146628  69663  76965   111Y09M
## 6 63000060 大同區     63000 臺北市  51988 118065  57003  61062   111Y09M
##                         geometry
## 1 MULTIPOLYGON (((121.572 25....
## 2 MULTIPOLYGON (((121.5727 25...
## 3 MULTIPOLYGON (((121.541 25....
## 4 MULTIPOLYGON (((121.5522 25...
## 5 MULTIPOLYGON (((121.5174 25...
## 6 MULTIPOLYGON (((121.5176 25...
```


``` r
library(sf)
library(ggplot2)
library(dplyr)

# 你已經有ubike.sf，這是一個sf物件
ubike.sf <- st_as_sf(ubike.df, coords = c("longitude", "latitude"), crs = 4326)

# 使用ggplot繪製地圖和位置點，並根據fullness的值上色
ggplot() + 
  geom_sf(data = sf_tpe) +  # 繪製sf_tpe區域
  geom_sf(data = ubike.sf, aes(color = Fullness, size=total), alpha=0.3) +
  scale_color_manual(values = c("Full" = "black", "Empty" = "red", "Available" = "blue")) + 
  theme_void()
```

<img src="V24_PointMap_files/figure-html/unnamed-chunk-5-1.png" width="100%" style="display: block; margin: auto;" />

### Using ggmap (Deprecated)

-   首先要設定google map的參數，先打開[google map](https://www.google.com.tw/maps)，縮放到你等一下希望看到的地圖底圖範圍後，複製新的網址列如[`https://www.google.com.tw/maps/@25.0353215,121.4916909,12.62z`](https://www.google.com.tw/maps/@25.0353215,121.4916909,12.62z)。這個網址後面的參數包含著經緯度（25.0353215,121.4916909）和地圖的縮放程度（12.62z）。因此你可以把他貼到`get_googlemap()`的函式中。除此之外，尚可以指定地圖種類，自行查詢help（maptype can be terrain, roadmap, satellite, hybrid, or toner-lite）。

-   接下來要用`geom_point()`這個`ggplot2`的函式繪製點圖，一共包含四個參數，用`data`指定資料集、用`aes()`指定x、y軸、用`colour`指定顏色、用`size`指定圓圈的大小。這邊我用圓圈的大小來表示總車格數，所以是`ubike.df$tot`，但如果是原始的`tot`數值可能畫起來會太大，這時後就必須要**自行反覆嘗試**，看看要取`log()`或者取`sqrt()`或者除以某個數值，好讓畫出來的點為適合觀看的大小。最後我多加了一個參數`alpha`讓圈圈半透明。


