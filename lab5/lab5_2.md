---
title: "dplyr Superhero"
date: "2022-01-21"
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
tabyl(superhero_info, alignment, race)
```

```
##  alignment Alien Alpha Amazon Android Animal Asgardian Atlantean Bizarro
##        bad     2     0      0       5      2         2         1       0
##       good     3     5      2       4      2         3         4       0
##    neutral     1     0      0       0      0         0         0       1
##       <NA>     1     0      0       0      0         0         0       0
##  Bolovaxian Clone Cosmic Entity Cyborg Czarnian Dathomirian Zabrak Demi-God
##           0     0             1      8        0                  1        0
##           1     1             0      3        0                  0        2
##           0     0             3      0        1                  0        0
##           0     0             0      0        0                  0        0
##  Demon Eternal Flora Colossus Frost Giant God / Eternal Gorilla Gungan Human
##      2       1              0           1             5       1      0    50
##      3       1              1           1             6       0      1   148
##      1       0              0           0             1       0      0     9
##      0       0              0           0             2       0      0     1
##  Human / Altered Human / Clone Human / Cosmic Human / Radiation Human-Kree
##                1             1              0                 2          0
##                2             0              2                 8          2
##                0             0              0                 1          0
##                0             0              0                 0          0
##  Human-Spartoi Human-Vulcan Human-Vuldarian Icthyo Sapien Inhuman Kaiju
##              0            0               0             0       0     1
##              1            1               1             1       4     0
##              0            0               0             0       0     0
##              0            0               0             0       0     0
##  Kakarantharaian Korugaran Kryptonian Luphomoid Maiar Martian Metahuman Mutant
##                0         0          3         1     1       0         1     12
##                1         0          4         0     0       1         1     46
##                0         1          0         0     0       0         0      4
##                0         0          0         0     0       0         0      1
##  Mutant / Clone New God Neyaphem Parademon Planet Rodian Saiyan Spartoi
##               0       3        1         1      0      1      1       1
##               1       0        0         0      1      0      1       0
##               0       0        0         0      0      0      0       0
##               0       0        0         0      0      0      0       0
##  Strontian Symbiote Talokite Tamaranean Ungaran Vampire Xenomorph XX121 Yautja
##          0        4        0          0       0       0               1      1
##          0        3        1          1       1       2               0      0
##          1        0        0          0       0       0               0      0
##          0        2        0          0       0       0               0      0
##  Yoda's species Zen-Whoberian Zombie NA_
##               0             0      1  87
##               1             1      0 217
##               0             0      0   0
##               0             0      0   0
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
neutral_heros <- superhero_info %>% 
  filter(alignment == "neutral") %>% 
  select(name)
neutral_heros
```

```
## # A tibble: 24 × 1
##    name        
##    <chr>       
##  1 Bizarro     
##  2 Black Flash 
##  3 Captain Cold
##  4 Copycat     
##  5 Deadpool    
##  6 Deathstroke 
##  7 Etrigan     
##  8 Galactus    
##  9 Gladiator   
## 10 Indigo      
## # … with 14 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% 
  select(alignment, name, race)
```

```
## # A tibble: 734 × 3
##    alignment name          race             
##    <chr>     <chr>         <chr>            
##  1 good      A-Bomb        Human            
##  2 good      Abe Sapien    Icthyo Sapien    
##  3 good      Abin Sur      Ungaran          
##  4 bad       Abomination   Human / Radiation
##  5 bad       Abraxas       Cosmic Entity    
##  6 bad       Absorbing Man Human            
##  7 good      Adam Monroe   <NA>             
##  8 good      Adam Strange  Human            
##  9 good      Agent 13      <NA>             
## 10 good      Agent Bob     Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info %>% 
  filter(race != "Human") %>% 
  select(name)
