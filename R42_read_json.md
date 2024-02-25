

# Read JSON {#read_json}

jsonlite套件提供了處理 JSON 格式資料的功能。


```r
# loading jsonlite package to parse a textual json file to an R object
library(jsonlite)
```

## Reading JSON

### JSON as a string

1.  **`fromJSON('{"a":1, "b":2}')`**。

    -   這是一個 JSON 物件，由一對大括號 **`{}`** 包圍。

    -   該物件中有兩個鍵值對，用逗號 **`,`** 分隔。

    -   第一個鍵值對中，鍵 **`a`** 對應的值為數字 1。

    -   第二個鍵值對中，鍵 **`b`** 對應的值為數字 2。

2.  **`fromJSON('[{"a":1, "b":2}, {"a":1, "b":3}]')`**

    -   這是一個 JSON 陣列，由一對中括號 **`[]`** 包圍。

    -   該陣列中有兩個元素，用逗號 **`,`** 分隔。

    -   每個元素都是一個 JSON 物件，由一對大括號 **`{}`** 包圍。

    -   第一個元素的 JSON 物件中，鍵 **`a`** 對應的值為數字 1，鍵 **`b`** 對應的值為數字 2。

    -   第二個元素的 JSON 物件中，鍵 **`a`** 對應的值為數字 1，鍵 **`b`** 對應的值為數字 3。


```r
library(jsonlite)
lst <- fromJSON('{"a":1, "b":2}')
class(lst) #list
```

```{.output}
## [1] "list"
```

```r
lst$a
```

```{.output}
## [1] 1
```

```r
fromJSON('[{"a":1, "b":2}, {"a":1, "b":3}]')
```

```{.output}
##   a b
## 1 1 2
## 2 1 3
```

```r
tbl <- fromJSON('[{"a":1, "b":2}, {"a":1, "b":3}, {"a":5, "b":7}]')
class(tbl) # a data.frame
```

```{.output}
## [1] "data.frame"
```

```r
tbl$a[1]
```

```{.output}
## [1] 1
```

### JSON as a local file

有時候JSON在編製的過程會記錄很多詮釋資料，所以不見得會把資料放在第一層，因此要嘗試去「trace」它才會知道資料在哪裡。例如以下範例是爬蟲爬回來的104職缺查詢結果，可猜想資料應放在如下結構中：

```         
{
  "data": {
    "list": [
      ...
    ]
  }
}
```


```r
library(jsonlite)
raw <- read_json("data/url_104.json")
raw$data$list[[1]]
```

```{.output}
## $jobType
## [1] "2"
## 
## $jobNo
## [1] "10788904"
## 
## $jobName
## [1] "約聘資料分析師(T45駐點食藥署)"
## 
## $jobNameSnippet
## [1] "約聘<em class='b-txt--highlight'>資料分析</em>師(T45駐點食藥署)"
## 
## $jobRole
## [1] "1"
## 
## $jobRo
## [1] "1"
## 
## $jobAddrNoDesc
## [1] "台北市南港區"
## 
## $jobAddress
## [1] ""
## 
## $description
## [1] "1.負責資料標準化、探勘及分析工作\r\n2.依客戶需求產出[[[資料分析]]]報告\r\n3.其他交辦工作"
## 
## $optionEdu
## [1] "大學"
## 
## $period
## [1] "00"
## 
## $periodDesc
## [1] "經歷不拘"
## 
## $applyCnt
## [1] "00005"
## 
## $applyDesc
## [1] "0~5人應徵"
## 
## $custNo
## [1] "97162640000"
## 
## $custName
## [1] "關貿網路股份有限公司"
## 
## $coIndustry
## [1] "1001001001"
## 
## $coIndustryDesc
## [1] "電腦系統整合服務業"
## 
## $salaryLow
## [1] "0033000"
## 
## $salaryHigh
## [1] "0050000"
## 
## $salaryDesc
## [1] "月薪 33,000~50,000元"
## 
## $s10
## [1] "50"
## 
## $appearDate
## [1] "20191004"
## 
## $appearDateDesc
## [1] "10/04"
## 
## $optionZone
## [1] "9703424"
## 
## $isApply
## [1] "0"
## 
## $applyDate
## [1] ""
## 
## $isSave
## [1] "0"
## 
## $descSnippet
## [1] "1.負責資料標準化、探勘及分析工作\r\n2.依客戶需求產出<em class='b-txt--highlight'>資料分析</em>報告\r\n3.其他交辦工作"
## 
## $tags
## $tags[[1]]
## [1] "上市上櫃"
## 
## $tags[[2]]
## [1] "員工560人"
## 
## 
## $link
## $link$applyAnalyze
## [1] "//www.104.com.tw/jobs/apply/analysis?j=64404a2d3a4c445c3738406932343d208466649725c4c4627272727273e34402b826j52&channel=104rpt&jobsource=apply_analyze"
## 
## $link$job
## [1] "//www.104.com.tw/job/6f8rs?jobsource=jolist_c_relevance"
## 
## $link$cust
## [1] "//www.104.com.tw/company/18mw4ku8?jobsource=jolist_c_relevance"
## 
## 
## $jobsource
## [1] "jolist_c_relevance"
## 
## $jobNameRaw
## [1] "約聘資料分析師(T45駐點食藥署)"
## 
## $custNameRaw
## [1] "關貿網路股份有限公司"
```

### JSON as a web file

當你使用手機上的 App 查看天氣、查詢公車路線、搜尋商品資訊時，你會注意到這些 App 看起來很不同，但它們都可以透過 Web API 來存取相同的資訊來源。簡單來說，Web API 就像是一種「資訊櫃檯」，提供存取資料的服務，它允許不同的應用程式通訊，以存取、分享、更新或刪除資料。透過 Web API，其他應用程式可以查詢、存取、下載、上傳、更新等操作資料，而不需要直接與資料庫或其他應用程式進行通訊。

舉例來說，一個網站可能有一個 Web API，它可以提供網站上所有文章的標題和內容。當其他網站或 App 需要獲取這些文章時，它們可以向該 Web API 發送請求，獲取所需資料。這樣就可以在不同的應用程式之間分享資料，提高效率和便利性。

Web API 和 JSON 之間的關係在於，Web API 通常會將回應資料以 JSON 的格式返回，以便接收方應用程式可以方便地解析和使用該資料。當一個應用程式需要從另一個應用程式獲取資料時，它可以透過 Web API 發送請求，並期望以 JSON 格式獲取回應。

如果該JSON是網頁伺服器所提供的Web API，以下程式碼示範了如何使用 R 語言中的 `httr` 和 `jsonlite` 套件，透過 HTTP `GET()` 請求獲取 Web API 所提供的 JSON 資料。以下是程式碼的解釋：

1.  **`GET("https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json")`** ：使用 httr 套件中的 **`GET`** 函數來發送 HTTP GET 請求，並取得 Web API 所提供的 JSON 資料。

