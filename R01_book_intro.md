

# Introduction {#introduction}

本書從基本的 **`base`** R 語法開始學習，因為 R 語言最初是為統計學研究而開發的。因此，許多統計套件和基礎操作都是使用 **`base`** R 語法編寫的，這些知識對於瞭解 R 的核心功能和基本操作是非常重要的，並能夠建立起對於數據分析和統計建模的基礎知識。不過，現在 **`tidyverse`** 風格的編寫方式越來越流行，這種方式使得程式碼撰寫效率更高且可讀性更強，對於數據科學家而言是非常有用的。因此，本書會接續使用 **`tidyverse`** 中的主要函式庫和函數，例如 **`dplyr`**、**`ggplot2`** 和 **`tidyr`**，以便讀者能夠掌握這些工具，並能夠有效地應用於實際數據分析項目中。

## Preloaded tool kits

當 R 的程式開始執行時，會預載入一些基本的套件，包括 **`stats`**、**`graphics`**、**`grDevices`**、**`utils`** 和 **`datasets`**。這些套件為 R 提供了基本的數據處理、統計分析、圖形顯示和檔案處理等功能。以下是這些套件的重要函式：

-   **`stats`**：這個套件包含了許多統計分析相關的函式，例如假設檢定、方差分析、線性迴歸和時間序列分析等。其中，比較常用的函式有 **`t.test()`**、**`lm()`**、**`glm()`**、**`anova()`**、**`cor()`** 等。

-   **`graphics`**：這個套件提供了用於繪製各種圖形的函式，例如散佈圖、直方圖、盒鬚圖、線圖、散點矩陣等。其中，常用的函式有 **`plot()`**、**`hist()`**、**`boxplot()`**、**`lines()`**、**`points()`** 等。

-   **`grDevices`**：這個套件包含了用於輸出圖形的函式，例如 **`pdf()`**、**`png()`**、**`jpeg()`** 和 **`tiff()`** 等。

-   **`utils`**：這個套件包含了一些實用的函式，例如 **`install.packages()`**、**`help()`**、**`data()`** 等。

-   **`datasets`**：這個套件包含了一些內建的數據集，可以用來進行測試和練習，例如 **`iris`**、**`mtcars`**、**`CO2`** 等。可以使用 **`data()`** 函式載入這些數據集。

## Dataset used in the book

本書所採用的資料可以從台大新聞所「新聞資料分析與視覺呈現」的github repository下載。[R4CSS/data](https://github.com/p4css/R4CSS/tree/master/data)。

## Cases in the book

-   WP: Paid maternity leave
-   Taipei Residential Hot spot: Contingency table, categorical data
-   Trump's tweets: Line plot, Bar chart, timeline, and text processing
-   NYT: Net worth: Line plot
-   NYT: Carbon Proportion: Treemap
-   NYT: Optimism by countries
-   NYT: Population growth
-   Annual Budget of TW government: NA Processing, Treemap
-   Vaccinating proportion by country x year: Proportion
-   NYT: LeBron James's Achievement