```

```
## # A tibble: 222 × 1
##    name        
##    <chr>       
##  1 Abe Sapien  
##  2 Abin Sur    
##  3 Abomination 
##  4 Abraxas     
##  5 Ajax        
##  6 Alien       
##  7 Amazo       
##  8 Angel       
##  9 Angel Dust  
## 10 Anti-Monitor
## # … with 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guy <- superhero_info %>% 
  filter(alignment == "good")
good_guy
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
bad_guy <- superhero_info %>% 
  filter(alignment == "bad")
bad_guy
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

```r
superhero_info %>% 
  filter(skin_color == "purple")
```

```
## # A tibble: 3 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Gladi… Male   blue      Stro… Blue          198 Marvel C… purple     neutral  
## 2 Purpl… Male   purple    Human Purple        180 Marvel C… purple     bad      
## 3 Thanos Male   red       Eter… No Hair       201 Marvel C… purple     bad      
## # … with 1 more variable: weight <dbl>
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guy, race)
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
asgardians <- filter(good_guy, race == "Asgardian")
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
superhero_info %>% 
  filter(alignment == "bad" & gender == "Male" & race == "Human" & height >= 200)
```

```
## # A tibble: 5 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bane   Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
## 2 Docto… Male   brown     Human Brown         201 Marvel C… <NA>       bad      
## 3 Kingp… Male   blue      Human No Hair       201 Marvel C… <NA>       bad      
## 4 Lizard Male   red       Human No Hair       203 Marvel C… <NA>       bad      
## 5 Scorp… Male   brown     Human Brown         211 Marvel C… <NA>       bad      
## # … with 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)? more good guys

```r
superhero_info %>% 
  filter(is.na(hair_color))
```

```
## # A tibble: 172 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Agen… Male   <NA>      <NA>  <NA>          191 Marvel C… <NA>       good     
##  2 Alex… Male   <NA>      Human <NA>           NA Wildstorm <NA>       bad      
##  3 Alex… Male   <NA>      <NA>  <NA>           NA NBC - He… <NA>       good     
##  4 Alla… Male   <NA>      <NA>  <NA>           NA Wildstorm <NA>       good     
##  5 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  6 Ando… Male   <NA>      <NA>  <NA>           NA NBC - He… <NA>       good     
##  7 Angel Male   <NA>      Vamp… <NA>           NA Dark Hor… <NA>       good     
##  8 Ange… Female <NA>      <NA>  <NA>           NA Image Co… <NA>       bad      
##  9 Anti… Male   <NA>      <NA>  <NA>           NA Image Co… <NA>       bad      
## 10 Arse… Male   <NA>      Human <NA>           NA DC Comics <NA>       good     
## # … with 162 more rows, and 1 more variable: weight <dbl>
```



```r
tabyl(superhero_info, hair_color, alignment)
```

```
##        hair_color bad good neutral NA_
##            Auburn   3   10       0   0
##             black   0    3       0   0
##             Black  42  108       8   0
##      Black / Blue   1    0       0   0
##             blond   1    2       0   0
##             Blond  11   85       1   2
##              Blue   1    1       1   0
##             Brown  27   55       4   0
##     Brown / Black   0    1       0   0
##     Brown / White   0    4       0   0
##            Brownn   1    0       0   0
##              Gold   1    0       0   0
##             Green   1    7       0   0
##              Grey   3    2       0   0
##            Indigo   0    1       0   0
##           Magenta   0    1       0   0
##           No Hair  35   37       3   0
##            Orange   0    2       0   0
##    Orange / White   0    1       0   0
##              Pink   0    1       0   0
##            Purple   3    1       1   0
##               Red   9   40       2   0
##        Red / Grey   1    0       0   0
##      Red / Orange   1    0       0   0
##       Red / White   0    1       0   0
##            Silver   0    3       0   1
##  Strawberry Blond   3    4       0   0
##             White  10   10       2   1
##            Yellow   0    2       0   0
##              <NA>  53  114       2   3
```



```r
tabyl(good_guy, hair_color)
```

```
##        hair_color   n     percent valid_percent
##            Auburn  10 0.020161290   0.026178010
##             black   3 0.006048387   0.007853403
##             Black 108 0.217741935   0.282722513
##             blond   2 0.004032258   0.005235602
##             Blond  85 0.171370968   0.222513089
##              Blue   1 0.002016129   0.002617801
##             Brown  55 0.110887097   0.143979058
##     Brown / Black   1 0.002016129   0.002617801
##     Brown / White   4 0.008064516   0.010471204
##             Green   7 0.014112903   0.018324607
##              Grey   2 0.004032258   0.005235602
##            Indigo   1 0.002016129   0.002617801
##           Magenta   1 0.002016129   0.002617801
##           No Hair  37 0.074596774   0.096858639
##            Orange   2 0.004032258   0.005235602
##    Orange / White   1 0.002016129   0.002617801
##              Pink   1 0.002016129   0.002617801
##            Purple   1 0.002016129   0.002617801
##               Red  40 0.080645161   0.104712042
##       Red / White   1 0.002016129   0.002617801
##            Silver   3 0.006048387   0.007853403
##  Strawberry Blond   4 0.008064516   0.010471204
##             White  10 0.020161290   0.026178010
##            Yellow   2 0.004032258   0.005235602
##              <NA> 114 0.229838710            NA
```

```r
table(bad_guy$hair_color)
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
big_heros <- superhero_info %>% 
  filter(xor(weight >= 300, height >= 200))
