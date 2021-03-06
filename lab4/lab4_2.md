---
title: "Transforming data 2: `filter()`"
date: "2022-01-20"
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
1. Use the functions of dplyr (filter, select, arrange) to organize and sort data frames.  
2. Use `mutate()` to calculate a new column from existing columns.  

## Review  
In the previous lab, we used `select()` to extract columns of interest from a data frame. This helps us focus our attention on the variables relevant to our question. However, it doesn't allow us to extract observations from within the data frame. The `filter()` function allows us to extract data that meet specific criteria. When combined with `select()`, we have the power to transform, shape, and explore data with the potential to make new discoveries.  

## Load the tidyverse

```r
library("tidyverse")
```

## Load the data
For these exercises we will use the data from lab 4_1.  

```r
fish <- readr::read_csv("data/Gaeta_etal_CLC_data.csv")
```

```
## Rows: 4033 Columns: 6
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): lakeid, annnumber
## dbl (4): fish_id, length, radii_length_mm, scalelength
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
mammals <- readr::read_csv("data/mammal_lifehistories_v2.csv")
```

```
## Rows: 1440 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): order, family, Genus, species
## dbl (9): mass, gestation, newborn, weaning, wean mass, AFR, max. life, litte...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

To keep things tidy, I am going to rename the variables in the mammals data.  

```r
mammals <- rename(mammals, genus=Genus, wean_mass="wean mass", max_life="max. life", litter_size="litter size", litters_per_year="litters/year")
```

```r
names(mammals)
```

```
##  [1] "order"            "family"           "genus"            "species"         
##  [5] "mass"             "gestation"        "newborn"          "weaning"         
##  [9] "wean_mass"        "AFR"              "max_life"         "litter_size"     
## [13] "litters_per_year"
```

```r
mammals <- clean_names(mammals)
names(mammals)
```

```
##  [1] "order"            "family"           "genus"            "species"         
##  [5] "mass"             "gestation"        "newborn"          "weaning"         
##  [9] "wean_mass"        "afr"              "max_life"         "litter_size"     
## [13] "litters_per_year"
```

## `filter()`get things out of rows!!! select() get things out of columns!!!!
Unlike `select()`, `filter()` allows us to extract data that meet specific criteria within a variable. Let's say that we are interested only in the fish that occur in lake "AL". We can use `filter()` to extract these observations.  

```r
filter(fish, lakeid == "AL")
```

```
## # A tibble: 383 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 373 more rows
```

```r
tabyl(fish, lakeid)
```

```
##  lakeid   n    percent
##      AL 383 0.09496653
##      AR 262 0.06496405
##      BO 197 0.04884701
##      BR 291 0.07215472
##      CR 343 0.08504835
##      DY 355 0.08802380
##      FD 302 0.07488222
##      JN 238 0.05901314
##      LC 173 0.04289611
##      LJ 181 0.04487974
##      LR 292 0.07240268
##     LSG 143 0.03545748
##      MN 293 0.07265063
##      RD 135 0.03347384
##      UB 191 0.04735929
##      WS 254 0.06298041
```

Similarly, if we are only interested in fish with a length greater than or equal to 350 we can use `filter()` to extract these observations.  

```r
filter(fish, radii_length_mm >= 2.9)
```

```
## # A tibble: 2,560 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         300 EDGE         175            3.02        3.02
##  2 AL         301 EDGE         194            3.34        3.34
##  3 AL         301 3            194            2.97        3.34
##  4 AL         302 EDGE         324            6.07        6.07
##  5 AL         302 9            324            5.73        6.07
##  6 AL         302 8            324            5.41        6.07
##  7 AL         302 7            324            5.01        6.07
##  8 AL         302 6            324            4.56        6.07
##  9 AL         302 5            324            4.20        6.07
## 10 AL         302 4            324            3.72        6.07
## # … with 2,550 more rows
```

+ `filter()` allows all of the expected operators; i.e. >, >=, <, <=, != (not equal), and == (equal).  

Using the `!` operator allows for the exclusion of specific observations.

```r
filter(fish, lakeid != "AL")
```

```
## # A tibble: 3,650 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AR         269 EDGE         140            2.01        2.01
##  2 AR         269 1            140            1.48        2.01
##  3 AR         270 EDGE         193            2.66        2.66
##  4 AR         270 3            193            2.39        2.66
##  5 AR         270 2            193            2.03        2.66
##  6 AR         270 1            193            1.42        2.66
##  7 AR         271 EDGE         220            3.50        3.50
##  8 AR         271 5            220            3.13        3.50
##  9 AR         271 4            220            2.86        3.50
## 10 AR         271 3            220            2.63        3.50
## # … with 3,640 more rows
```

## Using `filter()` with multiple observations  
Filtering multiple values within the same variable requires the `%in%` [operator](https://www.tutorialspoint.com/r/r_operators.htm).    

```r
filter(fish, length %in% c(167, 175))
```

```
## # A tibble: 18 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167           2.70         2.70
##  2 AL         299 2            167           2.04         2.70
##  3 AL         299 1            167           1.31         2.70
##  4 AL         300 EDGE         175           3.02         3.02
##  5 AL         300 3            175           2.67         3.02
##  6 AL         300 2            175           2.14         3.02
##  7 AL         300 1            175           1.23         3.02
##  8 BO         397 EDGE         175           2.67         2.67
##  9 BO         397 3            175           2.39         2.67
## 10 BO         397 2            175           1.59         2.67
## 11 BO         397 1            175           0.830        2.67
## 12 LSG         45 EDGE         175           3.21         3.21
## 13 LSG         45 3            175           2.92         3.21
## 14 LSG         45 2            175           2.44         3.21
## 15 LSG         45 1            175           1.60         3.21
## 16 RD         103 EDGE         167           2.80         2.80
## 17 RD         103 2            167           2.10         2.80
## 18 RD         103 1            167           1.31         2.80
```

Alternatively, you can use `between` if you are looking for a range of specific values.  

```r
filter(fish, between(scalelength, 2.5, 2.55))
```

```
## # A tibble: 10 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 LSG         56 EDGE         145            2.55        2.55
##  2 LSG         56 2            145            1.94        2.55
##  3 LSG         56 1            145            1.20        2.55
##  4 LSG         57 EDGE         143            2.52        2.52
##  5 LSG         57 2            143            2.13        2.52
##  6 LSG         57 1            143            1.19        2.52
##  7 UB          80 EDGE         153            2.55        2.55
##  8 UB          80 3            153            2.10        2.55
##  9 UB          80 2            153            1.62        2.55
## 10 UB          80 1            153            1.14        2.55
```

You can also extract observations "near" a certain value but you need to specify a tolerance.  

```r
filter(fish, near(radii_length_mm, 2, tol = 0.2))
```

```
## # A tibble: 291 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 2            167            2.04        2.70
##  2 AL         300 2            175            2.14        3.02
##  3 AL         302 2            324            2.19        6.07
##  4 AL         303 2            325            2.04        6.79
##  5 AL         306 2            350            2.14        6.94
##  6 AL         308 2            355            1.86        6.67
##  7 AL         312 2            367            2.17        6.81
##  8 AL         313 2            367            2.06        6.47
##  9 AL         315 2            372            2.04        6.47
## 10 AL         316 2            372            1.82        6.35
## # … with 281 more rows
```

## Practice
1. Filter the `fish` data to include the samples from lake "BO".

```r
filter(fish, lakeid == "BO")
```

```
## # A tibble: 197 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 BO         389 EDGE         104           1.50         1.50
##  2 BO         389 1            104           0.736        1.50
##  3 BO         390 EDGE         105           1.59         1.59
##  4 BO         390 1            105           0.698        1.59
##  5 BO         391 EDGE         107           1.43         1.43
##  6 BO         391 1            107           0.695        1.43
##  7 BO         392 EDGE         124           2.11         2.11
##  8 BO         392 2            124           1.36         2.11
##  9 BO         392 1            124           0.792        2.11
## 10 BO         393 EDGE         141           2.16         2.16
## # … with 187 more rows
```

2. Filter the data to include all lakes except "AR".

```r
filter(fish, lakeid != "AR")
```

```
## # A tibble: 3,771 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 3,761 more rows
```

3. Filter the fish data to include all fish with a scalelength within 0.25 of 8.

```r
filter(fish, near(scalelength, 8, tol=0.25))
```

```
## # A tibble: 236 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         309 EDGE         355            7.89        7.89
##  2 AL         309 13           355            7.56        7.89
##  3 AL         309 12           355            7.36        7.89
##  4 AL         309 11           355            7.16        7.89
##  5 AL         309 10           355            6.77        7.89
##  6 AL         309 9            355            6.39        7.89
##  7 AL         309 8            355            5.96        7.89
##  8 AL         309 7            355            5.44        7.89
##  9 AL         309 6            355            4.74        7.89
## 10 AL         309 5            355            4.06        7.89
## # … with 226 more rows
```

4. Filter the fish data to include fish with a scalelength between 2 and 4.

```r
filter(fish, between(scalelength, 2, 4))
```

```
## # A tibble: 723 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 713 more rows
```

## Using `filter()` on multiple conditions
You can also use `filter()` to extract data based on multiple conditions. Below we extract only the fish that have lakeid "AL" and length >350.

```r
filter(fish, lakeid == "AL" & length > 350)
```

```
## # A tibble: 314 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         307 EDGE         353            7.55        7.55
##  2 AL         307 13           353            7.28        7.55
##  3 AL         307 12           353            6.98        7.55
##  4 AL         307 11           353            6.73        7.55
##  5 AL         307 10           353            6.48        7.55
##  6 AL         307 9            353            6.22        7.55
##  7 AL         307 8            353            5.92        7.55
##  8 AL         307 7            353            5.44        7.55
##  9 AL         307 6            353            5.06        7.55
## 10 AL         307 5            353            4.37        7.55
## # … with 304 more rows
```

Notice that the `|` operator generates a different result.

```r
filter(fish, lakeid == "AL" | length > 350)
```

```
## # A tibble: 948 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 938 more rows
```

```r
filter(fish, xor(lakeid == "AL", length >=200))
```

```
## # A tibble: 3,219 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 3,209 more rows
```

Rules:  
+ `filter(df, condition1, condition2)` will return rows where both conditions are met.  
+ `filter(df, condition1, !condition2)` will return all rows where condition one is true but condition 2 is not.  
+ `filter(df, condition1 | condition2)` will return rows where condition 1 and/or condition 2 is met.  
+ `filter(df, xor(condition1, condition2))` will return all rows where only one of the conditions is met, and not when both conditions are met.  

In this case, we filter out the fish with a length over 400 and a scale length over 11 or a radii length over 8.

```r
filter(fish, length > 400, (scalelength > 11 | radii_length_mm > 8))
```

```
## # A tibble: 23 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         324 EDGE         406            8.21        8.21
##  2 AL         327 EDGE         413            8.33        8.33
##  3 AL         327 15           413            8.11        8.33
##  4 AL         328 EDGE         420            8.71        8.71
##  5 AL         328 16           420            8.41        8.71
##  6 AL         328 15           420            8.14        8.71
##  7 WS         180 EDGE         403           11.0        11.0 
##  8 WS         180 16           403           10.6        11.0 
##  9 WS         180 15           403           10.3        11.0 
## 10 WS         180 14           403            9.93       11.0 
## # … with 13 more rows
```

## Practice  
1. Have a look at the mammals data using the summary functions of your choosing.    

```r
summary(mammals)
```

```
##     order              family             genus             species         
##  Length:1440        Length:1440        Length:1440        Length:1440       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##       mass             gestation          newborn             weaning       
##  Min.   :     -999   Min.   :-999.00   Min.   :   -999.0   Min.   :-999.00  
##  1st Qu.:       50   1st Qu.:-999.00   1st Qu.:   -999.0   1st Qu.:-999.00  
##  Median :      403   Median :   1.05   Median :      2.6   Median :   0.73  
##  Mean   :   383577   Mean   :-287.25   Mean   :   6703.1   Mean   :-427.17  
##  3rd Qu.:     7009   3rd Qu.:   4.50   3rd Qu.:     98.0   3rd Qu.:   2.00  
##  Max.   :149000000   Max.   :  21.46   Max.   :2250000.0   Max.   :  48.00  
##    wean_mass             afr             max_life       litter_size      
##  Min.   :    -999   Min.   :-999.00   Min.   :-999.0   Min.   :-999.000  
##  1st Qu.:    -999   1st Qu.:-999.00   1st Qu.:-999.0   1st Qu.:   1.000  
##  Median :    -999   Median :   2.50   Median :-999.0   Median :   2.270  
##  Mean   :   16049   Mean   :-408.12   Mean   :-490.3   Mean   : -55.634  
##  3rd Qu.:      10   3rd Qu.:  15.61   3rd Qu.: 147.2   3rd Qu.:   3.835  
##  Max.   :19075000   Max.   : 210.00   Max.   :1368.0   Max.   :  14.180  
##  litters_per_year  
##  Min.   :-999.000  
##  1st Qu.:-999.000  
##  Median :   0.375  
##  Mean   :-477.141  
##  3rd Qu.:   1.155  
##  Max.   :   7.500
```

2. What are the names of the variables in the mammals data?  

```r
names(mammals)
```

```
##  [1] "order"            "family"           "genus"            "species"         
##  [5] "mass"             "gestation"        "newborn"          "weaning"         
##  [9] "wean_mass"        "afr"              "max_life"         "litter_size"     
## [13] "litters_per_year"
```

3.  From the `mammals` data, filter all members of the family Bovidae with a mass greater than 450000.

```r
filter(mammals, family =="Bovidae" & mass > 450000)
```

```
## # A tibble: 7 × 13
##   order   family genus  species   mass gestation newborn weaning wean_mass   afr
##   <chr>   <chr>  <chr>  <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl> <dbl>
## 1 Artiod… Bovid… Bison  bison   4.98e5      8.93  20000    10.7     157500  29.4
## 2 Artiod… Bovid… Bison  bonasus 5   e5      9.14  23000.    6.6       -999  30.0
## 3 Artiod… Bovid… Bos    fronta… 8   e5      9.02  23033.    4.5       -999  24.2
## 4 Artiod… Bovid… Bos    javani… 6.67e5      9.83   -999     9.5       -999  25.5
## 5 Artiod… Bovid… Bubal… bubalis 9.5 e5     10.5   37500     7.5       -999  19.9
## 6 Artiod… Bovid… Synce… caffer  5.05e5     11.0   42862.    9.18    166000  47.9
## 7 Artiod… Bovid… Tauro… derbia… 6.8 e5      8.67   -999  -999         -999  36.4
## # … with 3 more variables: max_life <dbl>, litter_size <dbl>,
## #   litters_per_year <dbl>
```

4. Let's say we are only interested in family, genus, species, mass, and gestation. Relimit the mammals data frame to these variables.  

```r
mammals <- select(mammals, family, genus, species, mass, gestation, newborn)
mammals
```

```
## # A tibble: 1,440 × 6
##    family         genus       species          mass gestation newborn
##    <chr>          <chr>       <chr>           <dbl>     <dbl>   <dbl>
##  1 Antilocapridae Antilocapra americana      45375       8.13   3246.
##  2 Bovidae        Addax       nasomaculatus 182375       9.39   5480 
##  3 Bovidae        Aepyceros   melampus       41480       6.35   5093 
##  4 Bovidae        Alcelaphus  buselaphus    150000       7.9   10167.
##  5 Bovidae        Ammodorcas  clarkei        28500       6.8    -999 
##  6 Bovidae        Ammotragus  lervia         55500       5.08   3810 
##  7 Bovidae        Antidorcas  marsupialis    30000       5.72   3910 
##  8 Bovidae        Antilope    cervicapra     37500       5.5    3846 
##  9 Bovidae        Bison       bison         497667.      8.93  20000 
## 10 Bovidae        Bison       bonasus       500000       9.14  23000.
## # … with 1,430 more rows
```

5. Use the output from #4 to focus on the family Bovidae with a mass greater than 450000.

```r
filter(mammals, family == "Bovidae" & mass >= 450000)
```

```
## # A tibble: 7 × 6
##   family  genus       species      mass gestation newborn
##   <chr>   <chr>       <chr>       <dbl>     <dbl>   <dbl>
## 1 Bovidae Bison       bison     497667.      8.93  20000 
## 2 Bovidae Bison       bonasus   500000       9.14  23000.
## 3 Bovidae Bos         frontalis 800000       9.02  23033.
## 4 Bovidae Bos         javanicus 666667.      9.83   -999 
## 5 Bovidae Bubalus     bubalis   950000      10.5   37500 
## 6 Bovidae Syncerus    caffer    504667.     11.0   42862.
## 7 Bovidae Taurotragus derbianus 680000       8.67   -999
```

6. From the `mammals` data, build a data frame that compares `mass`, `gestation`, and `newborn` among the primate genera `Lophocebus`, `Erythrocebus`, and `Macaca`. Among these genera, which species has the smallest `newborn` mass?

```r
newerer_mammals <- select(mammals, mass, gestation, newborn,genus)
```

```r
names(mammals)
```

```
## [1] "family"    "genus"     "species"   "mass"      "gestation" "newborn"
```


```r
#filter(newerer_mammals, genus == "Lophocebus" | genus == "Erythrocebus" | genus == "Macaca")
filter(newerer_mammals, genus %in% c("Lophocebus", "Erythrocebus", "Macaca"))
```

```
## # A tibble: 15 × 4
##      mass gestation newborn genus       
##     <dbl>     <dbl>   <dbl> <chr>       
##  1  5883.      5.56    546. Erythrocebus
##  2  6726.      5.97    462. Lophocebus  
##  3 10037.      5.67    533. Macaca      
##  4  8858.      5.72    505. Macaca      
##  5  5575       5.43    390. Macaca      
##  6  9753.      5.49    450  Macaca      
##  7  7308.      6       486. Macaca      
##  8  6212.      5.78    458. Macaca      
##  9  3495    -999       446  Macaca      
## 10  4875       5.94    418  Macaca      
## 11  5413.      5.47    476. Macaca      
## 12  6317.      5.4     401  Macaca      
## 13  6133.      5.71    476. Macaca      
## 14  3456.      5.49    408. Macaca      
## 15  3735       5.43    391. Macaca
```



## Wrap-up  

Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
