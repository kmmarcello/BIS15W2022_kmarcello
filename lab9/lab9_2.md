---
title: "Data Visualization: `ggplot` part 1"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Produce boxplots using `ggplot.`  
2. Customize labels on axes using `labs` and `themes`.  
3. Use `color`, `fill`, and `group` to customize plots and improve overall aesthetics.  

## Breakout Rooms  
Please take 5-8 minutes to check over your answers to HW 8 in your group. If you are stuck, please remember that you can check the key in [Joel's repository](https://github.com/jmledford3115/BIS15LW2022_jledford).  

##Resources  
- [ggplot2 cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf)  
- [`ggplot` themes](https://ggplot2.tidyverse.org/reference/ggtheme.html)  
- [Rebecca Barter `ggplot` Tutorial](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/)  

## Load the libraries

```r
library(tidyverse)
library(skimr)
library(janitor)
library(here)
library(palmerpenguins)
```

## Review
Now that you have been introduced to `ggplot`, let's practice a few more plot types. Remember that plots are built in layers: **plot= data + geom_ + aesthetics**. We have to specify each of these in order for a plot to be produced. If you get stuck, it is often helpful to stop and make a quick sketch of what you want or expect to see on a piece of scratch paper.  

Let's review using the `penguins` data. First, get an idea of the structure: Are the data tidy? Are there NA's? Are the variables discrete, categorical, or continuous?

```r
penguins
```

```
## # A tibble: 344 × 8
##    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
##  1 Adelie  Torgersen           39.1          18.7               181        3750
##  2 Adelie  Torgersen           39.5          17.4               186        3800
##  3 Adelie  Torgersen           40.3          18                 195        3250
##  4 Adelie  Torgersen           NA            NA                  NA          NA
##  5 Adelie  Torgersen           36.7          19.3               193        3450
##  6 Adelie  Torgersen           39.3          20.6               190        3650
##  7 Adelie  Torgersen           38.9          17.8               181        3625
##  8 Adelie  Torgersen           39.2          19.6               195        4675
##  9 Adelie  Torgersen           34.1          18.1               193        3475
## 10 Adelie  Torgersen           42            20.2               190        4250
## # … with 334 more rows, and 2 more variables: sex <fct>, year <int>
```

In a previous lab, we asked how many penguins were measured on each island.

```r
penguins %>% count(island)
```

```
## # A tibble: 3 × 2
##   island        n
##   <fct>     <int>
## 1 Biscoe      168
## 2 Dream       124
## 3 Torgersen    52
```


```r
penguins %>% 
  count(island, species, sort = F) %>% 
  ggplot(aes(x=island, y=n))+
  geom_col()
```

![](lab9_2_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

What if we wanted a plot that showed the number of measured penguins for each species?

```r
penguins %>% count(species)
```

```
## # A tibble: 3 × 2
##   species       n
##   <fct>     <int>
## 1 Adelie      152
## 2 Chinstrap    68
## 3 Gentoo      124
```


```r
penguins %>% 
  count(species) %>% 
  ggplot(aes(x=species, y=n))+
  geom_col()
```

![](lab9_2_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

How about average bill length by sex?

```r
penguins %>% 
  filter(sex!="NA") %>% 
  group_by(sex) %>% 
  summarise(mean_bill_length=mean(bill_length_mm))
```

```
## # A tibble: 2 × 2
##   sex    mean_bill_length
##   <fct>             <dbl>
## 1 female             42.1
## 2 male               45.9
```


```r
penguins %>% 
  filter(sex!="NA") %>% 
  group_by(sex) %>% 
  summarise(ave_bill_length=mean(bill_length_mm)) %>% 
  ggplot(aes(x=sex, y=ave_bill_length)) +
  geom_col()
```

![](lab9_2_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Box Plots
For the next series of examples, we will use the `homerange` data.  **Database of vertebrate home range sizes.**   

Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  


```r
homerange <- read_csv(here("lab9", "data", "Tamburelloetal_HomeRangeDatabase.csv"))
homerange
```

```
## # A tibble: 569 × 24
##    taxon  common.name   class   order   family genus species primarymethod N    
##    <chr>  <chr>         <chr>   <chr>   <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake … american eel  actino… anguil… angui… angu… rostra… telemetry     16   
##  2 river… blacktail re… actino… cyprin… catos… moxo… poecil… mark-recaptu… <NA> 
##  3 river… central ston… actino… cyprin… cypri… camp… anomal… mark-recaptu… 20   
##  4 river… rosyside dace actino… cyprin… cypri… clin… fundul… mark-recaptu… 26   
##  5 river… longnose dace actino… cyprin… cypri… rhin… catara… mark-recaptu… 17   
##  6 river… muskellunge   actino… esocif… esoci… esox  masqui… telemetry     5    
##  7 marin… pollack       actino… gadifo… gadid… poll… pollac… telemetry     2    
##  8 marin… saithe        actino… gadifo… gadid… poll… virens  telemetry     2    
##  9 marin… lined surgeo… actino… percif… acant… acan… lineat… direct obser… <NA> 
## 10 marin… orangespine … actino… percif… acant… naso  litura… telemetry     8    
## # … with 559 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

Boxplots help us visualize a range of values. So, on the x-axis we typically have something categorical and the y-axis is the range. In the case below, we are plotting `log10.mass` by taxonomic class in the `homerange` data. `geom_boxplot()` is the geom type for a standard box plot. The center line in each box represents the median, not the mean.

Let's look at the variable `log10.mass` grouped by taxonomic class.

```r
homerange %>% 
  group_by(class) %>% 
  summarize(min_log10.mass=min(log10.mass),
            max_log10.mass=max(log10.mass),
            median_log10.mass=median(log10.mass))
```

```
## # A tibble: 4 × 4
##   class          min_log10.mass max_log10.mass median_log10.mass
##   <chr>                   <dbl>          <dbl>             <dbl>
## 1 actinopterygii         -0.658           3.55              2.08
## 2 aves                    0.712           4.95              1.82
## 3 mammalia                0.620           6.60              3.33
## 4 reptilia                0.539           4.03              2.51
```


```r
homerange %>% 
  ggplot(aes(x = class, y = log10.mass)) +
  geom_boxplot()
```

![](lab9_2_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

## Practice
1. There are more herbivores than carnivores in the homerange data, but how do their masses compare? Make a summary and boxplot that compares their masses. Use `log10.mass`.

```r
homerange %>% 
  select(trophic.guild, log10.mass) %>% 
  group_by(trophic.guild) %>% 
  summarise(mean_log10.mass = mean(log10.mass))
```

```
## # A tibble: 2 × 2
##   trophic.guild mean_log10.mass
##   <chr>                   <dbl>
## 1 carnivore                2.24
## 2 herbivore                3.13
```


```r
mammalia <- homerange %>% 
  select(trophic.guild, log10.mass) %>% 
  ggplot(aes(x = trophic.guild, y = log10.mass)) +
  geom_boxplot()
mammalia
```

![](lab9_2_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

2. Have a closer look at carnivorous mammals. Summarize and visualize the range of log10.mass by family.

```r
homerange %>% 
  filter(trophic.guild == "carnivore" & class == "mammalia")
```

```
## # A tibble: 80 × 24
##    taxon   common.name   class  order  family  genus species primarymethod N    
##    <chr>   <chr>         <chr>  <chr>  <chr>   <chr> <chr>   <chr>         <chr>
##  1 mammals giant golden… mamma… afros… chryso… chry… trevel… telemetry*    <NA> 
##  2 mammals Grant's gold… mamma… afros… chryso… erem… granti  telemetry*    <NA> 
##  3 mammals arctic fox    mamma… carni… canidae alop… lagopus telemetry*    <NA> 
##  4 mammals Ethiopian wo… mamma… carni… canidae canis simens… telemetry*    <NA> 
##  5 mammals culpeo        mamma… carni… canidae pseu… culpae… telemetry*    <NA> 
##  6 mammals South Americ… mamma… carni… canidae pseu… griseus telemetry*    <NA> 
##  7 mammals kit fox       mamma… carni… canidae vulp… macroti telemetry*    <NA> 
##  8 mammals Ruppel's fox  mamma… carni… canidae vulp… ruppel… telemetry*    <NA> 
##  9 mammals swift fox     mamma… carni… canidae vulp… velox   telemetry*    <NA> 
## 10 mammals fossa         mamma… carni… eupler… cryp… ferox   telemetry*    <NA> 
## # … with 70 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

3. Now use a boxplot to visualize the range of body mass by family of mammalian carnivore.

```r
carnivore_mammals <- homerange %>% 
  filter(trophic.guild == "carnivore" & class == "mammalia") %>% 
  select(family, trophic.guild, log10.mass) %>% 
  ggplot(aes(x = family, y = log10.mass)) +
  geom_boxplot()
carnivore_mammals
```

![](lab9_2_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

## That's it, let's take a break!   

-->[Home](https://jmledford3115.github.io/datascibiol/)
