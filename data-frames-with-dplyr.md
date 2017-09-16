# Data Frames with dplyr

specific for data frames

* one observation per row
* each column a variable of characteristic
* use is default R implementation
* Other can be used for relational databaseies 
* simplifies existing functionalities and faster

Verbs

select, ilter, arrange, rename, mutate, summarise/summarize

So takes a dataframe, formated and outputs a dataframe

```
library(dplyr)
fileURL <- "https://github.com/DataScienceSpecialization/courses/blob/master/03_GettingData/dplyr/chicago.rds?raw=true"
download.file(fileURL, destfile = "chicago.rds", method = "curl", extra='-L')
chicago <- readRDS("chicago.rds")

> dim(chicago)
[1] 6940    8
> str(chicago)
'data.frame':	6940 obs. of  8 variables:
 $ city      : chr  "chic" "chic" "chic" "chic" ...
 $ tmpd      : num  31.5 33 33 29 32 40 34.5 29 26.5 32.5 ...
 $ dptp      : num  31.5 29.9 27.4 28.6 28.9 ...
 $ date      : Date, format: "1987-01-01" ...
 $ pm25tmean2: num  NA NA NA NA NA NA NA NA NA NA ...
 $ pm10tmean2: num  34 NA 34.2 47 NA ...
 $ o3tmean2  : num  4.25 3.3 3.33 4.38 4.75 ...
 $ no2tmean2 : num  20 23.2 23.8 30.4 30.3 ...
```

##### Subset by refering only by names

subsetting with excluding is easier than regular

```
> names(chicago)
[1] "city"       "tmpd"       "dptp"       "date"       "pm25tmean2"
[6] "pm10tmean2" "o3tmean2"   "no2tmean2" 
> head(select(chicago, city:dptp))
  city tmpd   dptp
1 chic 31.5 31.500
2 chic 33.0 29.875
3 chic 33.0 27.375
4 chic 29.0 28.625
5 chic 32.0 28.875
6 chic 40.0 35.125

> select all by with -()
> head(select(chicago, -(city:dptp)))
        date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 1987-01-01         NA   34.00000 4.250000  19.98810
2 1987-01-02         NA         NA 3.304348  23.19099
3 1987-01-03         NA   34.16667 3.333333  23.81548
4 1987-01-04         NA   47.00000 4.375000  30.43452
5 1987-01-05         NA         NA 4.750000  30.33333
6 1987-01-06         NA   48.00000 5.833333  25.77233
```

##### filter and refer by using names

```
> chic.f <- filter(chicago, pm25tmean2 > 30)
> head(chic.f, 5)
  city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2
1 chic   23 21.9 1998-01-17      38.10   32.46154  3.180556
2 chic   28 25.8 1998-01-23      33.95   38.69231  1.750000
3 chic   55 51.3 1998-04-30      39.40   34.00000 10.786232
4 chic   59 53.7 1998-05-01      35.40   28.50000 14.295125
5 chic   57 52.0 1998-05-02      33.30   35.00000 20.662879
  no2tmean2
1  25.30000
2  29.37630
3  25.31310
4  31.42905
5  26.79861
> chic.f <- filter(chicago, pm25tmean2 > 30 & tmpd > 80)
> head(chic.f, 5)
  city tmpd dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 chic   81 71.2 1998-08-23    39.6000       59.0 45.86364  14.32639
2 chic   81 70.4 1998-09-06    31.5000       50.5 50.66250  20.31250
3 chic   82 72.2 2001-07-20    32.3000       58.5 33.00380  33.67500
4 chic   84 72.9 2001-08-01    43.7000       81.5 45.17736  27.44239
5 chic   85 72.6 2001-08-08    38.8375       70.0 37.98047  27.62743
```

##### arrange

```
># arrange by date
> chicago <- arrange(chicago, date)
> head(chicago)
  city tmpd   dptp       date pm25tmean2 pm10tmean2 o3tmean2
1 chic 31.5 31.500 1987-01-01         NA   34.00000 4.250000
2 chic 33.0 29.875 1987-01-02         NA         NA 3.304348
3 chic 33.0 27.375 1987-01-03         NA   34.16667 3.333333
4 chic 29.0 28.625 1987-01-04         NA   47.00000 4.375000
5 chic 32.0 28.875 1987-01-05         NA         NA 4.750000
6 chic 40.0 35.125 1987-01-06         NA   48.00000 5.833333
  no2tmean2
1  19.98810
2  23.19099
3  23.81548
4  30.43452
5  30.33333
6  25.77233

> chicago <- arrange(chicago, desc(date))
> head(chicago)
  city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2
1 chic   35 30.1 2005-12-31   15.00000       23.5  2.531250
2 chic   36 31.0 2005-12-30   15.05714       19.2  3.034420
3 chic   35 29.4 2005-12-29    7.45000       23.5  6.794837
4 chic   37 34.5 2005-12-28   17.75000       27.5  3.260417
5 chic   40 33.6 2005-12-27   23.56000       27.0  4.468750
6 chic   35 29.6 2005-12-26    8.40000        8.5 14.041667
  no2tmean2
1  13.25000
2  22.80556
3  19.97222
4  19.28563
5  23.50000
6  16.81944
> tail(chicago)
     city tmpd   dptp       date pm25tmean2 pm10tmean2 o3tmean2
6935 chic 40.0 35.125 1987-01-06         NA   48.00000 5.833333
6936 chic 32.0 28.875 1987-01-05         NA         NA 4.750000
6937 chic 29.0 28.625 1987-01-04         NA   47.00000 4.375000
6938 chic 33.0 27.375 1987-01-03         NA   34.16667 3.333333
6939 chic 33.0 29.875 1987-01-02         NA         NA 3.304348
6940 chic 31.5 31.500 1987-01-01         NA   34.00000 4.250000
     no2tmean2
6935  25.77233
6936  30.33333
6937  30.43452
6938  23.81548
6939  23.19099
6940  19.98810
```

