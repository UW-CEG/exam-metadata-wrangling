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
## Warning: Missing column names filled in: 'X3' [3]
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   `1` = col_double(),
##   C = col_character(),
##   X3 = col_character()
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
metadata_3 <- metadata_2 %>% 
  rename("X1" = "1") %>% 
  rename("X2" = "C")
```


```r
for (i in 1:nrow(metadata_3)) {
  if(str_detect(metadata_3[[2]][[i]], "^[[:alpha:]]{1}$")){
    metadata_3[[3]][[i]] <- metadata_3[[2]][[i]]
    metadata_3[[2]][[i]] <- metadata_3[[1]][[i]]}
}

for (i in 1:nrow(metadata_3)) {
  if(str_detect(metadata_3[[2]][[i]], "^[[:digit:]]{1,3}$")){
    metadata_3[[2]][[i]] <- "answer"}
}
```


```r
tibble(metadata_3)
```

```
## # A tibble: 3,141 x 3
##       X1 X2            X3                               
##    <dbl> <chr>         <chr>                            
##  1     1 answer        C                                
##  2    NA Z-7e-Chapter    13                             
##  3    NA activate-exam   1                              
##  4    NA bloom-rating    3                              
##  5    NA instructor      inst1                          
##  6    NA qtr             A18                            
##  7    NA ques-code       ML-083                         
##  8    NA topic           Formal Charge; Lewis Structures
##  9     2 answer        E                                
## 10    NA Z-7e-Chapter    4                              
## # ... with 3,131 more rows
```

### Repeat question numbers down dataset


```r
metadata_3[[1]][[nrow(metadata_3)]] <- metadata_3[[1]][[nrow(metadata_3)-7]]

for (i in 1:(nrow(metadata_3)-1)) {
  if(str_detect(metadata_3[[1]][[i]], "^[[:digit:]]{1,3}$")){
    current_digit <- metadata_3[[1]][[i]]
    j <- 1
    while (is.na(metadata_3[[1]][[i+j]])) {
      metadata_3[[1]][[i+j]] <- current_digit
      j <- j+1
    }
  }
}

tibble(metadata_3)
```

```
## # A tibble: 3,141 x 3
##       X1 X2            X3                               
##    <dbl> <chr>         <chr>                            
##  1     1 answer        C                                
##  2     1 Z-7e-Chapter    13                             
##  3     1 activate-exam   1                              
##  4     1 bloom-rating    3                              
##  5     1 instructor      inst1                          
##  6     1 qtr             A18                            
##  7     1 ques-code       ML-083                         
##  8     1 topic           Formal Charge; Lewis Structures
##  9     2 answer        E                                
## 10     2 Z-7e-Chapter    4                              
## # ... with 3,131 more rows
```

### Make data wide


```r
metadata_4 <- pivot_wider(metadata_3, names_from = X2, values_from = X3) %>% 
  rename("question" = "X1")

tibble(metadata_4)
```

```
## # A tibble: 404 x 10
##    question answer `Z-7e-Chapter` `activate-exam` `bloom-rating` instructor
##       <dbl> <chr>  <chr>          <chr>           <chr>          <chr>     
##  1        1 C        13             1               3              inst1   
##  2        2 E        4              2               3              inst3   
##  3        3 C        4              2               2              inst1   
##  4        4 E        3              2               3              inst1   
##  5        5 B        15             2               3              inst1   
##  6        6 E        12             1               2              inst1   
##  7        7 C        15             Final           5              inst3   
##  8        8 D        3              2               3              inst1   
##  9        9 B        3              2               3+             inst1   
## 10       10 E        15             2               3              inst1   
## # ... with 394 more rows, and 4 more variables: qtr <chr>, `ques-code` <chr>,
## #   topic <chr>, `0 ISSUES` <chr>
```

### Save data frame to file


```r
# Save as rds
write_rds(metadata_4, here("all_metadata_CHEM142_wide.rds"))

# Save as csv
write_csv(metadata_4, here("all_metadata_CHEM142_wide.csv"))
```