big_heros
```

```
## # A tibble: 37 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Alien Male   <NA>      Xeno… No Hair       244 Dark Hor… black      bad      
##  2 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  3 Ant-… Male   blue      Human Blond         211 Marvel C… <NA>       good     
##  4 Apoc… Male   red       Muta… Black         213 Marvel C… grey       bad      
##  5 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  6 Beta… Male   <NA>      <NA>  No Hair       201 Marvel C… <NA>       good     
##  7 Cable Male   blue      Muta… White         203 Marvel C… <NA>       good     
##  8 Cent… Male   white     Alien White         201 Marvel C… grey       good     
##  9 Cloak Male   brown     <NA>  black         226 Marvel C… <NA>       good     
## 10 Colo… Male   silver    Muta… Black         226 Marvel C… <NA>       good     
## # … with 27 more rows, and 1 more variable: weight <dbl>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
tall_heros <- superhero_info %>% 
  filter(height >= 200)
tall_heros
```

```
## # A tibble: 59 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… <NA>       good     
##  2 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  3 Alien Male   <NA>      Xeno… No Hair       244 Dark Hor… black      bad      
##  4 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  5 Ant-… Male   blue      Human Blond         211 Marvel C… <NA>       good     
##  6 Anti… Male   blue      Symb… Blond         229 Marvel C… <NA>       <NA>     
##  7 Apoc… Male   red       Muta… Black         213 Marvel C… grey       bad      
##  8 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  9 Beta… Male   <NA>      <NA>  No Hair       201 Marvel C… <NA>       good     
## 10 Bloo… Female blue      Human Brown         218 Marvel C… <NA>       bad      
## # … with 49 more rows, and 1 more variable: weight <dbl>
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
superhero_info %>% 
  select(name, height, weight) %>% 
  mutate(hw_ratio = height/weight) %>% 
  arrange(hw_ratio)
```

```
## # A tibble: 734 × 4
##    name        height weight hw_ratio
##    <chr>        <dbl>  <dbl>    <dbl>
##  1 Giganta       62.5    630   0.0992
##  2 Utgard-Loki   15.2     58   0.262 
##  3 Darkseid     267      817   0.327 
##  4 Juggernaut   287      855   0.336 
##  5 Red Hulk     213      630   0.338 
##  6 Sasquatch    305      900   0.339 
##  7 Hulk         244      630   0.387 
##  8 Bloodaxe     218      495   0.440 
##  9 Thanos       201      443   0.454 
## 10 A-Bomb       203      441   0.460 
## # … with 724 more rows
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

