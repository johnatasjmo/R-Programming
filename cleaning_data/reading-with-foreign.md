# Reading with foreign package

Can handle:

##### SAS

.xport

##### STAT

STATA 5 to 12 with read.dta\(\) . Default `convert.factors = TRUE`, `conver.dates = TRUE`, `missing.type = FALSE`

```r
read_dta(file, encoding = NULL)
read_stata(file, encoding = NULL)
write_dta(data, path, version = 14)
```

##### SPSS

```r
read.spss(file, 
    use.value.lablels = TRUE,
    to.data.frame = FALSE)
```

```r
read_sav(file, user_na = FALSE)

read_por(file, user_na = FALSE)

write_sav(data, path)

read_spss(file, user_na = FALSE)
```



