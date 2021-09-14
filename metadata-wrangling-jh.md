---
title: 'Metadata Wrangling!'
author: "Jackson Hughes"
date: "9/13/2021"
output: 
  html_document:
    keep_md: yes
---




```r
metadata_0 <- read_csv("All Metadata CHEM 142 MC.csv")
```

```
## Warning: Missing column names filled in: 'X3' [3], 'X4' [4], 'X5' [5], 'X6' [6],
## 'X7' [7], 'X8' [8], 'X9' [9], 'X10' [10], 'X11' [11], 'X12' [12]
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   `1` = col_double(),
##   C = col_character(),
##   X3 = col_character(),
##   X4 = col_logical(),
##   X5 = col_logical(),
##   X6 = col_logical(),
##   X7 = col_logical(),
##   X8 = col_logical(),
##   X9 = col_logical(),
##   X10 = col_logical(),
##   X11 = col_logical(),
##   X12 = col_logical()
## )
```


```r
metadata_1 <- remove_empty(metadata_0)
```

```
## value for "which" not specified, defaulting to c("rows", "cols")
```

```r
metadata_2 <- metadata_1 %>% add_row(.before = 1)
```


```r
metadata_2[1,1] <- 1
metadata_2[1,2] <- "C"
metadata <- metadata_2 %>% 
  rename("X1" = "1") %>% 
  rename("X2" = "C")
```


```r
for (i in 1:nrow(metadata)) {
  if(str_detect(metadata[[2]][[i]], "[[:alpha:]]{1}")){
    replace(metadata[[3]][[i]], i, "metadata[[2]][[i]]")
    replace(metadata[[2]][[i]], i, "metadata[[1]][[i]]")}
}
```


```r
tibble(metadata)
```

```
## # A tibble: 3,141 x 3
##       X1 X2            X3                               
##    <dbl> <chr>         <chr>                            
##  1     1 C             <NA>                             
##  2    NA Z-7e-Chapter    13                             
##  3    NA activate-exam   1                              
##  4    NA bloom-rating    3                              
##  5    NA instructor      inst1                          
##  6    NA qtr             A18                            
##  7    NA ques-code       ML-083                         
##  8    NA topic           Formal Charge; Lewis Structures
##  9     2 E             <NA>                             
## 10    NA Z-7e-Chapter    4                              
## # ... with 3,131 more rows
```

#### This doesn't work! I think I understand the structure of the loop and the `if` function, but I need some guidance on how to go about shifting the value of the question number and question answer over one cell. My current approach above is just attempting to change the values of the cells themselves to whatever value is to the left of that cell using `replace`. But... what I tried didn't work :/