```r
superhero_powers %>% 
  filter(accelerated_healing == TRUE & durability == TRUE & super_strength == TRUE) %>% 
  select(hero_names)
```

```
## # A tibble: 97 × 1
##    hero_names  
##    <chr>       
##  1 A-Bomb      
##  2 Abe Sapien  
##  3 Angel       
##  4 Anti-Monitor
##  5 Anti-Venom  
##  6 Aquaman     
##  7 Arachne     
##  8 Archangel   
##  9 Ardina      
## 10 Ares        
## # … with 87 more rows
```

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
kinesis <- superhero_powers %>% 
  select(hero_names, contains("kinesis")) %>% 
  filter(across(contains("kinesis"), .==T))
  #filter(cryokinesis==T | electrokinesis==T | telekinesis==T | hyperkinesis==T | thirstokinesis
#==T | biokinesis==T | terrakinesis==T | vitakinesis==T)
kinesis
```
this is way too hard.

```r
#kinesis <- superhero_powers %>% 
  #select(hero_names, contains("kinesis")) %>% 
  #filter(across(contains("kinesis"), any()))
```


16. Pick your favorite superhero and let's see their powers!
# from questions asked about this in class. filter your super hero name, pipe, select all the columns(variables) that equal TRUE. This will give you only the powers the superhero has.

```r
superhero_powers %>% 
  filter(hero_names == "Purple Man") %>% 
  select_if(all_vars(.=='TRUE'))
```

```
## Warning: The `.predicate` argument of `select_if()` can't contain quosures. as of dplyr 0.8.3.
## Please use a one-sided formula, a function, or a function name.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

```
## # A tibble: 1 × 5
##   accelerated_healing telepathy substance_secretion mind_control hypnokinesis
##   <lgl>               <lgl>     <lgl>               <lgl>        <lgl>       
## 1 TRUE                TRUE      TRUE                TRUE         TRUE
```

```r
superhero_powers %>% 
  filter(hero_names == "Chuck Norris") %>% 
  select_if(all_vars(.=='TRUE'))
```

```
## # A tibble: 1 × 1
##   peak_human_condition
##   <lgl>               
## 1 TRUE
```


```r
superhero_powers %>% 
  select(hero_names, substance_secretion) %>% 
  filter(substance_secretion ==T)
```

```
## # A tibble: 17 × 2
##    hero_names    substance_secretion
##    <chr>         <lgl>              
##  1 Alex Mercer   TRUE               
##  2 Alien         TRUE               
##  3 Fin Fang Foom TRUE               
##  4 Hellgramite   TRUE               
##  5 Hybrid        TRUE               
##  6 Jack-Jack     TRUE               
##  7 Man-Thing     TRUE               
##  8 Maya Herrera  TRUE               
##  9 Poison Ivy    TRUE               
## 10 Purple Man    TRUE               
## 11 Quill         TRUE               
## 12 Silk          TRUE               
## 13 Spider-Gwen   TRUE               
## 14 Spider-Man    TRUE               
## 15 Sunspot       TRUE               
## 16 Toad          TRUE               
## 17 Toxin         TRUE
```

```r
superhero_powers %>% 
  select(hero_names, peak_human_condition) %>% 
  filter(peak_human_condition ==T)
```

```
## # A tibble: 30 × 2
##    hero_names      peak_human_condition
##    <chr>           <lgl>               
##  1 Batgirl IV      TRUE                
##  2 Batman          TRUE                
##  3 Batman II       TRUE                
##  4 Black Lightning TRUE                
##  5 Black Manta     TRUE                
##  6 Black Panther   TRUE                
##  7 Black Widow     TRUE                
##  8 Captain America TRUE                
##  9 Catwoman        TRUE                
## 10 Chuck Norris    TRUE                
## # … with 20 more rows
```
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
