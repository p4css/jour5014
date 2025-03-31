---
title: "R for Data Journalism"
author: "HSIEH, JI-LUNG"
date: "2025-03-31"
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
github-repo: rstudio/bookdown-demo
description: "For my Class teaching how to make data news reports"
---

# About {.unnumbered}

這本書是寫給臺大新聞所「**新聞資料分析與視覺化**」課程使用。該課程並重三個面向的訓練：程式語言、視覺化、資料新聞。學生必須先能夠熟練地使用R語言來操作、讀取、清理、視覺化資料；然後以產製新聞為課程目標，了解資料要如何清理，以及選擇適合的視覺化的方法來強化新聞敘事，並避免視覺化方式引起讀者對新聞的理解謬誤。準此，本書分為幾個部分，包含PART I介紹程式語言基礎；PART II則以國際或國內新聞為個案，來介紹資料獲取（爬蟲）、清理、合併、篩選、轉換；PART III則著重如何用資料視覺化來強化敘事。

本書所沿用的資料分析與視覺化案例均為國內、國外的新聞案例如各國產假支薪等級、居住正義、空氣污染、人口議題、COVID-19、資源區域分佈不均、選舉與公投、運輸交通等相關議題的新聞。並大量採用紐約時報挑選作為數據理解與視覺化推廣的「[What's going on in this graph?](https://www.nytimes.com/column/whats-going-on-in-this-graph)」系列新聞，包含美國不同年代各年齡層的淨資產來做視覺化案例。在視覺化教材的設計上，本書大量參考紐時「[What's going on in this graph?](https://www.nytimes.com/column/whats-going-on-in-this-graph)」的分類與[@wilke2019fundamentals]所著「[Fundamentals of Data Visualization](https://clauswilke.com/dataviz/)」一書的內容安排，強調利用資料視覺化方法來呈現新聞數據中的數量、分佈、比例、趨勢等，並均換用國內或紐時的相關資料新聞案例做範例，以利中文讀者的理解。

**學習路徑**


```{=html}
<div class="grViz html-widget html-fill-item" id="htmlwidget-8abad0022f35882791a5" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-8abad0022f35882791a5">{"x":{"diagram":"\ndigraph G {\n  fontname=\"Helvetica,Arial,sans-serif\"\n  graph [layout = dot, rankdir=TD]\n  node [shape = rect,  height=0, fontname=\"Helvetica\", width=2]\n  node [style = filled, fillcolor=\"honeydew1\"]\n  edge [fontname=\"Courier\", splines=false, weight=2]\n\n  # define nodes\n  rbasic  [label = \"R Basic\"]\n  matleave[label = \"Filter & Select data\n(Maternal leave)\"]\n  tptheft [label = \"Pivot Analysis\n(Taipei Theft)\"]\n  \n  subgraph cluster_1{\n    label=\"base to dplyr\"\n    style=filled; fillcolor=\"lightgrey\";\n    dplyr_ml  [label = \"Filter & Select data with dplyr\n(Maternal leave)\"]\n    dplyr_tp  [label = \"Pivot Analysis with dplyr\n(Taipei Theft)\"]\n  }\n  \n  # define edge\n  node [fillcolor=\"azure1\"]\n    rbasic -> matleave -> tptheft -> {dplyr_tp, dplyr_ml} -> dplyr \n  node [fillcolor=\"gold\"]\n    dplyr -> {join, ggplot, textmining, scraper}\n  node [fillcolor=\"lightgrey\"]\n    join -> DB\n  node [fillcolor=\"yellow1\"]\n    ggplot -> temporal -> geospatial\n  node [fillcolor=\"skyblue\"]\n    scraper -> json -> htmlparser\n  node [fillcolor=\"pink1\"]\n    textmining -> doclevel -> wordlevel -> keyness -> POS -> sentiment\n  \n  # define path over edge\n  edge [constraint=false, penwidth=3, color=\"#ff000033\", weight=1, splines=curved]\n  rbasic -> matleave -> tptheft -> dplyr_ml\n  dplyr_ml -> dplyr_tp -> dplyr -> join -> ggplot\n  ggplot -> temporal -> geospatial -> textmining\n  textmining -> doclevel -> wordlevel -> keyness -> scraper\n}\n","config":{"engine":"dot","options":null}},"evals":[],"jsHooks":[]}</script>
```
