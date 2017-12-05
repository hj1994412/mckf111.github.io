---
title: "Viridis Demo"
output:
  html_document:
    keep_md: true
---



The code below demonstrates two color palettes in the [viridis](https://github.com/sjmgarnier/viridis) package. Each plot displays a contour map of the Maunga Whau volcano in Auckland, New Zealand.

## Viridis colors


```r
image(volcano, col = viridis(200))
```

![](2017-12-05-testRmarkdown_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

## Magma colors


```r
image(volcano, col = viridis(200, option = "A"))
```

![](2017-12-05-testRmarkdown_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
