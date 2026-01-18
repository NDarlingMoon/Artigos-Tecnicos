
```r
click_economia_dataset$rede <- gsub("\\batacadista\\b|\\bhipermercado\\b|\\bhipermercados\\b|\\bsupermercado\\b|\\bsupermercados\\b|\\bsuper\\b","", click_economia_dataset$rede, ignore.case = TRUE) %>% str_squish()

```
