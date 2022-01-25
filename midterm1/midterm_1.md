---
title: "Midterm 1"
author: "Kaylah Marcello"
date: "2022-01-25"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
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

```r
library("skimr")
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example. 

We have learned how to extract knowledge from structured or unstructured data, particularly by using functions to separate out columns (select()), filter by rows (filter()), group(group_by) specific data to get overall knowledge of each group. Arranging (arange()) and summarizing (summarize()) specific aspects of data. We have also learned how to turn unstructured data into more structured data by eliminating NAs. We have several tools to initially look at summaries of data so we can make decisions about how to use it (glimps(), str(), head(), skim()).

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice? 

Pipes (%>%) have by far been the most helpful and interesting thing I have learned. I would like more practice with simplifying the code with functions like across().

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
skim(elephants)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |elephants |
|Number of rows           |288       |
|Number of columns        |3         |
|_______________________  |          |
|Column type frequency:   |          |
|character                |1         |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Sex           |         0|             1|   1|   1|     0|        2|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|   sd|    p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|----:|-----:|------:|------:|------:|------:|:-----|
|Age           |         0|             1|  10.97|  8.4|  0.01|   4.58|   9.46|  16.50|  32.17|▆▇▂▂▂ |
|Height        |         0|             1| 187.68| 50.6| 75.46| 160.75| 200.00| 221.09| 304.06|▃▃▇▇▁ |

4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
names(elephants)
```

```
## [1] "Age"    "Height" "Sex"
```

```r
elephants <- clean_names(elephants)
```


```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```

```r
names(elephants)
```

```
## [1] "age"    "height" "sex"
```

5. (2 points) How many male and female elephants are represented in the data?

```r
tabyl(elephants, sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```

6. (2 points) What is the average age all elephants in the data?

```r
elephants %>% 
  summarise(mean_age = mean(age))
```

```
## # A tibble: 1 × 1
##   mean_age
##      <dbl>
## 1     11.0
```

7. (2 points) How does the average age and height of elephants compare by sex?

```r
elephants %>% 
  group_by(sex) %>% 
  summarise(mean_age = mean(age), mean_height = mean(height), n=n())
```

```
## # A tibble: 2 × 4
##   sex   mean_age mean_height     n
##   <fct>    <dbl>       <dbl> <int>
## 1 F        12.8         190.   150
## 2 M         8.95        185.   138
```
8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
elephants %>% 
  group_by(sex) %>% 
  filter(age > "20") %>% 
  summarise(mean_height = mean(height), min_height = min(height), 
            max_height =max(height), 
            n=n())
```

```
## # A tibble: 2 × 5
##   sex   mean_height min_height max_height     n
##   <fct>       <dbl>      <dbl>      <dbl> <int>
## 1 F            205.       123.       278.    75
## 2 M            200.       136.       304.    68
```
For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

```r
vertebrates <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
str(vertebrates)
```

```
## spec_tbl_df [24 × 26] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ TransectID             : num [1:24] 1 2 2 3 4 5 6 7 8 9 ...
##  $ Distance               : num [1:24] 7.14 17.31 18.32 20.85 15.95 ...
##  $ HuntCat                : chr [1:24] "Moderate" "None" "None" "None" ...
##  $ NumHouseholds          : num [1:24] 54 54 29 29 29 29 29 54 25 73 ...
##  $ LandUse                : chr [1:24] "Park" "Park" "Park" "Logging" ...
##  $ Veg_Rich               : num [1:24] 16.7 15.8 16.9 12.4 17.1 ...
##  $ Veg_Stems              : num [1:24] 31.2 37.4 32.3 29.4 36 ...
##  $ Veg_liana              : num [1:24] 5.78 13.25 4.75 9.78 13.25 ...
##  $ Veg_DBH                : num [1:24] 49.6 34.6 42.8 36.6 41.5 ...
##  $ Veg_Canopy             : num [1:24] 3.78 3.75 3.43 3.75 3.88 2.5 4 4 3 3.25 ...
##  $ Veg_Understory         : num [1:24] 2.89 3.88 3 2.75 3.25 3 2.38 2.71 3.25 3.13 ...
##  $ RA_Apes                : num [1:24] 1.87 0 4.49 12.93 0 ...
##  $ RA_Birds               : num [1:24] 52.7 52.2 37.4 59.3 52.6 ...
##  $ RA_Elephant            : num [1:24] 0 0.86 1.33 0.56 1 0 1.11 0.43 2.2 0 ...
##  $ RA_Monkeys             : num [1:24] 38.6 28.5 41.8 19.9 41.3 ...
##  $ RA_Rodent              : num [1:24] 4.22 6.04 1.06 3.66 2.52 1.83 3.1 1.26 4.37 6.31 ...
##  $ RA_Ungulate            : num [1:24] 2.66 12.41 13.86 3.71 2.53 ...
##  $ Rich_AllSpecies        : num [1:24] 22 20 22 19 20 22 23 19 19 19 ...
##  $ Evenness_AllSpecies    : num [1:24] 0.793 0.773 0.74 0.681 0.811 0.786 0.818 0.757 0.773 0.668 ...
##  $ Diversity_AllSpecies   : num [1:24] 2.45 2.31 2.29 2.01 2.43 ...
##  $ Rich_BirdSpecies       : num [1:24] 11 10 11 8 8 10 11 11 11 9 ...
##  $ Evenness_BirdSpecies   : num [1:24] 0.732 0.704 0.688 0.559 0.799 0.771 0.801 0.687 0.784 0.573 ...
##  $ Diversity_BirdSpecies  : num [1:24] 1.76 1.62 1.65 1.16 1.66 ...
##  $ Rich_MammalSpecies     : num [1:24] 11 10 11 11 12 12 12 8 8 10 ...
##  $ Evenness_MammalSpecies : num [1:24] 0.736 0.705 0.65 0.619 0.736 0.694 0.776 0.79 0.821 0.783 ...
##  $ Diversity_MammalSpecies: num [1:24] 1.76 1.62 1.56 1.48 1.83 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   TransectID = col_double(),
##   ..   Distance = col_double(),
##   ..   HuntCat = col_character(),
##   ..   NumHouseholds = col_double(),
##   ..   LandUse = col_character(),
##   ..   Veg_Rich = col_double(),
##   ..   Veg_Stems = col_double(),
##   ..   Veg_liana = col_double(),
##   ..   Veg_DBH = col_double(),
##   ..   Veg_Canopy = col_double(),
##   ..   Veg_Understory = col_double(),
##   ..   RA_Apes = col_double(),
##   ..   RA_Birds = col_double(),
##   ..   RA_Elephant = col_double(),
##   ..   RA_Monkeys = col_double(),
##   ..   RA_Rodent = col_double(),
##   ..   RA_Ungulate = col_double(),
##   ..   Rich_AllSpecies = col_double(),
##   ..   Evenness_AllSpecies = col_double(),
##   ..   Diversity_AllSpecies = col_double(),
##   ..   Rich_BirdSpecies = col_double(),
##   ..   Evenness_BirdSpecies = col_double(),
##   ..   Diversity_BirdSpecies = col_double(),
##   ..   Rich_MammalSpecies = col_double(),
##   ..   Evenness_MammalSpecies = col_double(),
##   ..   Diversity_MammalSpecies = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```
to factors

