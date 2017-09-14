# Reading with haven

Packages like:

* SAS: statistical, business analytics
* STATA: economist
* SPSS: social sciences

haven can deal with SAS, STATA and SPSS. The end result is a R data frame

```
install.package("haven")
library(haven)
```

**SAS \(.sas7dbdat, .sas7bcat\)**

`read_sas()` reads files and as a result has also labels inside.

##### STATA \(.dta\)

`read_stata()`_, _`read_dta()` reads STATA file. Converts numbers

`as_factor()`_ will convert as factor some values i.e. _

`as_factor(ontime$Airline)`

`as.character(as_factor(ontime$Airline)`

##### 

##### SPSS \(.sav, .por\)

`read_spss()`

`read_por()`

`read_sav()`

```
> # Load the haven package
> library(haven)
> 
> # Import sales.sas7bdat: sales
> sales <- read_sas("sales.sas7bdat")
> 
> # Display the structure of sales
> str(sales)
Classes 'tbl_df', 'tbl' and 'data.frame':    431 obs. of  4 variables:
 $ purchase: num  0 0 1 1 0 0 0 0 0 0 ...
 $ age     : num  41 47 41 39 32 32 33 45 43 40 ...
 $ gender  : chr  "Female" "Female" "Female" "Female" ...
 $ income  : chr  "Low" "Low" "Low" "Low" ...
 - attr(*, "label")= chr "SALES"
```

```
> sugar <- read_dta("http://assets.datacamp.com/production/course_1478/datasets/trade.dta")
> str(sugar)
Classes ‘tbl_df’, ‘tbl’ and 'data.frame':    10 obs. of  5 variables:
 $ Date    :Class 'labelled'  atomic [1:10] 10 9 8 7 6 5 4 3 2 1
  .. ..- attr(*, "label")= chr "Date"
  .. ..- attr(*, "format.stata")= chr "%9.0g"
  .. ..- attr(*, "labels")= Named num [1:10] 1 2 3 4 5 6 7 8 9 10
  .. .. ..- attr(*, "names")= chr [1:10] "2004-12-31" "2005-12-31" "2006-12-31" "2007-12-31" ...
 $ Import  : atomic  37664782 16316512 11082246 35677943 9879878 ...
  ..- attr(*, "label")= chr "Import"
  ..- attr(*, "format.stata")= chr "%9.0g"
 $ Weight_I: atomic  54029106 21584365 14526089 55034932 14806865 ...
  ..- attr(*, "label")= chr "Weight_I"
  ..- attr(*, "format.stata")= chr "%9.0g"
 $ Export  : atomic  5.45e+07 1.03e+08 3.79e+07 4.85e+07 7.15e+07 ...
  ..- attr(*, "label")= chr "Export"
  ..- attr(*, "format.stata")= chr "%9.0g"
 $ Weight_E: atomic  9.34e+07 1.58e+08 8.80e+07 1.12e+08 1.32e+08 ...
  ..- attr(*, "label")= chr "Weight_E"
  ..- attr(*, "format.stata")= chr "%9.0g"
 - attr(*, "label")= chr "Written by R."
> sugar$Date <- as.Date(as_factor(sugar$Date))
> str(sugar)
Classes ‘tbl_df’, ‘tbl’ and 'data.frame':    10 obs. of  5 variables:
 $ Date    : Date, format: "2013-12-31" "2012-12-31" ...
 $ Import  : atomic  37664782 16316512 11082246 35677943 9879878 ...
  ..- attr(*, "label")= chr "Import"
  ..- attr(*, "format.stata")= chr "%9.0g"
 $ Weight_I: atomic  54029106 21584365 14526089 55034932 14806865 ...
  ..- attr(*, "label")= chr "Weight_I"
  ..- attr(*, "format.stata")= chr "%9.0g"
 $ Export  : atomic  5.45e+07 1.03e+08 3.79e+07 4.85e+07 7.15e+07 ...
  ..- attr(*, "label")= chr "Export"
  ..- attr(*, "format.stata")= chr "%9.0g"
 $ Weight_E: atomic  9.34e+07 1.58e+08 8.80e+07 1.12e+08 1.32e+08 ...
  ..- attr(*, "label")= chr "Weight_E"
  ..- attr(*, "format.stata")= chr "%9.0g"
 - attr(*, "label")= chr "Written by R."
```

Plot variables

```
> plot(sugar$Import, sugar$Weight_I)
```

```
> # Import person.sav: traits
> traits <- read_sav("person.sav")
> 
> # Summarize traits
> summary(traits)
    Neurotic      Extroversion   Agreeableness   Conscientiousness
 Min.   : 0.00   Min.   : 5.00   Min.   :15.00   Min.   : 7.00    
 1st Qu.:18.00   1st Qu.:26.00   1st Qu.:39.00   1st Qu.:25.00    
 Median :24.00   Median :31.00   Median :45.00   Median :30.00    
 Mean   :23.63   Mean   :30.23   Mean   :44.55   Mean   :30.85    
 3rd Qu.:29.00   3rd Qu.:34.00   3rd Qu.:50.00   3rd Qu.:36.00    
 Max.   :44.00   Max.   :65.00   Max.   :73.00   Max.   :58.00    
 NA's   :14      NA's   :16      NA's   :19      NA's   :14
> 
> # Print out a subset greater than 40
> subset(traits, Extroversion > 40 & Agreeableness >40)
# A tibble: 8 × 4
  Neurotic Extroversion Agreeableness Conscientiousness
     <dbl>        <dbl>         <dbl>             <dbl>
1       38           43            49                29
2       20           42            46                31
3       18           42            49                31
4       42           43            44                29
5       30           42            51                24
6       18           42            50                25
7       27           45            55                23
8       18           43            57                34
```

```
> # haven is already loaded
> 
> # Import SPSS data from the URL: work
> work <- read_sav("http://s3.amazonaws.com/assets.datacamp.com/production/course_1478/datasets/employee.sav")
> 
> # Display summary of work$GENDER
> summary(work$GENDER)
   Length     Class      Mode 
      474  labelled character
> 
> 
> # Convert work$GENDER to a factor
> work$GENDER <- as_factor(work$GENDER)
> 
> 
> # Display summary of work$GENDER again
> summary(work$GENDER)
Female   Male 
   216    258
```



