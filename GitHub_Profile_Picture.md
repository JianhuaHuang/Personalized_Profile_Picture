``` r
library(png)
library(reshape)
library(grid)
library(ggplot2)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following object is masked from 'package:reshape':
    ## 
    ##     rename

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(grDevices)
library(maps)
library(scales)

img <- readPNG('C:/Users/Jianhua/Dropbox/work_doc/github/Personalized_Profile_Picture/Self_Picture.PNG')
g <- rasterGrob(img, interpolate=TRUE)

df.pic <- melt(as.matrix(g$raster)) %>%
  transmute(x = X2, y = max(X1) - X1, color = value)
```

Plot with `ggplot`
------------------

using the `ggplot` function, we can plot the picture directly.

``` r
ggplot(df.pic, aes(x = x, y = y)) +
  geom_raster(aes(fill = color)) + 
  scale_fill_identity() +
  coord_equal() + 
  theme_void()
```

![](GitHub_Profile_Picture_files/figure-markdown_github/unnamed-chunk-2-1.png)

### Plot part of the picture

``` r
df.part <- filter(df.pic, x > max(x) * .3, x < max(x) * .82, y > max(y) * .36, 
  y < max(y) * .68)

ggplot(df.part, aes(x = x, y = y)) +
  geom_raster(aes(fill = color)) + 
  scale_fill_identity() +
  coord_equal() + 
  theme_void()
```

![](GitHub_Profile_Picture_files/figure-markdown_github/unnamed-chunk-3-1.png)

``` r
# divide the color hex value into red, green, and blue
df.part <- data.frame(df.part, t(col2rgb(df.part$color)))

## without alpha, the last layer covers all previous
p <- ggplot(df.part, aes(x = x, y = y)) +
  geom_raster(aes(fill = red), alpha = 1) +
  geom_raster(aes(fill = blue), alpha = .67) +
  geom_raster(aes(fill = green), alpha = .33) +
  coord_equal() + 
  theme_void() 

p + scale_fill_gradient2(high = 'red', low = 'orange', midpoint = 128)+ 
  guides(fill = FALSE)
```

![](GitHub_Profile_Picture_files/figure-markdown_github/unnamed-chunk-3-2.png)

``` r
p + scale_fill_gradient2(high = 'wheat3', low = 'lightskyblue', midpoint = 128)+ 
  guides(fill = FALSE)
```

![](GitHub_Profile_Picture_files/figure-markdown_github/unnamed-chunk-3-3.png)

``` r
p + scale_fill_gradient2(high = 'wheat3', low = 'skyblue2', midpoint = 128)+ 
  guides(fill = FALSE)
```

![](GitHub_Profile_Picture_files/figure-markdown_github/unnamed-chunk-3-4.png)

``` r
p + scale_fill_gradient2(high = 'tan', low = 'lightsteelblue', midpoint = 128)+ 
  guides(fill = FALSE)
```

![](GitHub_Profile_Picture_files/figure-markdown_github/unnamed-chunk-3-5.png)