```r
vertebrates$HuntCat <- as.factor(vertebrates$HuntCat)
class(vertebrates$HuntCat)
```

```
## [1] "factor"
```

```r
vertebrates$LandUse <- as.factor(vertebrates$LandUse)
class(vertebrates$LandUse)
```

```
## [1] "factor"
```

cleaning the names

```r
vertebrates <- clean_names(vertebrates)
```


```r
names(vertebrates)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```

```r
head(vertebrates)
```

```
## # A tibble: 6 × 26
##   transect_id distance hunt_cat num_households land_use veg_rich veg_stems
##         <dbl>    <dbl> <fct>             <dbl> <fct>       <dbl>     <dbl>
## 1           1     7.14 Moderate             54 Park         16.7      31.2
## 2           2    17.3  None                 54 Park         15.8      37.4
## 3           2    18.3  None                 29 Park         16.9      32.3
## 4           3    20.8  None                 29 Logging      12.4      29.4
## 5           4    16.0  None                 29 Park         17.1      36  
## 6           5    17.5  None                 29 Park         16.5      29.2
## # … with 19 more variables: veg_liana <dbl>, veg_dbh <dbl>, veg_canopy <dbl>,
## #   veg_understory <dbl>, ra_apes <dbl>, ra_birds <dbl>, ra_elephant <dbl>,
## #   ra_monkeys <dbl>, ra_rodent <dbl>, ra_ungulate <dbl>,
## #   rich_all_species <dbl>, evenness_all_species <dbl>,
## #   diversity_all_species <dbl>, rich_bird_species <dbl>,
## #   evenness_bird_species <dbl>, diversity_bird_species <dbl>,
## #   rich_mammal_species <dbl>, evenness_mammal_species <dbl>, …
```

10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

```r
vertebrates %>% 
  group_by(hunt_cat) %>%  
  filter(hunt_cat != "None") %>% 
  summarise(mean_diversity_bird_species = mean(diversity_bird_species), 
            mean_diversity_mammal_species = mean(diversity_mammal_species), 
            n=n())
```

```
## # A tibble: 2 × 4
##   hunt_cat mean_diversity_bird_species mean_diversity_mammal_species     n
##   <fct>                          <dbl>                         <dbl> <int>
## 1 High                            1.66                          1.74     7
## 2 Moderate                        1.62                          1.68     8
```


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  


```r
vertebrates %>% 
  filter(xor(distance <=3, distance >= 25)) %>% 
  group_by(distance) %>% 
  summarise(across(c(ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate), 
                   mean, na.rm=T))
```

```
## # A tibble: 3 × 7
##   distance ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##      <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1     2.7     0        85.0        0.29       9.09      3.74        1.86
## 2     2.92    0.24     68.2        0         25.6       4.05        1.88
## 3    26.8     4.91     31.6        0         54.1       1.29        8.12
```

12. (4 points) Based on your interest, do one exploratory analysis on the `vertebrate` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`

```r
vertebrates %>% 
  filter(xor(num_households <= 20, num_households >= 50)) %>% 
  group_by(num_households) %>% 
  summarise(across(c(ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate), 
                   mean, na.rm=T))
```

```
## # A tibble: 6 × 7
##   num_households ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##            <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1             13    0.12     61.9        0          32.0      3.51        2.46
## 2             17    0.51     57.4        2.3        35.1      2.09        2.56
## 3             19    0        57.8        0          37.8      3.19        1.04
## 4             54    1.86     53.6        0.33       33.7      3.83        6.73
## 5             56    0.46     66.6        0.52       26.7      4.54        1.27
## 6             73    1.96     70.0        0          20.4      4.12        3.26
```

