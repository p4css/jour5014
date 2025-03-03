

## Paid Maternity Leave {#maternity}

本案例將使用R重新製作華盛頓郵報2016年8月13日的一篇報導，該報導探討了美國婦女產假支薪情況。案例中將應用data.frame和基本的繪圖與資料摘要方法。

原始新聞來源：[The world is getting better at paid maternity leave. The U.S. is not. - The Washington Post](https://www.washingtonpost.com/news/worldviews/wp/2016/08/13/the-world-is-getting-better-at-paid-maternity-leave-the-u-s-is-not/)。該篇報導提及，美國因為目前的政策不保障帶薪產假，許多女性感到必須在工作和照顧家庭之間做出選擇，這種性別不平等破壞了她們在工作機會上的平等機會。同時，世界各地的婦女待遇正在逐漸改善。至少190個國家對嬰兒的母親規定了某種形式的帶薪假期，產假待遇在52個國家有所提高。專家表示，現在美國城市和州正通過不同形式的帶薪家庭假法案，這顯示美國雇主正在展示有競爭力的福利不會影響員工表現。記者選定了「美國沒有產假支薪」作為新聞切入點。在呈現的時候，就必須要盡可能地凸顯這樣的情形。一般來說，會繪製世界地圖且用顏色來凸顯美國是目前少數沒有產假支薪的國家之一如下：

![](https://www.washingtonpost.com/blogs/worldviews/files/2016/08/matleave-map.jpg)

### The Data

該筆資料來自位於UCLA公衛學院的[World Policy Analysis Center](http://www.worldpolicycenter.org/)，其包含兩個壓縮檔，分別為橫斷式資料與部分資料的縱貫性資料。就產假支薪而言，其調查了198個國家的政策，將支薪週數分為五個等級，並蒐錄了1995年至2013年間的變化（matleave_95\~matleave13）。通常在資料分析時，應先閱讀資料了解資料。首先要先觀察資料，開啟資料後概略如下：

![](images/clipboard-265029472.png)

打開資料後便先觀察資料的「列（Row）」是什麼？又有哪些「欄（Column）」？通常在列上的會是逐筆的觀察值，如前述資料每一筆是個國家，而欄上面則是每個觀察所觀察到的變項，例如國名縮寫（iso2、iso3）、地區（region）、經濟等級（wb_econ）、和本案例所要觀察的產假支付等級。但產假支付等級可以看從matleave_95到matleave_13共19欄是19年的資料，數值為1到5，空白的通常是該年沒記錄到的缺漏值。通常統計資料如果是用等第來紀錄的話，會給編碼表，也就是1到5分別代表哪種等級、哪種程度。這份資料也有給編碼表如下，產假給付程度在該調查中分為五個等級，包含**1**（0週）、**2**（0-14週）、**3**（14-25週）、**4**（26-51週）、**5**（52週以上）等。

![](images/clipboard-3879755206.png){width="425"}

### Visual Strategies

但如果只是繪製地圖，卻又略顯單薄。而該筆來自[Word Policy Analysis Center](https://www.worldpolicycenter.org/)資料其實含有自1995年至2003年共19年的資料（本案例即就是下載該中心所分享的調查資料，不用申請帳號）。於是該專題的作者便利用這些歷史資料，進一步凸顯美國在這方面一直沒有改變，始終都是保持0週，即使其他各國逐漸在調高產假的給付程度。但要處理197個國家的在19年間的變化相當不易。若為每年繪製一張世界地圖，然後以動畫或動態卷軸來凸顯這19年間美國的變化，也會因為國家數過多而難以聚焦在作者想突顯的美國。

而這便是作者在視覺化上相當具有巧思的地方。作者便從給付程度最高的層級開始做長條圖，共五個階層的子圖。而每個階層的子圖，作者又將該層級的圖分為「保持不變（Stay Same）」和「持續增加（Increase）」兩組。經過這樣的分組，會得到9個子圖。分別為等級5（保持不變、持續增加）、等級4（保持不變、持續增加）、...、等級1（保持不變）。讀者在看的時候，會依次看到給付程度最高到最低的國家，也可以看到哪些國家在這19年間制度有所變化（通常是增加）。但看到最後的時候，便會看到美國的情形，即是無產假給付。

![](images/clipboard-1939265210.png){width="441"}

### Cleaning

#### Read .xlsx

在進行產假支薪調查數據的分析與視覺化時，我們從該調查網站上所下載的資料是一個Excel文件。由於R語言本身不直接支援讀取Excel格式的文件，我們必須依靠外部的套件來實現這一功能，如**`readxl`**套件。它是專門設計來讀取**`.xls`**和**`.xlsx`**格式文件的強大工具。**`readxl`**套件是**`tidyverse`**套件集的一部分。**`tidyverse`**是一組旨在數據科學和數據處理領域提供便利的R套件集合，包括了**`ggplot2`**、**`dplyr`**、**`tidyr`**等多個流行的套件。如果你之前已經安裝了**`tidyverse`**，那麼**`readxl`**套件應該也已經安裝在你的系統上，無需進行重複安裝。

然而，即便**`readxl`**已經安裝，它並不會隨著**`tidyverse`**套件集的其他部分自動加載到R的執行環境中。這意味著，在你打算使用**`readxl`**套件來讀取Excel文件之前，需要先手動執行**`library(readxl)`**命令來加載它。


```r
# Import readxl package
# install.packages("tidyverse")
library(readxl)
```

這段程式碼使用`read_excel()`函式從`data`資料夾中的`WORLD-MACHE_Gender_6.8.15.xls`檔案中的`Sheet1`工作表讀取資料。其中`col_names=T`為該函式的參數，表示第一列為欄位名稱。讀取後的資料會被Assign給變數`df`。


```r
# Use read_excel() to convert excel sheet to data.frame
df <- read_excel("data/WORLD-MACHE_Gender_6.8.15.xls", "Sheet1", col_names=T)
```

#### Preview data

Preview data by `View()`, `class()`, `dim()`, `str()`, `summary()` and `names()`


```r
# View(df)
class(df)		# [1] "tbl_df"     "tbl"        "data.frame"
```

```{.output}
## [1] "tbl_df"     "tbl"        "data.frame"
```

```r
dim(df)
```

```{.output}
## [1] 197 156
```

```r
# Show names of variables (vectors, columns) by names()
names(df)
```

```{.output}
##   [1] "country"           "iso2"              "iso3"             
##   [4] "region"            "wb_econ"           "matleave_95"      
##   [7] "matleave_96"       "matleave_97"       "matleave_98"      
##  [10] "matleave_99"       "matleave_00"       "matleave_01"      
##  [13] "matleave_02"       "matleave_03"       "matleave_04"      
##  [16] "matleave_05"       "matleave_06"       "matleave_07"      
##  [19] "matleave_08"       "matleave_09"       "matleave_10"      
##  [22] "matleave_11"       "matleave_12"       "matleave_13"      
##  [25] "matleave_wrr_95"   "matleave_wrr_96"   "matleave_wrr_97"  
##  [28] "matleave_wrr_98"   "matleave_wrr_99"   "matleave_wrr_00"  
##  [31] "matleave_wrr_01"   "matleave_wrr_02"   "matleave_wrr_03"  
##  [34] "matleave_wrr_04"   "matleave_wrr_05"   "matleave_wrr_06"  
##  [37] "matleave_wrr_07"   "matleave_wrr_08"   "matleave_wrr_09"  
##  [40] "matleave_wrr_10"   "matleave_wrr_11"   "matleave_wrr_12"  
##  [43] "matleave_wrr_13"   "bf_dur_95"         "bf_dur_96"        
##  [46] "bf_dur_97"         "bf_dur_98"         "bf_dur_99"        
##  [49] "bf_dur_00"         "bf_dur_01"         "bf_dur_02"        
##  [52] "bf_dur_03"         "bf_dur_04"         "bf_dur_05"        
##  [55] "bf_dur_06"         "bf_dur_07"         "bf_dur_08"        
##  [58] "bf_dur_09"         "bf_dur_10"         "bf_dur_11"        
##  [61] "bf_dur_12"         "bf_dur_13"         "mat_bfeed_6mon_95"
##  [64] "mat_bfeed_6mon_96" "mat_bfeed_6mon_97" "mat_bfeed_6mon_98"
##  [67] "mat_bfeed_6mon_99" "mat_bfeed_6mon_00" "mat_bfeed_6mon_01"
##  [70] "mat_bfeed_6mon_02" "mat_bfeed_6mon_03" "mat_bfeed_6mon_04"
##  [73] "mat_bfeed_6mon_05" "mat_bfeed_6mon_06" "mat_bfeed_6mon_07"
##  [76] "mat_bfeed_6mon_08" "mat_bfeed_6mon_09" "mat_bfeed_6mon_10"
##  [79] "mat_bfeed_6mon_11" "mat_bfeed_6mon_12" "mat_bfeed_6mon_13"
##  [82] "minage_fem_leg_95" "minage_fem_leg_96" "minage_fem_leg_97"
##  [85] "minage_fem_leg_98" "minage_fem_leg_99" "minage_fem_leg_00"
##  [88] "minage_fem_leg_01" "minage_fem_leg_02" "minage_fem_leg_03"
##  [91] "minage_fem_leg_04" "minage_fem_leg_05" "minage_fem_leg_06"
##  [94] "minage_fem_leg_07" "minage_fem_leg_08" "minage_fem_leg_09"
##  [97] "minage_fem_leg_10" "minage_fem_leg_11" "minage_fem_leg_12"
## [100] "legal_diff_leg_95" "legal_diff_leg_96" "legal_diff_leg_97"
## [103] "legal_diff_leg_98" "legal_diff_leg_99" "legal_diff_leg_00"
## [106] "legal_diff_leg_01" "legal_diff_leg_02" "legal_diff_leg_03"
## [109] "legal_diff_leg_04" "legal_diff_leg_05" "legal_diff_leg_06"
## [112] "legal_diff_leg_07" "legal_diff_leg_08" "legal_diff_leg_09"
## [115] "legal_diff_leg_10" "legal_diff_leg_11" "legal_diff_leg_12"
## [118] "minage_fem_pc_95"  "minage_fem_pc_96"  "minage_fem_pc_97" 
## [121] "minage_fem_pc_98"  "minage_fem_pc_99"  "minage_fem_pc_00" 
## [124] "minage_fem_pc_01"  "minage_fem_pc_02"  "minage_fem_pc_03" 
## [127] "minage_fem_pc_04"  "minage_fem_pc_05"  "minage_fem_pc_06" 
## [130] "minage_fem_pc_07"  "minage_fem_pc_08"  "minage_fem_pc_09" 
## [133] "minage_fem_pc_10"  "minage_fem_pc_11"  "minage_fem_pc_12" 
## [136] "legal_diff_pc_95"  "legal_diff_pc_96"  "legal_diff_pc_97" 
## [139] "legal_diff_pc_98"  "legal_diff_pc_99"  "legal_diff_pc_00" 
## [142] "legal_diff_pc_01"  "legal_diff_pc_02"  "legal_diff_pc_03" 
## [145] "legal_diff_pc_04"  "legal_diff_pc_05"  "legal_diff_pc_06" 
## [148] "legal_diff_pc_07"  "legal_diff_pc_08"  "legal_diff_pc_09" 
## [151] "legal_diff_pc_10"  "legal_diff_pc_11"  "legal_diff_pc_12" 
## [154] "minwage_ppp_2013"  "mw_overtime"       "oecd"
```

#### Select variables

由於所需要的資料為第三欄的變數`iso3`（為國家代碼）和第六至24欄的`matleave95`\~`matleave13`共29年的資料，所以需要在`df[ , ]`中選出這幾欄。只要把所要取的欄以vector的型態放在`df[row,col]`之`col`的位置，便可以選出所要的欄。程式碼`c(3, 6:24)`代表取出`df`的第3欄、第6至24欄。最後，我們將取出來的欄位指給一個新的變數`matleave`。


```r
# Select the 3rd and 6th to 24th columns
matleave <- df[ , c(3, 6:24)]

# Use class(), dim(), and str() to inspect the data
class(matleave)
```

```{.output}
## [1] "tbl_df"     "tbl"        "data.frame"
```

```r
dim(matleave)
```

```{.output}
## [1] 197  20
```

```r
str(matleave)
```

```{.output}
## tibble [197 × 20] (S3: tbl_df/tbl/data.frame)
##  $ iso3       : chr [1:197] "AFG" "ALB" "DZA" "AND" ...
##  $ matleave_95: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_96: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_97: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_98: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_99: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_00: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_01: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_02: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_03: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_04: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_05: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_06: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_07: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_08: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_09: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_10: num [1:197] 2 5 3 3 2 2 2 5 NA 5 ...
##  $ matleave_11: num [1:197] 2 5 3 3 2 2 2 5 3 5 ...
##  $ matleave_12: num [1:197] 2 5 3 3 2 2 2 5 3 5 ...
##  $ matleave_13: num [1:197] 2 5 3 3 2 2 2 5 3 5 ...
```

#### Check & Replace NAs

-   處理開放資料常常會遇到紀錄遺漏的情形，這些遺漏的值在R語言中通常以**`NA`**（Not Available）來表示。這種情況很常見，特別是當數據來自於廣泛的來源，如網絡調查或公開資料庫時。適當處理這些**`NA`**值對於維持分析的準確性和可靠性至關重要。
-   為了識別和處理這些**`NA`**值，R提供了一些有用的函數和技巧。例如，**`is.na(v)`**函數可以用來檢測向量**`v`**中的**`NA`**值。如果你想選擇所有的**`NA`**紀錄，可以使用**`v[is.na(v)]`**這樣的語法。這個表達式會傳回所有在向量**`v`**中為**`NA`**的元素，這對於進一步的分析和資料清洗非常有幫助。
-   在某些情況下，你可能會想要以某個特定值來取代**`NA`**值，以避免在繪圖或進行其他數據分析時產生錯誤。例如，你可以選擇以0來取代所有的**`NA`**值，這可以通過**`v[is.na(v)] <- 0`**來實現。這樣，所有原本為**`NA`**的資料格都會被賦予0值。
-   此外，**`sum(is.na(v))`**這個表達式可以用來檢測向量**`v`**中還有多少**`NA`**值。這個函數的運作機制是計算所有**`is.na(v)`**為**`TRUE`**的情況，即所有**`NA`**值的總數。如果這個結果不是0，那麼就表示在向量或dataframe中還存在**`NA`**值。這對於確保數據清理工作已經完成，並且數據集準備好進行分析是非常有用的。


```r
# is.na() to indicate each element is NA or NOT(TRUE/FALSE)
head(is.na(matleave), n=20)
```

```{.output}
##        iso3 matleave_95 matleave_96 matleave_97 matleave_98 matleave_99
##  [1,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [2,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [3,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [4,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [5,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [6,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [7,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [8,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [9,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [10,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [11,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [12,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [13,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [14,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [15,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [16,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [17,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [18,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [19,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [20,] FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##       matleave_00 matleave_01 matleave_02 matleave_03 matleave_04 matleave_05
##  [1,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [2,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [3,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [4,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [5,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [6,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [7,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [8,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [9,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [10,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [11,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [12,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [13,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [14,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [15,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [16,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [17,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [18,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [19,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [20,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##       matleave_06 matleave_07 matleave_08 matleave_09 matleave_10 matleave_11
##  [1,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [2,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [3,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [4,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [5,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [6,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [7,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [8,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
##  [9,]       FALSE       FALSE       FALSE       FALSE        TRUE       FALSE
## [10,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [11,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [12,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [13,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [14,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [15,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [16,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [17,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [18,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [19,]       FALSE       FALSE       FALSE       FALSE       FALSE       FALSE
## [20,]       FALSE        TRUE        TRUE       FALSE       FALSE       FALSE
##       matleave_12 matleave_13
##  [1,]       FALSE       FALSE
##  [2,]       FALSE       FALSE
##  [3,]       FALSE       FALSE
##  [4,]       FALSE       FALSE
##  [5,]       FALSE       FALSE
##  [6,]       FALSE       FALSE
##  [7,]       FALSE       FALSE
##  [8,]       FALSE       FALSE
##  [9,]       FALSE       FALSE
## [10,]       FALSE       FALSE
## [11,]       FALSE       FALSE
## [12,]       FALSE       FALSE
## [13,]       FALSE       FALSE
## [14,]       FALSE       FALSE
## [15,]       FALSE       FALSE
## [16,]       FALSE       FALSE
## [17,]       FALSE       FALSE
## [18,]       FALSE       FALSE
## [19,]       FALSE       FALSE
## [20,]       FALSE       FALSE
```

```r
# Assign 0 to those NA data
matleave[is.na(matleave)] <- 0

# anyNA() to check if there are still NA cells.
anyNA(matleave)
```

```{.output}
## [1] FALSE
```

```r
# sum(is.na()) to count the number of NA
sum(is.na(matleave))
```

```{.output}
## [1] 0
```

#### Filtering data

這個階段我希望篩選出2013年（matleave_13）給付等級為5，而1995年（matleave_95）也是5的國家。也就是抽取出有資料記錄以來給付程度就都是最高等級的那群國家，注意，這邊是根據「欄」的資料數值來篩選出合乎條件的「列」。

`matleave[matleave$'matleave_13'==5, ]`中的第一個`matleave`表示要篩選的資料集，中括號中的`matleave$'matleave_13'==5`是篩選條件，表示將篩選`matleave`資料集中的`matleave_13`變數值等於`5`的列；但我同時要滿足第二個條件`matleave$'matleave_95'`，因此要用一個AND符號\<span color="red"\>`&`\</span\>表示兩個條件必須同時成立。中括號中的逗號後方未有欄位名稱表示將保留所有欄位（變項），僅篩選出符合條件的列，並將篩選後所產生的dataframe指給變數`m55`。


``` r
# Use logical comparison to see if the last year equals to 5
# Assign matching data to var m5
# filter rows by condition
m55 <- matleave[matleave$'matleave_13' == 5 &
                  matleave$'matleave_95' == 5, ]

# nrow() to count matching data
nrow(m55)
```

``` output
## [1] 18
```

``` r
# Is it possible to use length() to check the data length?
# matleave$'matleave_13'
# matleave$'matleave_13'==5
# length(matleave$'matleave_13'==5)
```

### Plotting

當我們在R中進行資料視覺化時，理解資料結構對於正確使用圖形化函數是非常重要的。以**`matleave`**資料集為例，如果我們想要繪製其第二列所有行（除了第一行）的條形圖，預設可以被加入繪製的應該要是數值，所以，

-   首先要除去第一欄。因為第一欄為國家名稱（iso3）。所以，選取第二列資料，但忽略第一欄，應該是**`matleave[2, -1]`**。
-   再來，利用**`class(matleave[2, -1])`**來查看**`matleave[2, -1]`**的資料類型。結果會是一個**`data.frame`**的資料類型，因為選取單一列時，R仍然保持了資料的**`data.frame`**結構。
-   然而，當我們嘗試使用**`barplot()`**函數繪製長條圖時，就不能直接把`data.frame`給`barplot()`進行繪製。。這是因為**`barplot()`**函數期望的輸入是一個`vector`。因此，我們使用**`unlist(matleave[2, -1])`**將單列的**`data.frame`**轉換成`vector`。**`unlist()`**函數的作用是將一個列表（或在這個案例中是**`data.frame`**）中的所有元素合併成一個`vector`，這樣就可以用於**`barplot()`**。
-   為了進一步理解這種差異，我們可以使用**`class()`**或**`str()`**函數來觀察未經**`unlist()`**處理的資料。這將顯示出資料仍然保留在**`data.frame`**結構中，與**`unlist()`**後轉換為`vector`的結構不同。這種轉換對於使用某些特定的繪圖函數，如**`barplot()`**，是必要的，因為它們需要一個`vector`作為輸入來正確地繪製圖形。

#### Plotting one row (one country)


``` r
# barplot() the second row of m55
# barplot(m55[2, ])       # raise error

# barplot() the second row when neglecting the first column
# barplot(m55[2, -1])     # raise error

# Take a look at the data type of matleave[2, ]
class(matleave[2, -1])
```

``` output
## [1] "tbl_df"     "tbl"        "data.frame"
```

``` r
class(unlist(matleave[2, -1]))
```

``` output
## [1] "numeric"
```

``` r
# unlist() to convert a single row data.frame to a vector for barplot()
barplot(unlist(m55[2, -1]))
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-6-1.png" width="672" />

Testing


``` r
# View(matleave[1]) # select the 1st variable
# View(matleave[ ,1]) # select the 1st column
# View(matleave[1, ]) # select the 1st row

class(m55[1])		# "tbl_df"     "tbl"        "data.frame"
```

``` output
## [1] "tbl_df"     "tbl"        "data.frame"
```

``` r
class(m55[ ,1])	# "tbl_df"     "tbl"        "data.frame"
```

``` output
## [1] "tbl_df"     "tbl"        "data.frame"
```

``` r
class(m55[1, ])	# "tbl_df"     "tbl"        "data.frame"
```

``` output
## [1] "tbl_df"     "tbl"        "data.frame"
```

``` r
class(m55$iso3)	# character (vector)
```

``` output
## [1] "character"
```

#### More arguments (args)

接下來我們要微調一下視覺化的結果。這行程式碼使用R中的barplot函數繪製一個長條圖，其中的參數說明如下：

1.  `unlist(m55[2, -1])`: 將`m55`資料集的第`2`行（不包括第1欄）轉換為一個向量，並作為長條圖的高度（即每個長條的高度）。
2.  `ylim=c(0, 5)`: 設置y軸的範圍為0到5，即長條圖的最大高度為5。
3.  `space=0`: 設置相鄰兩個長條之間的距離為0，即長條緊密相連。
4.  `border=NA`: 設置長條的邊框為透明，即不顯示邊框。
5.  `xaxt="n"`: 不顯示x軸的標籤。
6.  `yaxt="n"`: 不顯示y軸的標籤。


``` r
# barplot() the unlisted second row (neglecting the first col)
barplot(unlist(m55[2, -1]))
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-8-1.png" width="672" />

``` r
# use ?barplot to know more argument of the function.
?barplot

# Add arguments ylim, space, border, and axat/yaxt one by one to barplot()
barplot(unlist(m55[2, -1]), ylim=c(0, 5))
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-8-2.png" width="672" />

``` r
barplot(unlist(m55[2, -1]), ylim=c(0, 5), space=0)
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-8-3.png" width="672" />

``` r
barplot(unlist(m55[2, -1]), ylim=c(0, 5), space=0, border=NA)
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-8-4.png" width="672" />

``` r
barplot(unlist(m55[2, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-8-5.png" width="672" />

#### Plotting multiple lines

我們已經成功繪製了一個國家的資料，接下來我們要繪出所有國家的資料。以`m55`這個篩選後的資料為例，我分別要繪製出第1列至第6列的國家。底下可以看見每一行非常相似且一致的特徵，僅有`matleave`內的索引由1被列出至6。對於這種重複的程式碼，最好的方法是用迴圈（for-loop）的方式將相同的程式碼，從1\~6之間做六次。


``` r
# plot the first row
barplot(unlist(m55[1, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-9-1.png" width="672" />

``` r
# plot the second to 6th rows
barplot(unlist(m55[2, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
barplot(unlist(m55[3, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
barplot(unlist(m55[4, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
barplot(unlist(m55[5, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
barplot(unlist(m55[6, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
```

#### for-loop to plot multiple lines

這段R語言程式碼使用for-loop來重複執行一個指定的程式區塊，將`m55`資料集的前六行資料分別繪製成長條圖。在這段程式碼中，變數`i`控制了for-loop的迭代次數，它從1到6依次取值，然後依次執行所指定的程式區塊。

一般的for-loop的結構如下：`for (variable in sequence) {# code block to be executed}`。其中，變數`variable`是用來控制for-loop的迭代次數的，它會從序列`sequence`中逐一取出元素，並將其賦值給變數`variable`，然後執行大括號`{...}`中所指定的程式區塊。


``` r
# use for loop and use i as index to barplot multiple subgraphs
for(i in 1:6){
  barplot(unlist(m55[i, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
}
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-10-1.png" width="672" /><img src="R11_paid_maternal_files/figure-html/unnamed-chunk-10-2.png" width="672" />

#### Subplots

但這樣一個國家就要畫成一個Plot，如果要將多個國家、也就是多個Plots繪製在同一張圖上的話，R也有支援Subplot的函式與設定。在R語言中，`par`（parameter的縮寫）是一個用於設置繪圖參數的函數，通過它可以控制繪圖的外觀、尺寸、排列等各方面，以便更好地展示數據和分析結果。par函數可以用來設置以下參數：

-   `mfrow`：設置畫布的分割，即將畫布分為多少行和多少列，例如`mfrow=c(3,2)`代表三列二行。
-   `mai`：設置畫布的邊緣大小，包括上下左右四個邊緣的大小。
-   `cex`：設置字體大小的縮放比例。
-   `col`：設置線條、點和字體的顏色。
-   `pch`：設置散點圖中點的形狀。
-   `lty`：設置線條的類型。

在這段程式碼中，`par`函數被用來設置畫布的分割和邊緣大小，具體來說，`par(mfrow=c(3,2), mai= c(0.2, 0.2, 0.2, 0.2))`表示將畫布分為3行2列的子圖，並設置邊緣大小為0.2，包括上下左右四個邊緣。這樣可以方便地在同一張畫布上顯示多個圖形，並控制它們之間的排列和間距。


``` r
# use ?par to get more plotting parameters
?par

# use par() to set-up the layout of subgraphs
# use the parameter main=c(0.2, 0.2, 0.2, 0.2) to thrink the padding of figures.
par(mfrow=c(3,2), mai= c(0.2, 0.2, 0.2, 0.2))
for(i in 1:6){
  barplot(unlist(m55[i, -1]), ylim=c(0, 5), space=0, border=NA, xaxt="n", yaxt="n")
}
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-11-1.png" width="672" />

我用`nrow(m55)`來取得`m55`這個`data.frame`共有多少個國家，然後，我讓`for-loop`從`1:nrow(m55)`相當於繪製完所有`m55`中的子圖。注意我修改了`mfrow`為`mfrow=c(4, 6)`。


```r
# nrow() to check number of row of m55.
nrow(m55)
```

```{.output}
## [1] 18
```

```r
# use par() to set-up plotting parameters.
par(mfrow=c(4, 6), mai= c(0.2, 0.2, 0.2, 0.2))

# use for-loop to plot all graph as subgraph
for (i in 1:nrow(m55)){
  barplot(unlist(m55[i, -1]), border=NA, space=0, xaxt="n", yaxt="n", ylim = c(0,5))
}
```

<img src="R11_paid_maternal_files/figure-html/plot-m55-1.png" width="672" />

在每個子圖上，我要加上每個國家的國別代碼`iso3`，也就是`m55`的第一行如`m55[i,1]`，我用同樣的`i`來掃過每一列，繪製完`barplot()`後，便用`title()`函式來繪製文字。結果如下。注意我的設定`title(m55[i,1], line = -4, cex.main=3)`。`line`為繪製文字的基線，而`cex.main`是字型大小。


```r
par(mfrow=c(4,6), mai= c(0.2, 0.2, 0.2, 0.2))
for (i in 1:nrow(m55)){
  barplot(unlist(m55[i, -1]), border=NA, space=0,xaxt="n", yaxt="n", ylim = c(0,5))
	title(m55[i,1], line = -4, cex.main=3)
}
```

<img src="R11_paid_maternal_files/figure-html/plot-caption-1.png" width="672" />

### Practice. Plotting more

-   繪製`m05`的資料，也就是`matleave_95!=5`但`matleave_13==5`的資料。
-   繪製`m04`的資料，也就是`matleave_95!=4`但`matleave_13==4`的資料。
-   繪製`m44`的資料，也就是`matleave_95==4`但`matleave_13==4`的資料。


``` r
# plotting matleave_95 != 5 but matleave_13 == 5

# plotting for matleave_13 == 4
```

### Practice. Selecting and filtering by dplyr I

請嘗試問問ChatGPT，如果將以下程式碼改為dplyr的寫法，要怎麼寫。


``` r
df <- read_excel("data/WORLD-MACHE_Gender_6.8.15.xls", "Sheet1", col_names=T)

# select columns by index
# matleave <- df[ , c(3, 6:24)]

# select all NA cells and assign 0 to them
# matleave[is.na(matleave)] <- 0

# filter rows by condition
# m5 <- matleave[matleave$'matleave_13' == 5, ]

# filter rows by condition
# m55<- m5[m5$'matleave_95' == 5,]

# plot
par(mfrow=c(4,6), mai= c(0.2, 0.2, 0.2, 0.2))
for (i in c(1:nrow(m55))){
	barplot(unlist(m55[i,-1]),
			border=NA, space=0,xaxt="n", yaxt="n", ylim = c(0,5))
	title(m55[i,1], line = -4, cex.main=3)
}
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-13-1.png" width="672" />

### (More) Clean version


``` r
# readxl::read_excel() to import the xls file
library(readxl)

df <- read_excel("data/WORLD-MACHE_Gender_6.8.15.xls", "Sheet1", col_names=T)

# select iso3, and matleave columns by index
matleave <- df[ , c(3, 6:24)]

# str() to inspect the data structure of 
str(matleave)
```

``` output
## tibble [197 × 20] (S3: tbl_df/tbl/data.frame)
##  $ iso3       : chr [1:197] "AFG" "ALB" "DZA" "AND" ...
##  $ matleave_95: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_96: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_97: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_98: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_99: num [1:197] 2 5 3 2 2 2 2 3 1 5 ...
##  $ matleave_00: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_01: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_02: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_03: num [1:197] 2 5 3 3 2 2 2 3 1 5 ...
##  $ matleave_04: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_05: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_06: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_07: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_08: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_09: num [1:197] 2 5 3 3 2 2 2 5 1 5 ...
##  $ matleave_10: num [1:197] 2 5 3 3 2 2 2 5 NA 5 ...
##  $ matleave_11: num [1:197] 2 5 3 3 2 2 2 5 3 5 ...
##  $ matleave_12: num [1:197] 2 5 3 3 2 2 2 5 3 5 ...
##  $ matleave_13: num [1:197] 2 5 3 3 2 2 2 5 3 5 ...
```

``` r
# select all NA cells and assign 0 to them
matleave[is.na(matleave)] <- 0

# filter rows by condition
m55 <- matleave[matleave$'matleave_13' == 5 & matleave$'matleave_95' == 5, ]

# plot
par(mfrow=c(4,6), mai= c(0.2, 0.2, 0.2, 0.2))
for (i in c(1:nrow(m55))){
	barplot(unlist(m55[i,-1]),
			border=NA, space=0,xaxt="n", yaxt="n", ylim = c(0,5))
	title(m55[i,1], line = -4, cex.main=3)
}
```

<img src="R11_paid_maternal_files/figure-html/unnamed-chunk-14-1.png" width="672" />

### (More) The fittest version to compute staySame


``` r
# staySame version
# staySame <- apply(m5[,2:20], 1, function(x) length(unique(x[!is.na(x)]))) 
# m55 <- m5[staySame, ]
# m50 <- m5[!staySame, ]
```