##### rename

```
># rename two columns i.e. from pm25tmean2 to pm25
> chicago <- rename(chicago, pm25 = pm25tmean2, dewpoint = dptp)
> head(chicago)
  city tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2
1 chic   35     30.1 2005-12-31 15.00000       23.5  2.531250
2 chic   36     31.0 2005-12-30 15.05714       19.2  3.034420
3 chic   35     29.4 2005-12-29  7.45000       23.5  6.794837
4 chic   37     34.5 2005-12-28 17.75000       27.5  3.260417
5 chic   40     33.6 2005-12-27 23.56000       27.0  4.468750
6 chic   35     29.6 2005-12-26  8.40000        8.5 14.041667
  no2tmean2
1  13.25000
2  22.80556
3  19.97222
4  19.28563
5  23.50000
6  16.81944
```

mutate

```
> chicago <- mutate(chicago, pm25detrend = pm25-mean(pm25, na.rm = TRUE))
> head(select(chicago, pm25, pm25detrend))
      pm25 pm25detrend
1 15.00000   -1.230958
2 15.05714   -1.173815
3  7.45000   -8.780958
4 17.75000    1.519042
5 23.56000    7.329042
6  8.40000   -7.830958
```

##### Group

```
> # create factor variable tempcat and create a new labels hot cold
> chicago <- mutate(chicago, tempcat = factor(1* (tmpd > 80), labels =c ("cold", "hot")))
> hotcold <- group_by(chicago, tempcat)
> hotcold
# A tibble: 6,940 x 10
# Groups:   tempcat [3]
    city  tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2
   <chr> <dbl>    <dbl>     <date>    <dbl>      <dbl>     <dbl>
 1  chic    35     30.1 2005-12-31 15.00000       23.5  2.531250
 2  chic    36     31.0 2005-12-30 15.05714       19.2  3.034420
 3  chic    35     29.4 2005-12-29  7.45000       23.5  6.794837
 4  chic    37     34.5 2005-12-28 17.75000       27.5  3.260417
 5  chic    40     33.6 2005-12-27 23.56000       27.0  4.468750
 6  chic    35     29.6 2005-12-26  8.40000        8.5 14.041667
 7  chic    35     32.1 2005-12-25  6.70000        8.0 14.354167
 8  chic    37     35.2 2005-12-24 30.77143       25.2  1.770833
 9  chic    41     32.6 2005-12-23 32.90000       34.5  6.906250
10  chic    22     23.3 2005-12-22 36.65000       42.5  5.385417
# ... with 6,930 more rows, and 3 more variables: no2tmean2 <dbl>,
#   pm25detrend <dbl>, tempcat <fctr>

># now summarize
> summarize(hotcold, pm25 =mean(pm25, na.rm = TRUE), o3 = max(o3tmean2), no2 = median(no2tmean2))
# A tibble: 3 x 4
  tempcat     pm25        o3      no2
   <fctr>    <dbl>     <dbl>    <dbl>
1    cold 15.97807 66.587500 24.54924
2     hot 26.48118 62.969656 24.93870
3    <NA> 47.73750  9.416667 37.44444

```

##### summarize

```
> # summary for each year for each variables
> summarize(years, pm25 = mean(pm25, na.rm = TRUE), o3 = max(o3tmean2), no2 = median(no2tmean2))
# A tibble: 19 x 4
    year     pm25       o3      no2
   <dbl>    <dbl>    <dbl>    <dbl>
 1  1987      NaN 62.96966 23.49369
 2  1988      NaN 61.67708 24.52296
 3  1989      NaN 59.72727 26.14062
 4  1990      NaN 52.22917 22.59583
 5  1991      NaN 63.10417 21.38194
 6  1992      NaN 50.82870 24.78921
 7  1993      NaN 44.30093 25.76993
 8  1994      NaN 52.17844 28.47500
 9  1995      NaN 66.58750 27.26042
10  1996      NaN 58.39583 26.38715
11  1997      NaN 56.54167 25.48143
12  1998 18.26467 50.66250 24.58649
13  1999 18.49646 57.48864 24.66667
14  2000 16.93806 55.76103 23.46082
15  2001 16.92632 51.81984 25.06522
16  2002 15.27335 54.88043 22.73750
17  2003 15.23183 56.16608 24.62500
18  2004 14.62864 44.48240 23.39130
19  2005 16.18556 58.84126 22.62387
```



