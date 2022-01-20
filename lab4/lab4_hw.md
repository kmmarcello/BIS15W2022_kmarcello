---
title: "Lab 4 Homework"
author: "Kaylah Marcello"
date: "2022-01-20"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```

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

```r
janitor::clean_names(homerange)
```

```
## # A tibble: 569 × 24
##    taxon  common_name   class   order   family genus species primarymethod n    
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
## # … with 559 more rows, and 15 more variables: mean_mass_g <dbl>,
## #   log10_mass <dbl>, alternative_mass_reference <chr>, mean_hra_m2 <dbl>,
## #   log10_hra <dbl>, hra_reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic_guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10_preymass <dbl>, ppmr <dbl>, prey_size_reference <chr>
```

```r
colnames(homerange)
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

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  
in the homerange data in the taxon variable I want you to change the taxon in homerange to factor. same with order.

```r
homerange$taxon <- as.factor(homerange$taxon)
```

```r
homerange$order <- as.factor(homerange$order)
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa <- select(homerange, "taxon", "common.name", "class", "order", "family", "genus", "species")
taxa
```

```
## # A tibble: 569 × 7
##    taxon         common.name             class   order   family   genus  species
##    <fct>         <chr>                   <chr>   <fct>   <chr>    <chr>  <chr>  
##  1 lake fishes   american eel            actino… anguil… anguill… angui… rostra…
##  2 river fishes  blacktail redhorse      actino… cyprin… catosto… moxos… poecil…
##  3 river fishes  central stoneroller     actino… cyprin… cyprini… campo… anomal…
##  4 river fishes  rosyside dace           actino… cyprin… cyprini… clino… fundul…
##  5 river fishes  longnose dace           actino… cyprin… cyprini… rhini… catara…
##  6 river fishes  muskellunge             actino… esocif… esocidae esox   masqui…
##  7 marine fishes pollack                 actino… gadifo… gadidae  polla… pollac…
##  8 marine fishes saithe                  actino… gadifo… gadidae  polla… virens 
##  9 marine fishes lined surgeonfish       actino… percif… acanthu… acant… lineat…
## 10 marine fishes orangespine unicornfish actino… percif… acanthu… naso   litura…
## # … with 559 more rows
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```
**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivores <- filter(homerange, trophic.guild == "carnivore")
carnivores
```

```
## # A tibble: 342 × 24
##    taxon   common.name   class   order  family genus species primarymethod N    
##    <fct>   <chr>         <chr>   <fct>  <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake f… american eel  actino… angui… angui… angu… rostra… telemetry     16   
##  2 river … blacktail re… actino… cypri… catos… moxo… poecil… mark-recaptu… <NA> 
##  3 river … central ston… actino… cypri… cypri… camp… anomal… mark-recaptu… 20   
##  4 river … rosyside dace actino… cypri… cypri… clin… fundul… mark-recaptu… 26   
##  5 river … longnose dace actino… cypri… cypri… rhin… catara… mark-recaptu… 17   
##  6 river … muskellunge   actino… esoci… esoci… esox  masqui… telemetry     5    
##  7 marine… pollack       actino… gadif… gadid… poll… pollac… telemetry     2    
##  8 marine… saithe        actino… gadif… gadid… poll… virens  telemetry     2    
##  9 marine… giant treval… actino… perci… caran… cara… ignobi… telemetry     4    
## 10 lake f… rock bass     actino… perci… centr… ambl… rupest… mark-recaptu… 16   
## # … with 332 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

```r
herbivores <- filter(homerange, trophic.guild == "herbivore")
herbivores
```

```
## # A tibble: 227 × 24
##    taxon  common.name   class   order  family  genus species primarymethod N    
##    <fct>  <chr>         <chr>   <fct>  <chr>   <chr> <chr>   <chr>         <chr>
##  1 marin… lined surgeo… actino… perci… acanth… acan… lineat… direct obser… <NA> 
##  2 marin… orangespine … actino… perci… acanth… naso  litura… telemetry     8    
##  3 marin… bluespine un… actino… perci… acanth… naso  unicor… telemetry     7    
##  4 marin… redlip blenny actino… perci… blenni… ophi… atlant… direct obser… 20   
##  5 marin… bermuda chub  actino… perci… kyphos… kyph… sectat… telemetry     11   
##  6 marin… cherubfish    actino… perci… pomaca… cent… argi    direct obser… <NA> 
##  7 marin… damselfish    actino… perci… pomace… chro… chromis direct obser… <NA> 
##  8 marin… twinspot dam… actino… perci… pomace… chry… biocel… direct obser… 18   
##  9 marin… wards damsel  actino… perci… pomace… poma… wardi   direct obser… <NA> 
## 10 marin… australian g… actino… perci… pomace… steg… apical… direct obser… <NA> 
## # … with 217 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

```r
mean(carnivores$mean.hra.m2, na.rm=T)
```

```
## [1] 13039918
```

```r
mean(herbivores$mean.hra.m2, na.rm=T)
```

```
## [1] 34137012
```

**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

```r
colnames(homerange)
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


```r
deer <- homerange %>%
  select("mean.mass.g", "log10.mass", "family", "genus", "species") %>%
  filter(family == "cervidae") %>%
  arrange(desc(log10.mass))
deer
```

```
## # A tibble: 12 × 5
##    mean.mass.g log10.mass family   genus      species    
##          <dbl>      <dbl> <chr>    <chr>      <chr>      
##  1     307227.       5.49 cervidae alces      alces      
##  2     234758.       5.37 cervidae cervus     elaphus    
##  3     102059.       5.01 cervidae rangifer   tarandus   
##  4      87884.       4.94 cervidae odocoileus virginianus
##  5      71450.       4.85 cervidae dama       dama       
##  6      62823.       4.80 cervidae axis       axis       
##  7      53864.       4.73 cervidae odocoileus hemionus   
##  8      35000.       4.54 cervidae ozotoceros bezoarticus
##  9      29450.       4.47 cervidae cervus     nippon     
## 10      24050.       4.38 cervidae capreolus  capreolus  
## 11      13500.       4.13 cervidae muntiacus  reevesi    
## 12       7500.       3.88 cervidae pudu       puda
```


**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
