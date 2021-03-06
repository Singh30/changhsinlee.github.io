---
layout: post
title: 用broom畫殘差圖(residual plot)
categories: [rstats, graphics]
tags: [broom, residual, ggplot2, regression]
lang: ch
comments: true
---



在作迴歸分析時，常常會用殘差圖(residual plot)來當作模型是否有改進空間的依據。最近在教學時發現David Robinson作的`broom`套件相當好用，也可以拿來畫殘差圖。`broom`內的函數大多是拿來計算和model相關的數據，而它和R內建函數的不同之處是所輸出的物件通通都是data frame，所以能和Hadley Wickham的`dplyr`及`ggplot2`無縫接軌。

## 例子

這裡我先對`MASS`套件內的`Boston`資料檔作線性迴歸。



```r
library(dplyr)
library(ggplot2)
library(broom)
data(Boston, package = "MASS")
bos.fit <- lm(medv ~ lstat, data = Boston)
```

之後把建好的模型`bos.fit`丟進`broom`裡的`augment()`，再用計算出的殘差值畫出殘差圖。


```r
b.res.plot <- bos.fit %>% augment %>%
  ggplot(aes(x = .fitted, y = .resid)) + geom_point()
b.res.plot
```

![plot of chunk unnamed-chunk-2](/figure/source/2016-11-02-make-residual-plot-with-broom/unnamed-chunk-2-1.png)

從得到的圖可以看出殘差本身有些規律，這時再用`ggplot`加上一條擬合線


```r
b.res.plot + geom_smooth(se = FALSE)
```

![plot of chunk unnamed-chunk-3](/figure/source/2016-11-02-make-residual-plot-with-broom/unnamed-chunk-3-1.png)

就不難看出這個線性迴歸模型還有改進空間。

## 其他函數

`broom`內還有很多方便的函數，像是`tidy()`和`glance()`，以及可以和`dplyr`一起拿來作拔靴法的`bootstrep`等等。另外David Robinson本身有寫[部落格](http://varianceexplained.org/)，他的文章也是相當高水準，值得一讀。