2.  **`%>% content("text")`** ：**`content("text")`** 表示將 HTTP 回應的資料轉換為純文字格式，便於後續處理。當我們把該純文字格式打開後，會發現他以JSON格式書寫。

3.  **`%>% fromJSON()`** ：一旦確認回傳的訊息以JSON格式書寫，那麼就可以用`jsonlite`套件的`fromJSON()`將該文字字串轉為R的物件。


```r
library(httr)
library(jsonlite)

raw <- GET("https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json") %>%
    content("text") %>%
    fromJSON()
write_json(raw, "data/opendata_ubike_202304261243.json")
```

你可以讀取本書預先爬取好的json檔來觀察前述程式所爬回的json檔轉成R物件後的結果。


```r
raw <- fromJSON("data/opendata_ubike_202304261243.json")
raw$retVal[["0001"]]
```

```{.output}
## $sno
## [1] "0001"
## 
## $sna
## [1] "捷運市政府站(3號出口)"
## 
## $tot
## [1] "84"
## 
## $sbi
## [1] "72"
## 
## $sarea
## [1] "信義區"
## 
## $mday
## [1] "20221030161036"
## 
## $lat
## [1] "25.0408578889"
## 
## $lng
## [1] "121.567904444"
## 
## $ar
## [1] "忠孝東路/松仁路(東南側)"
## 
## $sareaen
## [1] "Xinyi Dist."
## 
## $snaen
## [1] "MRT Taipei City Hall Stataion(Exit 3)-2"
## 
## $aren
## [1] "The S.W. side of Road Zhongxiao East Road & Road Chung Yan."
## 
## $bemp
## [1] "12"
## 
## $act
## [1] "1"
```

::: practice
### Practice. Convert ubike json to data.frame

當你把ubike的資料給載回來並轉為R的物件後，你會發現該物件是一層又一層的named list，也就是每個`list`或每個值都有他的名字（name）。這是因為原本ubike的JSON格式是一層又一層的`dict`（Dictionary），全部是key-to-value的對應。這也是一種常見的Web API編法。

本練習希望你做的便是，希望你把這個好幾個階層的list，轉為一個以列（Rows）為每筆資料，欄（Columns）為變項的data.frame。
:::

## Case 1: Air-Quality (well-formatted )

