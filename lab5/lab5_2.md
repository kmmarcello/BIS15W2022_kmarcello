---
title: "dplyr Superhero"
date: "2022-01-20"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Breakout Rooms  
Please take 5-8 minutes to check over your answers to the HW in your group. If you are stuck, please remember that you can check the key in [Joel's repository](https://github.com/jmledford3115/BIS15LW2021_jledford).  

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab 5 today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. We will spend most of the time in our breakout rooms. Your lab 5 homework will be to knit and push this file to your repository.  

## Load the tidyverse

```r
library("tidyverse")
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

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
janitor::clean_names(superhero_info)
```

```
## # A tibble: 734 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… <NA>       good     
##  2 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  3 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  4 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  5 Abra… Male   blue      Cosm… Black          NA Marvel C… <NA>       bad      
##  6 Abso… Male   blue      Human No Hair       193 Marvel C… <NA>       bad      
##  7 Adam… Male   blue      <NA>  Blond          NA NBC - He… <NA>       good     
##  8 Adam… Male   blue      Human Blond         185 DC Comics <NA>       good     
##  9 Agen… Female blue      <NA>  Blond         173 Marvel C… <NA>       good     
## 10 Agen… Male   brown     Human Brown         178 Marvel C… <NA>       good     
## # … with 724 more rows, and 1 more variable: weight <dbl>
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 × 168
##   hero_names  Agility `Accelerated Healing` `Lantern Power Ri… `Dimensional Awa…
##   <chr>       <lgl>   <lgl>                 <lgl>              <lgl>            
## 1 3-D Man     TRUE    FALSE                 FALSE              FALSE            
## 2 A-Bomb      FALSE   TRUE                  FALSE              FALSE            
## 3 Abe Sapien  TRUE    TRUE                  FALSE              FALSE            
## 4 Abin Sur    FALSE   FALSE                 TRUE               FALSE            
## 5 Abomination FALSE   TRUE                  FALSE              FALSE            
## 6 Abraxas     FALSE   FALSE                 FALSE              TRUE             
## # … with 163 more variables: Cold Resistance <lgl>, Durability <lgl>,
## #   Stealth <lgl>, Energy Absorption <lgl>, Flight <lgl>, Danger Sense <lgl>,
## #   Underwater breathing <lgl>, Marksmanship <lgl>, Weapons Master <lgl>,
## #   Power Augmentation <lgl>, Animal Attributes <lgl>, Longevity <lgl>,
## #   Intelligence <lgl>, Super Strength <lgl>, Cryokinesis <lgl>,
## #   Telepathy <lgl>, Energy Armor <lgl>, Energy Blasts <lgl>,
## #   Duplication <lgl>, Size Changing <lgl>, Density Control <lgl>, …
```

```r
#janitor::clean_names(superhero_powers)
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

```r
superhero_info <- janitor::clean_names(superhero_info)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral_heros <- select(superhero_info, "name")
neutral_heros
```

```
## # A tibble: 734 × 1
##    name         
##    <chr>        
##  1 A-Bomb       
##  2 Abe Sapien   
##  3 Abin Sur     
##  4 Abomination  
##  5 Abraxas      
##  6 Absorbing Man
##  7 Adam Monroe  
##  8 Adam Strange 
##  9 Agent 13     
## 10 Agent Bob    
## # … with 724 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
some_heros <- select(superhero_info, "name", "alignment", "race")
some_heros
```

```
## # A tibble: 734 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
not_human_heros <- filter(superhero_info, race != "Human")
not_human_heros
```

```
## # A tibble: 222 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  2 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  3 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  4 Abra… Male   blue      Cosm… Black          NA Marvel C… <NA>       bad      
##  5 Ajax  Male   brown     Cybo… Black         193 Marvel C… <NA>       bad      
##  6 Alien Male   <NA>      Xeno… No Hair       244 Dark Hor… black      bad      
##  7 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  8 Angel Male   <NA>      Vamp… <NA>           NA Dark Hor… <NA>       good     
##  9 Ange… Female yellow    Muta… Black         165 Marvel C… <NA>       good     
## 10 Anti… Male   yellow    God … No Hair        61 DC Comics <NA>       bad      
## # … with 212 more rows, and 1 more variable: weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- filter(superhero_info, alignment == "good")
good_guys
```

```
## # A tibble: 496 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… <NA>       good     
##  2 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  3 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  4 Adam… Male   blue      <NA>  Blond          NA NBC - He… <NA>       good     
##  5 Adam… Male   blue      Human Blond         185 DC Comics <NA>       good     
##  6 Agen… Female blue      <NA>  Blond         173 Marvel C… <NA>       good     
##  7 Agen… Male   brown     Human Brown         178 Marvel C… <NA>       good     
##  8 Agen… Male   <NA>      <NA>  <NA>          191 Marvel C… <NA>       good     
##  9 Alan… Male   blue      <NA>  Blond         180 DC Comics <NA>       good     
## 10 Alex… Male   <NA>      <NA>  <NA>           NA NBC - He… <NA>       good     
## # … with 486 more rows, and 1 more variable: weight <dbl>
```

```r
bad_guys <- filter(superhero_info, alignment == "bad")
bad_guys
```

```
## # A tibble: 207 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  2 Abra… Male   blue      Cosm… Black          NA Marvel C… <NA>       bad      
##  3 Abso… Male   blue      Human No Hair       193 Marvel C… <NA>       bad      
##  4 Air-… Male   blue      <NA>  White         188 Marvel C… <NA>       bad      
##  5 Ajax  Male   brown     Cybo… Black         193 Marvel C… <NA>       bad      
##  6 Alex… Male   <NA>      Human <NA>           NA Wildstorm <NA>       bad      
##  7 Alien Male   <NA>      Xeno… No Hair       244 Dark Hor… black      bad      
##  8 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  9 Ammo  Male   brown     Human Black         188 Marvel C… <NA>       bad      
## 10 Ange… Female <NA>      <NA>  <NA>           NA Image Co… <NA>       bad      
## # … with 197 more rows, and 1 more variable: weight <dbl>
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```


```r
asgardians <- filter(good_guys, race == "Asgardian")
asgardians
```

```
## # A tibble: 3 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sif    Female blue      Asga… Black         188 Marvel C… <NA>       good     
## 2 Thor   Male   blue      Asga… Blond         198 Marvel C… <NA>       good     
## 3 Thor … Female blue      Asga… Blond         175 Marvel C… <NA>       good     
## # … with 1 more variable: weight <dbl>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
tall_bad_guys <- filter(bad_guys, height >= 200)
tall_bad_guys
```

```
## # A tibble: 25 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  2 Alien Male   <NA>      Xeno… No Hair       244 Dark Hor… black      bad      
##  3 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  4 Apoc… Male   red       Muta… Black         213 Marvel C… grey       bad      
##  5 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  6 Bloo… Female blue      Human Brown         218 Marvel C… <NA>       bad      
##  7 Dark… Male   red       New … No Hair       267 DC Comics grey       bad      
##  8 Doct… Male   brown     Human Brown         201 Marvel C… <NA>       bad      
##  9 Doct… Male   brown     <NA>  Brown         201 Marvel C… <NA>       bad      
## 10 Doom… Male   red       Alien White         244 DC Comics <NA>       bad      
## # … with 15 more rows, and 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)? more good guys

```r
table(good_guys$hair_color)
```

```
## 
##           Auburn            black            Black            blond 
##               10                3              108                2 
##            Blond             Blue            Brown    Brown / Black 
##               85                1               55                1 
##    Brown / White            Green             Grey           Indigo 
##                4                7                2                1 
##          Magenta          No Hair           Orange   Orange / White 
##                1               37                2                1 
##             Pink           Purple              Red      Red / White 
##                1                1               40                1 
##           Silver Strawberry Blond            White           Yellow 
##                3                4               10                2
```

```r
table(bad_guys$hair_color)
```

```
## 
##           Auburn            Black     Black / Blue            blond 
##                3               42                1                1 
##            Blond             Blue            Brown           Brownn 
##               11                1               27                1 
##             Gold            Green             Grey          No Hair 
##                1                1                3               35 
##           Purple              Red       Red / Grey     Red / Orange 
##                3                9                1                1 
## Strawberry Blond            White 
##                3               10
```
10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight over 300?

```r
big_heros <- filter(superhero_info, between(weight, 200, 300))
big_heros
```

```
## # A tibble: 23 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Ares  Male   brown     <NA>  Brown         185 Marvel C… <NA>       good     
##  2 Beta… Male   <NA>      <NA>  No Hair       201 Marvel C… <NA>       good     
##  3 Blob  Male   brown     <NA>  Brown         178 Marvel C… <NA>       bad      
##  4 Colo… Male   silver    Muta… Black         226 Marvel C… <NA>       good     
##  5 Etri… Male   red       Demon No Hair       193 DC Comics yellow     neutral  
##  6 Glad… Male   blue      Stro… Blue          198 Marvel C… purple     neutral  
##  7 Gori… Male   yellow    Gori… Black         198 DC Comics <NA>       bad      
##  8 Hela  Female green     Asga… Black         213 Marvel C… <NA>       bad      
##  9 Hype… Male   blue      Eter… Red           183 Marvel C… <NA>       good     
## 10 King… Male   blue      Human No Hair       201 Marvel C… <NA>       bad      
## # … with 13 more rows, and 1 more variable: weight <dbl>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
tall_heros <- filter(superhero_info, height >=300)
tall_heros
```

```
## # A tibble: 8 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin F… Male   red       Kaka… No Hair      975  Marvel C… green      good     
## 2 Galac… Male   black     Cosm… Black        876  Marvel C… <NA>       neutral  
## 3 Groot  Male   yellow    Flor… <NA>         701  Marvel C… <NA>       good     
## 4 MODOK  Male   white     Cybo… Brownn       366  Marvel C… <NA>       bad      
## 5 Onsla… Male   red       Muta… No Hair      305  Marvel C… <NA>       bad      
## 6 Sasqu… Male   red       <NA>  Orange       305  Marvel C… <NA>       good     
## 7 Wolfs… Female green     <NA>  Auburn       366  Marvel C… <NA>       good     
## 8 Ymir   Male   white     Fros… No Hair      305. Marvel C… white      good     
## # … with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
big_and_tall <- filter(superhero_info, height >=300 | weight >= 450)
big_and_tall
```

```
## # A tibble: 14 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Bloo… Female blue      Human Brown       218   Marvel C… <NA>       bad      
##  2 Dark… Male   red       New … No Hair     267   DC Comics grey       bad      
##  3 Fin … Male   red       Kaka… No Hair     975   Marvel C… green      good     
##  4 Gala… Male   black     Cosm… Black       876   Marvel C… <NA>       neutral  
##  5 Giga… Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
##  6 Groot Male   yellow    Flor… <NA>        701   Marvel C… <NA>       good     
##  7 Hulk  Male   green     Huma… Green       244   Marvel C… green      good     
##  8 Jugg… Male   blue      Human Red         287   Marvel C… <NA>       neutral  
##  9 MODOK Male   white     Cybo… Brownn      366   Marvel C… <NA>       bad      
## 10 Onsl… Male   red       Muta… No Hair     305   Marvel C… <NA>       bad      
## 11 Red … Male   yellow    Huma… Black       213   Marvel C… red        neutral  
## 12 Sasq… Male   red       <NA>  Orange      305   Marvel C… <NA>       good     
## 13 Wolf… Female green     <NA>  Auburn      366   Marvel C… <NA>       good     
## 14 Ymir  Male   white     Fros… No Hair     305.  Marvel C… white      good     
## # … with 1 more variable: weight <dbl>
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
tabyl(superhero_info, height)
```

```
##  height   n     percent valid_percent
##    15.2   1 0.001362398   0.001934236
##    30.5   2 0.002724796   0.003868472
##    61.0   1 0.001362398   0.001934236
##    62.5   1 0.001362398   0.001934236
##    64.0   1 0.001362398   0.001934236
##    66.0   1 0.001362398   0.001934236
##    71.0   1 0.001362398   0.001934236
##    79.0   1 0.001362398   0.001934236
##   108.0   1 0.001362398   0.001934236
##   122.0   2 0.002724796   0.003868472
##   137.0   2 0.002724796   0.003868472
##   140.0   1 0.001362398   0.001934236
##   142.0   1 0.001362398   0.001934236
##   155.0   3 0.004087193   0.005802708
##   157.0   5 0.006811989   0.009671180
##   160.0   1 0.001362398   0.001934236
##   163.0   8 0.010899183   0.015473888
##   165.0  26 0.035422343   0.050290135
##   168.0  29 0.039509537   0.056092843
##   170.0  26 0.035422343   0.050290135
##   173.0  17 0.023160763   0.032882012
##   175.0  34 0.046321526   0.065764023
##   178.0  39 0.053133515   0.075435203
##   180.0  38 0.051771117   0.073500967
##   183.0  59 0.080381471   0.114119923
##   185.0  35 0.047683924   0.067698259
##   188.0  51 0.069482289   0.098646035
##   191.0  21 0.028610354   0.040618956
##   193.0  21 0.028610354   0.040618956
##   196.0  11 0.014986376   0.021276596
##   198.0  18 0.024523161   0.034816248
##   201.0  11 0.014986376   0.021276596
##   203.0   5 0.006811989   0.009671180
##   206.0   2 0.002724796   0.003868472
##   211.0   5 0.006811989   0.009671180
##   213.0   7 0.009536785   0.013539652
##   218.0   3 0.004087193   0.005802708
##   226.0   3 0.004087193   0.005802708
##   229.0   3 0.004087193   0.005802708
##   234.0   1 0.001362398   0.001934236
##   244.0   4 0.005449591   0.007736944
##   257.0   1 0.001362398   0.001934236
##   259.0   1 0.001362398   0.001934236
##   267.0   1 0.001362398   0.001934236
##   279.0   2 0.002724796   0.003868472
##   287.0   1 0.001362398   0.001934236
##   297.0   1 0.001362398   0.001934236
##   304.8   1 0.001362398   0.001934236
##   305.0   2 0.002724796   0.003868472
##   366.0   2 0.002724796   0.003868472
##   701.0   1 0.001362398   0.001934236
##   876.0   1 0.001362398   0.001934236
##   975.0   1 0.001362398   0.001934236
##      NA 217 0.295640327            NA
```

```r
big_and_short <- filter(superhero_info, height <=100 | weight >= 450)
big_and_short
```

```
## # A tibble: 16 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Anti… Male   yellow    God … No Hair      61   DC Comics <NA>       bad      
##  2 Bloo… Female blue      Human Brown       218   Marvel C… <NA>       bad      
##  3 Bloo… Male   white     <NA>  No Hair      30.5 Marvel C… <NA>       bad      
##  4 Dark… Male   red       New … No Hair     267   DC Comics grey       bad      
##  5 Giga… Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
##  6 Howa… Male   brown     <NA>  Yellow       79   Marvel C… <NA>       good     
##  7 Hulk  Male   green     Huma… Green       244   Marvel C… green      good     
##  8 Jack… Male   blue      Human Brown        71   Dark Hor… <NA>       good     
##  9 Jugg… Male   blue      Human Red         287   Marvel C… <NA>       neutral  
## 10 King… Male   yellow    Anim… Black        30.5 <NA>      <NA>       good     
## 11 Kryp… Male   blue      Kryp… White        64   DC Comics <NA>       good     
## 12 Red … Male   yellow    Huma… Black       213   Marvel C… red        neutral  
## 13 Sasq… Male   red       <NA>  Orange      305   Marvel C… <NA>       good     
## 14 Utga… Male   blue      Fros… White        15.2 Marvel C… <NA>       bad      
## 15 Wolf… Female green     <NA>  Auburn      366   Marvel C… <NA>       good     
## 16 Yoda  Male   brown     Yoda… White        66   George L… green      good     
## # … with 1 more variable: weight <dbl>
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
head(superhero_powers)
```

```
## # A tibble: 6 × 168
##   hero_names  agility accelerated_healing lantern_power_ring dimensional_awaren…
##   <chr>       <lgl>   <lgl>               <lgl>              <lgl>              
## 1 3-D Man     TRUE    FALSE               FALSE              FALSE              
## 2 A-Bomb      FALSE   TRUE                FALSE              FALSE              
## 3 Abe Sapien  TRUE    TRUE                FALSE              FALSE              
## 4 Abin Sur    FALSE   FALSE               TRUE               FALSE              
## 5 Abomination FALSE   TRUE                FALSE              FALSE              
## 6 Abraxas     FALSE   FALSE               FALSE              TRUE               
## # … with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>, …
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?


## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?


16. Pick your favorite superhero and let's see their powers!
# from questions asked about this in class. filter your super hero name, pipe, select all the columns(variables) that equal TRUE. This will give you only the powers the superhero has.

```r
superhero_powers %>% 
  filter(hero_names == "Abraxas") %>% 
  select_if(all_vars(.=='TRUE'))
```

```
## Warning: The `.predicate` argument of `select_if()` can't contain quosures. as of dplyr 0.8.3.
## Please use a one-sided formula, a function, or a function name.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

```
## # A tibble: 1 × 14
##   dimensional_awar… flight intelligence super_strength size_changing super_speed
##   <lgl>             <lgl>  <lgl>        <lgl>          <lgl>         <lgl>      
## 1 TRUE              TRUE   TRUE         TRUE           TRUE          TRUE       
## # … with 8 more variables: teleportation <lgl>, magic <lgl>,
## #   dimensional_travel <lgl>, immortality <lgl>, invulnerability <lgl>,
## #   molecular_manipulation <lgl>, energy_manipulation <lgl>, power_cosmic <lgl>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