前往 [https://data.gov.tw/dataset/40448](https://data.gov.tw/dataset/40448，點擊)對JSON 檔案按右鍵，然後複製連結，例如 "[https://data.epa.gov.tw/api/v2/aqx_p\_432?api_key=e8dd42e6-9b8b-43f8-991e-b3dee723a52d&limit=1000&sort=ImportDate%20desc&format=JSON"。](https://data.epa.gov.tw/api/v2/aqx_p_432?api_key=e8dd42e6-9b8b-43f8-991e-b3dee723a52d&limit=1000&sort=ImportDate%20desc&format=JSON%22。) (但是，連結地址，特別是 `api_key=9be7b239-557b-4c10-9775-78cadfc555e9`，每次都會更改。所以你必須要自己嘗試)。


```r
url <- "https://data.epa.gov.tw/api/v2/aqx_p_432?api_key=e8dd42e6-9b8b-43f8-991e-b3dee723a52d&limit=1000&sort=ImportDate%20desc&format=JSON"

df <- fromJSON(content(GET(url), "text", encoding = "utf-8"))
df %>% glimpse()
df$records %>% head() %>% knitr::kable(format = "html")
```

### Using knitr::kable() for better printing


```r
df$records %>% head() %>% knitr::kable(format = "html")
```

### Step-by-step: Parse JSON format string to R objects

`fromJSON(content(GET(url), "text", encoding = "utf-8"))`由內到外有三個函式。 \* `httr::GET()`按照指定的url發出GET request把網頁抓回來，如果是個合乎規定存取，就會順利取回該伺服器發的response。 \* `hrrt::content(response, "text", encoding = "utf-8")` 用`?content`查詢看看`content(response, "text")`的用途。其是把抓回來的檔案，轉為純文字的字串。content()是把抓回來的response解成純文字（JSON本身就是以純文字儲存，只是格式特別而已）。

`jsonlite::fromJSON()` 因為我們用眼睛看就知道他是個JSON格式的檔案，所以用`fromJSON()`這個函式，把用JSON格式編成的字串轉為R的物件，有可能是`data.frame`或`list`。`fromJSON()`預期會把JSON中`[]`的每一個項目轉為一筆筆的資料，然後把`{}`的pair當成column的變數名稱

#### `Step 1. GET()` 發送請求

向該URL的伺服器發送`GET()` request以取得該檔案。若成功取得，他會回覆一個[HTML status code](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Status)（你可上網查詢看看有哪些Status code）。如果成功的話就是2開頭的數字例如`200 OK`代表該伺服器接受該請求並開始傳回檔案。


```r
# Getting url back by GET()
response <- GET(url)

# Inspecting returned data
response
class(response)
```

(Tips) Using `?httr::GET` to inspect the function

#### `Step 2. httr::content()` 將回應資料的轉純文字

回應的資料看他的`class`是一個`response`，但如果看Global Environment看來是個`list`，裡面裝載很多資料，而主要核心的內容在`content`這個欄位，但看來是用`binary code`裝起來的，而不是純文字。

因此，對於這個抓回來的檔案，我需要用`httr::content()`幫忙把純文字給解出來。經查詢`help`可得知`content()`後面的參數有三類，其中可以要轉為純文字的就是`content(response, "text")`。因此偵測轉出來的變數會是長度為1的`character`。


```r
# Parsing to textual data by content()
text <- content(response, "text", encoding = "utf-8") 
nchar(text)
cat(text)
class(text)
length(text)
```

(Tips) using `??httr::content` to inspect the function

#### Step 3. `fromJSON()`: 將JSON格式文字轉為R物件

最後是將這個`character`轉為R的物件，也就是data.frame或list。注意，此時text是一個`character`，那是我們知道他是用JSON格式編寫的文字檔，就像我們知道.csv檔是用逗號分隔表示法依樣，JSON就是用層層疊疊的`[]{}`記號來表述資料的結構。

並要提醒初學者，`.json`或`.csv`都只是幫助程式初步篩選檔案的副檔名罷了，這兩種類型的檔案跟`.txt`檔一樣，都被歸屬為Win系統所謂的「純文字文件檔案」（就打開以後看得到文字的意思）。裡面的究竟是不是個完整的json檔這都要去看、去測。我自然也可以在`.json`的檔案裡偷偷亂用逗號分隔模式撰寫。


```r
df <- fromJSON(text)
dim(df)
glimpse(df)
?fromJSON
```

### Combining all

-   UVI Open data: <https://data.gov.tw/dataset/6076>
-   [https://data.epa.gov.tw/api/v2/uv_s\_01?api_key=e8dd42e6-9b8b-43f8-991e-b3dee723a52d&limit=1000&sort=publishtime desc&format=JSON](https://data.epa.gov.tw/api/v2/uv_s_01?api_key=e8dd42e6-9b8b-43f8-991e-b3dee723a52d&limit=1000&sort=publishtime%20desc&format=JSON)

#### Get from web api


```r
# specify data url
url <- "https://data.epa.gov.tw/api/v2/uv_s_01?api_key=e8dd42e6-9b8b-43f8-991e-b3dee723a52d&limit=1000&sort=publishtime%20desc&format=JSON"

# GET(), content(), then converting to R object by fromJSON()
raw <- fromJSON(content(GET(url), "text", encoding = "utf-8"))
```

#### Read from pre-saved JSON

你可以讀取本書預先爬取好的json檔來觀察前述程式所爬回的json檔轉成R物件後的結果。


```r
# You can read the pre-crawled JSON file  to observe the R object.
raw <- fromJSON("data/opendata_uvi_202304261215.json")

raw$records %>% head %>% knitr::kable()
```



|sitename |uvi  |publishagency |county |wgs84lon  |wgs84lat |publishtime      |
|:--------|:----|:-------------|:------|:---------|:--------|:----------------|
|宜蘭     |0.00 |中央氣象局    |宜蘭縣 |121,45,24 |24,45,50 |2023-04-26 00:00 |
|大武     |0.00 |中央氣象局    |臺東縣 |120,54,14 |22,21,20 |2023-04-26 00:00 |
|玉山     |0.00 |中央氣象局    |南投縣 |120,57,34 |23,29,15 |2023-04-26 00:00 |
|臺南     |0.00 |中央氣象局    |臺南市 |120,12,17 |22,59,36 |2023-04-26 00:00 |
|新竹     |0.01 |中央氣象局    |新竹縣 |121,00,51 |24,49,40 |2023-04-26 00:00 |
|日月潭   |0.00 |中央氣象局    |南投縣 |120,54,29 |23,52,53 |2023-04-26 00:00 |

預期結果

```         
sitename    uvi publishagency   county  wgs84lon    wgs84lat    publishtime
宜蘭  0.00    中央氣象局   宜蘭縣 121,45,24   24,45,50    2023-04-26 00:00
大武  0.00    中央氣象局   臺東縣 120,54,14   22,21,20    2023-04-26 00:00
玉山  0.00    中央氣象局   南投縣 120,57,34   23,29,15    2023-04-26 00:00
臺南  0.00    中央氣象局   臺南市 120,12,17   22,59,36    2023-04-26 00:00
新竹  0.01    中央氣象局   新竹縣 121,00,51   24,49,40    2023-04-26 00:00
日月潭 0.00    中央氣象局   南投縣 120,54,29   23,52,53    2023-04-26 00:00
```

::: practice
## **Practices: traversing json data**

下列這些網路文件應該都是json檔，請在以下的練習中，一個一個把他帶入把他抓回來看看。並用`str()`或`dplyr::glimpse()`觀察資料的內容。注意，如果你用了`View()`會沒辦法knit成html檔。又，每個不同的Web API存放的核心資料階層都不一樣，你必須要嘗試找到該多筆資料所在的data.frame。例如`url_cnyes`的資料便放在`raw$items$data`這樣的階層底下。

如果你無法順利撈取，你可以運用本書所預備的`url_cnyes.json`來做觀察json結構的練習。


```url_sc_flu
url_pchome <- "https://ecshweb.pchome.com.tw/search/v3.3/all/results?q=iphone&page=1&sort=rnk/dc"
url_ubike <- "https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json"
url_cnyes <- "https://news.cnyes.com/api/v3/news/category/headline?startAt=1588262400&endAt=1589212799&limit=30"


raw <- fromJSON(content(GET(url_cnyes), "text", encoding = "utf-8"))
# raw <- fromJSON("data/url_cnyes.json")
raw$items$data
```
:::

## Case 2: cnyes news (well-formatted)

第二類是最常會見到的例子，解出來的資料是個很多階層的`list`，通常一筆資料傳回來時多會附加一些metadata，比方說，一共幾筆資料、下一個資料區塊在哪裡，好讓使用者或者本地端的瀏覽器能夠繼續取得下一筆資料。因此，資料通常會在樹狀節點的某一個子節點。


```r
url_cnyes <- "https://news.cnyes.com/api/v3/news/category/headline?startAt=1588262400&endAt=1589212799&limit=30"

res <- fromJSON(content(GET(url_cnyes), "text", encoding = "utf-8"))
glimpse(res)
```


```r
raw <- fromJSON("data/url_cnyes.json")
df <- raw$items$data
head(df)
```

```{.output}
##    newsId                                                      title
## 1 4475846                〈財報〉疫情衝擊 Under Armour Q1營收大減23%
## 2 4475874 若一年內疫情未控制 避險基金大佬：美國將面臨第2次經濟大蕭條
## 3 4475873     〈美股早盤〉市場憂經濟重啟條件不成熟 道瓊早盤跌逾200點
## 4 4475465                   挽救國家財政危機 沙烏地撙節支出266億美元
## 5 4475875            長榮攜手海大開輪機工程專班 實習後正職月薪15萬元
## 6 4475855                         泰鼎-KY首季每股純益 1.5元 僅次健鼎
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   content
## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          &lt;p&gt;美國運動用品大廠 Under Armour (UAA-US) 於週一 (11 日) 盤前公佈 2020 財年第一季財報，受到新冠肺炎 (COVID-19) 疫情影響，導致零售業者被迫關閉店面，該公司於 Q1 營收及獲利雙雙不如市場預期。&lt;/p&gt;\n\n&lt;p&gt;受到財報表現不佳的影響，Under Armour 股價應聲下跌，於週一 (11 日) 台北時間晚上 10 時許，該公司股價下跌 10.97%，報每股 8.85 美元。自今年以來，Under Armour 股價已下跌了 54%。&lt;/p&gt;\n\n\n\n&lt;p&gt;&lt;em&gt;基於 non GAAP 財報關鍵數據:&lt;/em&gt;&lt;/p&gt;\n\n&lt;ul&gt;\n&lt;li&gt;營收：9.302 億美元，較去年同期的 12 億美元衰退，且低於市場預估值 9.49 億美元&lt;/li&gt;\n&lt;li&gt;稅後淨損：5.897 億美元，去年同期報稅後淨利 2250 萬美元&lt;/li&gt;\n&lt;li&gt;每股虧損：0.34 美元，低於市場預估值每股虧損 0.19 美元，去年同期每股盈餘報 0.05 美元&lt;/li&gt;\n&lt;/ul&gt;\n\n&lt;p&gt;由於疫情重創買氣，導致零售產業受到嚴重打擊，Under Armour CEO Patrik Frisk 表示，自 3 月中旬開始，疫情於北美地區快速蔓延，導致零售店面被迫關閉，並造成 Q1 營收大幅衰退 23%。&lt;/p&gt;\n\n&lt;p&gt;根據不同產品類別來看，Q1 服裝銷售下滑了 23%，報 5.98 億美元，鞋類銷售下滑 28%，報 2.1 億美元，配件銷售則下滑 17%，報 6800 萬美元。&lt;/p&gt;\n\n&lt;p&gt;根據不同銷售市場來看，Q1 北美營收下滑 28%，報 6.09 億美元，北美以外其他地區銷售則下滑 12%，報 2.87 億美元。北美地區的營收佔了 Under Armour 總營收的 65%。&lt;/p&gt;\n\n&lt;p&gt;此外，該公司表示，截至第一季為止，該公司帳上現金為 9.59 億美元。另外，預估今年的資本支出金額將達 1 億美元，低於較先前所預估的 1.6 億美元。&lt;/p&gt;\n\n&lt;p&gt;Under Armour 的營收成長，相當仰賴百貨公司等通路的銷售表現，但受到封鎖措施的影響，導致百貨公司被迫關閉。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;公司展望：&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;隨著美國宣佈實施經濟重啟計劃，包括梅西百貨 (M-US) 及 GAP(GPS-US) 皆規劃逐步重新恢復營業，但 Under Armour 卻於週一 (11 日) 表示，對於營業恢復的時程尚不確定。&lt;/p&gt;\n\n&lt;p&gt;此外，為了減輕疫情的衝擊，該公司計劃於 2020 年削減 3.25 億美元營運成本，包括暫時裁員部份零售店面的員工。由於疫情導致營運不確定性高，該公司已於上個月撤回了對於 2020 全年的財務預測。&lt;/p&gt;\n
## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            &lt;p&gt;美國知名避險基金經理人、Tudor Investment Corp. 創辦人 Paul Tudor Jones 週一 (11 日) 警告，若疫情在一年之內無法獲得控制，美國經濟將會陷入「第二次」經濟大蕭條。&lt;/p&gt;\n\n&lt;p&gt;Tudor Jones 表示：「若一年後我們依舊處於相同狀況，這將會形成第二次經濟大蕭條，這一切取決於封鎖措施是否解除。」&lt;/p&gt;\n\n&lt;p&gt;相較於中國、南韓等國在疫情控制及追蹤方面的努力，美國人對於自由的重視，正可能成為防疫過程中的一大阻礙，美國公衛專家即警告，若疫情尚未明顯趨緩即解封經濟，那麼可能帶來第二波的疫情蔓延。&lt;/p&gt;\n\n&lt;p&gt;Tudor Jones 說道：「與其他國家不同，我認為美國最大優勢在於個人主義，也就是對自由的熱愛，然而這在疫情之下卻是我們最大的弱點，看看在這方面取得成功的亞洲國家，他們的選擇顯示比起個人權利，他們更加重視社會價值觀。」&lt;/p&gt;\n\n&lt;p&gt;由於投資人認為經濟終將重啟，美股在今年 3 月創下史上最快步入熊市的紀錄後，正自低點迅速反彈，標普 500 指數已自疫情爆發後的底點反彈 30% 以上，且和紀錄高點僅相差 13.6%。&lt;/p&gt;\n\n\n\n&lt;p&gt;Tudor Jones 認為這波反彈相當容易預測，並強調未來走勢在很大程度上取決於疫情防控作業，重點將從流動性轉移到償債能力。&lt;/p&gt;\n\n&lt;p&gt;「若還是無法找到疫苗或治療方法，或者是更適合的大規模檢測方案，市場將會面臨更加艱難的時刻。」Tudor Jones 表示。&lt;/p&gt;\n
## 3 &lt;p&gt;自上週以來，由於各國政府開始逐步放寬封鎖措施，並重新開放經濟活動，帶動美股呈現上揚的趨勢。但是，近日來部份國家傳出新冠疫情再度惡化的消息，除了南韓新增確診病例快速攀升之外，日本及新加坡的新增病例亦呈增加趨勢，顯示疫情的狀況仍不穩定，導致投資人不安情緒升溫，並衝擊了市場對於經濟重啟的信心。&lt;/p&gt;\n\n&lt;p&gt;由於擔憂若過早重啟經濟，將可能冒著第二波疫情感染的風險，美股於週一 (11 日) 開盤 4 大指數皆下跌，另外，與疫情關聯性較高的產業，包括航空、零售、郵輪及賭場等，股價亦呈現下跌的趨勢，道瓊早盤下跌超過 200 點。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;新冠肺炎疫情&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;英國首相強生 (Boris Johnson) 於上週日 (10) 宣佈了解除英國封鎖並重啟經濟的「首批謹慎步驟」，呼籲英國各地的人們重返工作崗位。強生強調，不會立即結束封鎖，但自週三 (13 日) 起，將放鬆對人們行動的限制，從戶外體育運動時間不再設限開始，例如高爾夫和網球，並允許人們開車前往公園和海灘。&lt;/p&gt;\n\n&lt;p&gt;新冠疫情於西歐國家呈現放緩的跡象，但於俄羅斯卻日益嚴重，不僅連續數日的單日新增確診病例皆突破 1 萬例，且累計確診病例數已突破 20 萬例，並超越德國及法國，成為全球疫情第五嚴重的國家。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;中美貿易協議&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;美國智庫戰略及國際研究中心 (CSIS) 上週五 (8 日) 發佈報告預計，新冠疫情將導致今年中國對美採購量遠低第一階段貿易協議的要求。&lt;/p&gt;\n\n&lt;p&gt;美中矛盾因疫情衝擊而逐漸加劇，使外界對於第一階段貿易協議的現況感到疑慮，儘管兩國最高官員上週同意持續落實貿易協議，同時保持溝通協調，美國總統川普仍在上週五 (8 日) 表示，疫情衝擊大大改變他對於協議的觀感，對於是否終止協議感到相當掙扎。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;截至台北時間週一 (11 日)22 時許：&amp;nbsp;&lt;/em&gt;&lt;/p&gt;\n\n&lt;ul&gt;\n&lt;li&gt;道瓊指數下跌 241.98 點或 -0.99%，暫報 24089.34 點&lt;/li&gt;\n&lt;li&gt;那斯達克下跌 44.12 點或 -0.48%，暫報 9077.20 點&lt;/li&gt;\n&lt;li&gt;標普 500 下跌 24 點或 -0.82%，暫報 2905.80 點&lt;/li&gt;\n&lt;li&gt;費半下跌 15.59 點或 -0.88%，暫報 1760.02 點&lt;/li&gt;\n&lt;li&gt;台積電 ADR 下跌 0.64% 至每股 52.53 美元&lt;/li&gt;\n&lt;li&gt;十年期美債殖利率上漲至 0.686%&lt;/li&gt;\n&lt;li&gt; 紐約輕原油上漲 0.97% 至每桶 24.98 美元&lt;/li&gt;\n&lt;li&gt;布蘭特原油下跌 0.16% 至每桶 30.92 美元&lt;/li&gt;\n&lt;li&gt;黃金下跌 0.41% 至每盎司 1706.85 美元&lt;/li&gt;\n&lt;li&gt;美元指數上漲 0.31% 至 100.078 點 &amp;nbsp;&lt;/li&gt;\n&lt;/ul&gt;\n\n\n\n&lt;p&gt;&lt;em&gt;焦點個股：&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;特斯拉 (TSLA-US)&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;特斯拉早盤下跌 3.46%，達 790.68 美元。&lt;/p&gt;\n\n&lt;p&gt;根據中國乘用車市場信息聯席會於週一 (11 日) 報告指出，儘管 4 月份中國整體電動車市場呈現成長的趨勢，但特斯拉平價車款 Model 3 的銷售，卻較 3 月份暴跌了 64%，由 10160 輛減少至 3635 輛。此外，特斯拉在今年 1 月至 4 月，於中國的 Model 3 車款累計銷售量則達 19705 輛。&lt;/p&gt;\n\n&lt;p&gt;特斯拉執行長馬斯克 (Elon Musk) 於日前表示，為了使中國消費者能夠獲得電動車補貼的資格，已調降了 Model 3 於中國的銷售價格。此外，特斯拉亦致力於降低生產成本，因此，即使調降了銷售價格，該公司仍舊可以維持穩定的毛利率。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;蘋果 (AAPL-US)&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;蘋果早盤下跌 0.80%，達 307.61 美元。&amp;nbsp;&lt;/p&gt;\n\n&lt;p&gt;據報導指出，蘋果正計劃將該公司近五分之一的產能從中國轉移到印度，並通過富士康和緯創資通等合作製造商，擴大印度當地的生產業務，未來五年內投資規模可能升至 400 億美元，也可能使印度成為蘋果商品最大出口國。&lt;/p&gt;\n\n&lt;p&gt;數據顯示，蘋果的產品中有 90% 以上的產能來自中國。其中，iPhone 的代工生產很大一部分來自富士康與和碩。在 2015 年時，蘋果代工廠富士康就曾與印度達成備忘錄，同意加大對印度製造業的投資，並有計劃於 2020 年在印度建立 10 至 12 家製造消費電子產品的工廠。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;Under Armour (UAA-US)&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;Under Armour 早盤下跌 7.95%，達 9.15 美元。&lt;/p&gt;\n\n&lt;p&gt;美國運動用品大廠 Under Armour 於週一 (11 日) 盤前公佈 2020 財年第一季財報，受到疫情影響，導致零售業者被迫關閉店面，該公司於 Q1 營收衰退 23% 至 9.3 億美元。其中，服裝銷售下跌了 23%，鞋類銷售下滑 28%，配件銷售則下滑 17%。&lt;/p&gt;\n\n&lt;p&gt;此外，疫情亦造成 Under Armour 獲利能力大幅下降，Q1 稅後淨損報 5.89 億美元，每股損失 0.34 美元。去年同期稅後淨利則為 2250 萬美元，每股盈餘為 0.05 美元。&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;今日關鍵經濟數據：&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;無&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;&lt;em&gt;華爾街分析：&lt;/em&gt;&lt;/p&gt;\n\n&lt;p&gt;Vital Knowledge 創辦人 Adam Crisafulli 表示，全球經濟呈現逐步重啟的態勢，預期在接下來幾週，開放的速度將會加速，但儘管如此，目前 S&amp;amp;P 500 的估值仍屬被高估的狀態。&lt;/p&gt;\n\n&lt;p&gt;Principal Global Investors 經濟學家 Bob Baur 亦表示，全球疫情逐漸好轉的消息、以及對於經濟成長的預期，目前皆已被反映在股價的估值上。&lt;/p&gt;\n\n&lt;p&gt;穆迪首席經濟學家 Mark Zandi 警告，各州過早重新開放企業是一次豪賭，如果因此出現第二波感染潮，將引發經濟蕭條。Zandi 表示，市場認為 V 型復甦的可能性很高，而隨著美國企業重新開放，就業有望從 5 月最後一週開始反彈，如果不出現第二波感染，就業增長將從夏季持續至秋季初。&lt;/p&gt;\n
## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   &lt;p&gt;為挽救低油價及疫情帶來的國家財政危機，沙烏地阿拉伯週一 (11 日) 宣布自 6 月起停發生活津貼，同時將加值稅上調三倍，撙節支出總計 266 億美元。&lt;/p&gt;\n\n&lt;p&gt;受外在因素衝擊，財政部長賈丹 (Mohammed Al-Jadaan) 認為沙國正處於財政危機邊緣，宣布縮減「願景 2030」(Vision 2030) 經濟改革計劃內的部分措施，國家將自 6 月 1 日起停發生活津貼，並自 7 月 1 日起將加值稅上調三倍至 15％，總計削減規模達 266 億美元。&lt;/p&gt;\n\n&lt;p&gt;賈丹稱道：「肺炎疫情導致政府收入下滑，對公共財政形成的壓力來到難以應付的水平，將衝擊國家中長期宏觀經濟及公共財政，因此我們必須削減開支，並採取相關措施來支撐非石油收入的穩定。」&lt;/p&gt;\n\n&lt;p&gt;賈丹上週警告，沙國已數十年沒有經歷這種嚴重危機，強調該國必須大幅削減支出。根據統計，今年第一季沙國預算赤字約達 91 億美元，光是石油收入就減少 350 億美元，非石油收入則減少 170 億美元。&lt;/p&gt;\n\n&lt;p&gt;由於各國政府相繼頒布旅遊及封鎖禁令，沙國已受到油價暴跌和原油減產的深刻影響。今年 3 月，國際原油基準布蘭特原油暴跌逾 50%，導致沙國央行的淨海外資產每月損失 270 億美元，創下歷史紀錄，3 月外匯存底以 20 年幾年來最快速度下滑。&lt;/p&gt;\n\n\n\n&lt;p&gt;儘管高盛預計第二季石油收入將持續下滑，高盛經濟學家 Farouk Soussa 仍表示，預計沙國外匯存底損耗速度將逐漸放緩。&lt;/p&gt;\n
## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              &lt;p&gt;為擴大培育國籍海事專業人才，長榮海運 (2603-TW) 今 (11) 日宣布，與國立台灣海洋大學再度合作開辦學士後多元專長培力方案(輪機工程學系專班)，希望透過產業與學術密切合作，培養有志投身海勤工作的學員，並可在畢業後隨即就業，長榮指出，一旦正式成為長榮的海勤人員，月薪約 15 萬元。&lt;/p&gt;\n\n&lt;p&gt;長榮說明，此專班的招生對象為非輪機科系畢業的學員，只要具備學士資格，役畢或免服兵役者皆可報考，通過遴選後開始就讀，修業時間為三學期，長榮海運將全額補助學雜費和部分住宿費用，自 5 月 12 日起開放報名，預計 9 月正式開學。&lt;/p&gt;\n\n&lt;p&gt;而此方案是為配合長榮永續發展的專業人才需求，透過不同的管道來擴大培育優秀的國籍海事尖兵，並不影響海事科系學生到長榮實習和工作的機會。&lt;/p&gt;\n\n&lt;p&gt;長榮於 2017 年開始與海洋大學開辦第一期學士後輪機技術人才專班，該期學員已於去年畢業，並有超過 90% 的學員通過交通部的航海人員一等管輪測驗，陸續上船實習；部分學員已依規定累積一年的實習資歷，取得管輪適任證書，正式成為長榮的海勤人員，月薪約 15 萬元，未來隨著海勤年資與經驗增加，並通過相關測驗，可逐級晉升為輪機長，月薪將達 28-30 萬元的水準。&lt;/p&gt;\n\n&lt;p&gt;除了優渥的薪資外，長榮強調，貨櫃船隊提供舒適的住艙環境，並有圖書室、健身房、三溫暖及影音娛樂等設施，以及無線網路。&lt;/p&gt;\n
## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        &lt;p&gt;PCB 廠泰鼎 - KY(4927-TW) 今 (11) 日公布日首季稅後純益達 2.85 億元，每股純益為 1.5 元，在已公布 2020 年第一季財報的 PCB 廠中，僅次健鼎 (3044-TW) 的每股純益 1.86 元，第三名則是臻鼎 (4958-TW) 的 0.97 元。&lt;/p&gt;\n\n&lt;p&gt;泰鼎 2020 年首季營收 26.05 億元，毛利率 21%，季增 6 個百分點，年增 2 個百分點，稅後純益 2.85 億元，季增 5.06 倍、年增 11.8%，為單季次高，每股純益&amp;nbsp;1.5 元，也優於去年同期&amp;nbsp;1.43 元。&lt;/p&gt;\n\n&lt;p&gt;泰鼎 4 月營收 10.14 億元，月增 23.2%、年增 28.4%，為 9 個月新高，也創單月歷年同期新高; 累計今年前 4 月營收 36.19 億元，年減 0.5%。&lt;/p&gt;\n\n&lt;p&gt;展望第二季，泰鼎在 PC、家電、汽車、通訊四大區塊接單順利，預估營收相對第一季 26.05 億元，將雙位數成長。&lt;/p&gt;\n\n&lt;p&gt;泰鼎目前在泰國擁有每月 500 萬平方呎 PCB 產能，第 3 廠已經動工，全部完工後每月將再增加 180 萬平方呎新產能。&lt;/p&gt;\n
##   hasCoverPhoto isIndex
## 1             0       1
## 2             0       1
## 3             0       1
## 4             0       1
## 5             1       1
## 6             1       1
##                                                                                                                                                                                                   summary
## 1                                 Under Armour (UAA-US) 於週一 (11 日) 盤前公佈 2020 財年第一季財報，受到新冠肺炎 (COVID-19) 疫情影響，導致零售業者被迫關閉店面，該公司於 Q1 營收及獲利雙雙不如市場預期。
## 2                                            美國知名避險基金經理人、Tudor Investment Corp. 創辦人 Paul Tudor Jones 週一 (11 日) 警告，若疫情在一年之內無法獲得控制，美國經濟將陷入「第二次」經濟大蕭條。
## 3 由於擔憂若過早重啟經濟，將可能冒著第二波感染的風險，美股於週一 (11 日) 開盤 4 大指數皆下跌，另外，與疫情關聯性較高的產業，包括航空、零售、郵輪及賭場等，股價亦呈現下跌的趨勢。道瓊指數下跌超過 200 點。
## 4                                                               為挽救低油價及疫情帶來的國家財政危機，沙烏地阿拉伯週一 (11 日) 宣布自 6 月起停發生活津貼，同時將加值稅上調三倍，撙節支出總計 266 億美元。
## 5                                                                                                                                                       長榮攜手海大開輪機工程專班 實習後正職月薪 15 萬元
## 6                    PCB 廠泰鼎 - KY(4927-TW) 今 (11) 日公布日首季稅後純益達 2.85 億元，每股純益為 1.5 元，在已公布 2020 年第一季財報的 PCB 廠中，僅次於健鼎 (3044-TW) 的每股純益 1.86 元，而第三為臻鼎。
##   isCategoryHeadline video payment
## 1                  1             0
## 2                  1             0
## 3                  1             0
## 4                  1             0
## 5                  1             0
## 6                  1             0
##                                                                                                      otherProduct
## 1                                                  USS:UAA:STOCK:COMMON, USS:M:STOCK:COMMON, USS:GPS:STOCK:COMMON
## 2                                                                                                                
## 3 TWS:2330:STOCK:COMMON, USS:TSLA:STOCK:COMMON, USS:AAPL:STOCK:COMMON, USS:UAA:STOCK:COMMON, USS:TSM:STOCK:COMMON
## 4                                                                                                                
## 5                                                                                           TWS:2603:STOCK:COMMON
## 6                                             TWS:4927:STOCK:COMMON, TWS:3044:STOCK:COMMON, TWS:4958:STOCK:COMMON
##   isOutsource  publishAt
## 1           0 1589211902
## 2           0 1589209204
## 3           0 1589205268
## 4           0 1589204403
## 5           0 1589203838
## 6           0 1589203802
##                                                                     coverSrc.xs.src
## 1                                                                              <NA>
## 2                                                                              <NA>
## 3                                                                              <NA>
## 4                                                                              <NA>
## 5 https://cimg.cnyes.cool/prod/news/4475875/xs/8602ccaee38d54e5bb148e1cca3eb4bf.jpg
## 6 https://cimg.cnyes.cool/prod/news/4475855/xs/0e09778efa0a69a77710b8986c0d958a.jpg
##   coverSrc.xs.width coverSrc.xs.height
## 1                NA                 NA
## 2                NA                 NA
## 3                NA                 NA
## 4                NA                 NA
## 5               100                 56
## 6               100                 56
##                                                                     coverSrc.s.src
## 1                                                                             <NA>
## 2                                                                             <NA>
## 3                                                                             <NA>
## 4                                                                             <NA>
## 5 https://cimg.cnyes.cool/prod/news/4475875/s/8602ccaee38d54e5bb148e1cca3eb4bf.jpg
## 6 https://cimg.cnyes.cool/prod/news/4475855/s/0e09778efa0a69a77710b8986c0d958a.jpg
##   coverSrc.s.width coverSrc.s.height
## 1               NA                NA
## 2               NA                NA
## 3               NA                NA
## 4               NA                NA
## 5              180               101
## 6              180               101
##                                                                     coverSrc.m.src
## 1                                                                             <NA>
## 2                                                                             <NA>
## 3                                                                             <NA>
## 4                                                                             <NA>
## 5 https://cimg.cnyes.cool/prod/news/4475875/m/8602ccaee38d54e5bb148e1cca3eb4bf.jpg
## 6 https://cimg.cnyes.cool/prod/news/4475855/m/0e09778efa0a69a77710b8986c0d958a.jpg
##   coverSrc.m.width coverSrc.m.height
## 1               NA                NA
## 2               NA                NA
## 3               NA                NA
## 4               NA                NA
## 5              380               214
## 6              380               214
##                                                                     coverSrc.l.src
## 1                                                                             <NA>
## 2                                                                             <NA>
## 3                                                                             <NA>
## 4                                                                             <NA>
## 5 https://cimg.cnyes.cool/prod/news/4475875/l/8602ccaee38d54e5bb148e1cca3eb4bf.jpg
## 6 https://cimg.cnyes.cool/prod/news/4475855/l/0e09778efa0a69a77710b8986c0d958a.jpg
##   coverSrc.l.width coverSrc.l.height
## 1               NA                NA
## 2               NA                NA
## 3               NA                NA
## 4               NA                NA
## 5              640               360
## 6              640               360
##                                                                     coverSrc.xl.src
## 1                                                                              <NA>
## 2                                                                              <NA>
## 3                                                                              <NA>
## 4                                                                              <NA>
## 5 https://cimg.cnyes.cool/prod/news/4475875/xl/8602ccaee38d54e5bb148e1cca3eb4bf.jpg
## 6 https://cimg.cnyes.cool/prod/news/4475855/xl/0e09778efa0a69a77710b8986c0d958a.jpg
##   coverSrc.xl.width coverSrc.xl.height
## 1                NA                 NA
## 2                NA                 NA
## 3                NA                 NA
## 4                NA                 NA
## 5               960                539
## 6               960                540
##                                                                     coverSrc.xxl.src
## 1                                                                               <NA>
## 2                                                                               <NA>
## 3                                                                               <NA>
## 4                                                                               <NA>
## 5 https://cimg.cnyes.cool/prod/news/4475875/xxl/8602ccaee38d54e5bb148e1cca3eb4bf.jpg
## 6  https://cimg.cnyes.cool/prod/news/4475855/xl/0e09778efa0a69a77710b8986c0d958a.jpg
##   coverSrc.xxl.width coverSrc.xxl.height categoryId fundCategoryAbbr  etf
## 1                 NA                  NA        831             NULL NULL
## 2                 NA                  NA        831             NULL NULL
## 3                 NA                  NA        831             NULL NULL
## 4                 NA                  NA        833             NULL NULL
## 5               1080                 607        827             NULL NULL
## 6                960                 540        827             NULL NULL
##   fbShare fbComment fbCommentPluginCount
## 1       0         0                    0
## 2       0         0                    0
## 3       0         0                    0
## 4       0         0                    0
## 5       0         0                    0
## 6       0         0                    0
##                                                                                     market
## 1                                                                                     NULL
## 2                                                                                     NULL
## 3                                                             2330, 台積電, TWS:2330:STOCK
## 4                                                                                     NULL
## 5                                                               2603, 長榮, TWS:2603:STOCK
## 6 4927, 3044, 4958, 泰鼎-KY, 健鼎, 臻鼎-KY, TWS:4927:STOCK, TWS:3044:STOCK, TWS:4958:STOCK
##   source
## 1   <NA>
## 2   <NA>
## 3   <NA>
## 4   <NA>
## 5       
## 6
```

### (option) 取回資料並寫在硬碟

有時候寫爬蟲尤其是在爬會即時更新的資料時，會需要反覆定時地抓資料，這時候通常會先通通抓回來再慢慢合併整理。此時要特別注意如何保持每次抓回來的資料都是獨特的一個資料。以下面的例子來講，因為每次檔名都是一樣的，他會一直覆蓋過去，所以再怎麼抓，都不會是歷時性資料。通常會自動讀取當下時間當成檔名的一部分，這樣就不會重複了。這將在日後youbike的例子中用到。

```         
response <- GET(url_cnyes,
                write_disk("data/url_cnyes.json",
                           overwrite=TRUE))
```

## Case 3: footRumor (ill-formatted)

食品闢謠的例子可能是個沒好好編過JSON的單位所編出來的案子，資料很簡單，但卻是一個list裡面有329個data.frame，且每個data.frame只有對腳現有資料，然後每一筆資料就一個data.frame。


```r
url <- 'http://data.fda.gov.tw/cacheData/159_3.json'
safefood <- fromJSON(content(GET(url),'text'))
# write_json(safefood, "data/opendata_safefood_202304261256.json")
```

為了避免產製本書時出錯，本程式採用預備好的資料來練習。但仍建議你用前述方式自網路上爬取資料下來練習。


```r
safefood <- fromJSON("data/opendata_safefood_202304261256.json")
# str(safefood)
class(safefood)
```

```{.output}
## [1] "list"
```

```r
class(safefood[[1]])
```

```{.output}
## [1] "data.frame"
```

```r
dim(safefood[[1]])
```

```{.output}
## [1] 5 5
```

```r
# View(safefood[[1]])
# View(safefood)
# print(content(GET(url), "text"))
```

### 處理非典型的JSON檔

-   但這時候也不難觀察到其規律性。既然每個data.frame是一筆資料，且資料都是照順序出現在對角線，那我就把data.frame給`unlist()`拆成vector後，把`NA`給移除了，那剩下的就是我們要的資料了。

-   但，由於對整筆資料`unlist()`，那整筆資料會變成一個很長的vector，不過我們知道每五個元素就是一筆資料。所以我可以嘗試用`matrix()`的指令，讓資料每五個就折成一筆資料。

-   程序大致上是

    1.  `safefood.v <- unlist(safefood)` 把資料`unlist()`。
    2.  `safefood.v <- safefood.v[!is.na(safefood.v)]`剔除NA值
    3.  `safefood.m <- matrix(safefood.v, byrow = T, ncol = 5)`照列來折，因為每五個就一筆資料，所以是照列折，然後用`ncol = 5`來指定五個一折。


```r
# unlist data structure to a list
safefood.v <- unlist(safefood)
head(safefood.v)
```

```{.output}
## 分類1 分類2 分類3 分類4 分類5 標題1 
##    ""    NA    NA    NA    NA    NA
```

```r
# anyNA() to check if NAs still exist
anyNA(safefood.v)
```

```{.output}
## [1] TRUE
```

```r
# (option) check if NAs exist
sum(is.na(safefood.v))
```

```{.output}
## [1] 10720
```

```r
# remove NAs
safefood.v <- safefood.v[!is.na(safefood.v)]
# length(safefood.v)

# double-check NAs
anyNA(safefood.v)
```

```{.output}
## [1] FALSE
```

```r
# head(safefood.v)


# convert vector to matrix
safefood.m <- matrix(safefood.v, byrow = T, ncol = 5)
# ?matrix

# convert matrix to dataframe
safefood.df <- as.data.frame(safefood.m)

# delete the 4th column
safefood.df <- safefood.df[-4]

# naming the data.frame
names(safefood.df) <- c('category', 'question', 'answer', 'timestamp')
head(safefood.df)
```

```{.output}
##   category                                         question
## 1           使用含有acetaminophen成分藥品會傷腎，是真的嗎？
## 2                          使用退燒藥會延誤治療，是真的嗎？
## 3             美國全面禁止2歲以下幼兒使用感冒藥及退燒藥嗎？
## 4                  凡士林(Petrolatum)會致癌，請問是真的嗎？
## 5          市售護唇膏多含石蠟，長期使用可能致癌，是真的嗎？
## 6                        含藥化粧品比一般化粧品效果更好嗎？
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              answer
## 1            解答： Acetaminophen 是一種中樞作用型解熱鎮痛藥品，在正常健康狀況下，使用正常的建議劑量（成人每日不超過4000 毫克，兒童每日每公斤不超過50 毫克，分多次服用，每次至少間隔4 小時），並不會引起腎毒性。有少數報告顯示當急性過量使用時，會導致腎功能不良，但多為可逆性，如果慢性大量使用，才有嚴重腎臟傷害的可能性。 根據國外報告，成人長期持續使用1 年以上，而且累積劑量超過1 公斤（以一般每顆500 毫克的成人劑型而言，相當於2000 顆）時，可能導致嚴重腎臟傷害。此外，過量使用acetaminophen 或併服酒精性飲料時，具有肝臟損傷之風險。因此使用該成分藥品，應依照仿單或醫師指示使用，且避免長期持續使用或同時飲用酒精。 本文案為食藥署與臺灣兒科醫學會共同撰文
## 2                                                                                                                                                                                                                                                                                                        解答： (1)發燒為一種症狀，並非病因，瞭解引起發燒之原因遠比退燒重要。因此，出現發燒症狀時，應立即就醫治療，以瞭解導致發燒的原因，切勿自行購買退燒藥使用，以免延誤疾病之治療。 (2)退燒藥之使用，主要目的是減緩因發燒出現之不適感。當發燒超過38.5° C且有不舒服的感覺，可適當給予退燒藥，但不一定要讓體溫降到正常範圍。 本文案為食藥署與臺灣兒科醫學會共同撰文
## 3 解答： 美國食品藥物管理局並未完全禁止2 歲以下幼兒使用退燒藥或感冒藥，而是強調應在醫師指示下使用。美國分析兒童使用感冒藥產生的嚴重不良反應，主要是因為美國民眾可於藥局自行購買成藥，家長因為沒有劑量觀念，可能導致藥物過量而使幼兒發生危險。我國對於該類藥品之使用建議與國外一致，且中文仿單已註明「幼兒應洽醫師診治，不宜自行使用」，強調感冒藥必須經由專業醫師診治後，方能用於2 歲以下幼兒。 幼兒的生理發展尚未健全，必須特別注意藥物劑量。因此，無論中藥或西藥，幼兒使用藥品一定要經熟悉兒科專業的醫師評估其臨床效益及風險，並依幼兒之體重年齡處方適當劑量。家長依循專業醫師處方使用藥物，比較不會發生腎毒性等不良反應。 本文案為食藥署與臺灣兒科醫學會共同撰文
## 4                                                                                                                                                                                                                                                                                                                                                         解答： 凡士林(Petrolatum) 成分常被用於髮蠟、髮乳、潤膚霜、冷霜、唇膏、乳液等化粧品中，該成分安定性佳且歐盟、美國、日本等國的化粧品均未禁止使用凡士林成分，更可用於食品及口服藥品中，做為油性鎖水保濕劑使用。 提醒大家不要被謠言所誤導，選擇適合自己的產品，瞭解成分的用途與使用方式，才是保養身體的關鍵。
## 5                                                                                                                                                                                                                                                                                                                                                                  解答： 石蠟是護唇膏、各種保濕保養化粧品的常見的成分，目前國際間均允許該成分可添加於化粧品中。化粧品中使用之石蠟已經精製，而精製過後的石蠟安定性佳，更可用於食品及口服藥品中。 食藥署提醒大家，不要被謠言所誤導，選擇標示完整及適合自己的產品，瞭解成分的用途與使用方式，才是保障自身權益的關鍵。
## 6                                                                                                                   解答： (1)我國所謂的「含藥化粧品」，實際上為含有衛生福利部公告的「化粧品含有醫療或毒劇藥品基準」成分之化粧品，主要針對添加前述基準成分之染髮劑、燙髮劑、止汗制臭劑等產品加強安全性管理，與化粧品的效果無關。 (2)目前含藥化粧品依規定需於進口或上市前辦理查驗登記，取得許可證後始得輸入、製造及販售。 (3)民眾在選購含藥化粧品前，可先上食藥署首頁>業務專區>化粧品>含藥化粧品許可證查詢作業，查詢該含藥化粧品是否已取得許可證，才可安心購買。 相關連結： 西藥、醫療器材、含藥化粧品許可證查詢作業http://www.fda.gov.tw/mlms/H0001.aspx (另開視窗)
##            timestamp
## 1 06 25 2015 12:00AM
## 2 06 25 2015 12:00AM
## 3 06 25 2015 12:00AM
## 4 06 25 2015 12:00AM
## 5 06 25 2015 12:00AM
## 6 06 25 2015 12:00AM
```

::: notes
## Reviewing JSON

### Type I: Well-formatted JSON: UVI, AQI, Hospital_revisits

這類的資料以典型的[{}, {}, {}]形式儲存，以以下方式就可直接轉為data.frame `df <- fromJSON(content(GET(url), "text"))`

### Type II: hierarchical JSON: rent591, facebook graph api, google map

這類的json資料為well-formatted，但要的資料儲存在比較深的階層中，代表其並非簡單地二維表格，還有其他更多的詮釋資料被擺在同一個JSON檔案中。解決策略：通常`fromJSON()`轉完後為list，逐一就variable names查看資料在哪裡。`View(res$data$data)`

### Type III: Ill-formatted JSON: food_rumors, ubike

這類的資料並非以典型的`[{}, {}, {}]`形式儲存，但仍是有序的二維數據。可將資料`unlist()`攤開，然後去除不必要的NA後，按欄位數目重建Matrix再轉回data.frame

解決策略：用`as.data.frame()`或`unlist()`硬轉成data.frame或vector來看資料的出現是否有所規律。
:::

## 
