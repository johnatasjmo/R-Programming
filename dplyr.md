# dplyr package

tibble is special type of dataframe

package hflights

glimpse\(hflights\) uses more information to display

```r
two <- c("AA", "AS")
lut <- c("AA" = "American",
         "AS" = "Alaska",
         "B6" = "JetBlue")
two <- lut[two]
two
```

`select` removes columns and returns a modified copy

`filter` removes rows

`arranges` reorders

`mutate` changes values

`summarise` provides statistics

Observations in columns, variables in columns.

Resources tidyr data package

Select

![](/assets/Screen Shot 2017-11-08 at 8.22.38 AM.png)

```r
library(dplyr)
library(hflights)

# Convert the hflights data.frame into a hflights tbl
hflights <- tbl_df(hflights)
# Display the hflights tbl
hflights
# Create the object carriers
carriers <- hflights$UniqueCarrier

# Both the dplyr and hflights packages are loaded into workspace
lut <- c("AA" = "American", "AS" = "Alaska", "B6" = "JetBlue", "CO" = "Continental",
         "DL" = "Delta", "OO" = "SkyWest", "UA" = "United", "US" = "US_Airways",
         "WN" = "Southwest", "EV" = "Atlantic_Southeast", "F9" = "Frontier",
         "FL" = "AirTran", "MQ" = "American_Eagle", "XE" = "ExpressJet", "YV" = "Mesa")
lut
# Add the Carrier column to hflights
hflights$Carrier <- lut[hflights$UniqueCarrier]
# Glimpse at hflights
glimpse(hflights)

# shows unique cancelations codes
unique(hflights$CancellationCode)
# The lookup table created based on cancellation codes
lut <- c("A" = "carrier", "B" = "weather", "C" = "FFA", "D" = "security", "E" = "not cancelled")
# Add the Code column
hflights$Code <- lut[hflights$CancellationCode]
# Glimpse at hflights
glimpse(hflights)
```

Filter

filter\(hflights, Cancelled == 1\)

```
x < y, TRUE if x is less than y
x <= y, TRUE if x is less than or equal to y
x == y, TRUE if x equals y
x != y, TRUE if x does not equal y
x >= y, TRUE if x is greater than or equal to y
x > y, TRUE if x is greater than y
x %in% c(a, b, c), TRUE if x is in the vector c(a, b, c)
```

R also comes with a set of boolean operators that you can use to combine multiple logical tests into a single test. These include `&`\(and\), `|`\(or\), and `!`\(not\). Instead of using the `&`operator, you can also pass several logical tests to [`filter()`](http://www.rdocumentation.org/packages/dplyr/functions/filter) , separated by commas. The following two calls are completely equivalent:

```
filter(df, a > 0 & b > 0)
filter(df, a > 0, b > 0)

# which of the variables is not NA
filter(df, !is.na(x))
```

arrange

### summarise

```
min(x) - minimum value of vector x.
max(x) - maximum value of vector x.
mean(x) - mean value of vector x.
median(x) - median value of vector x.
quantile(x, p) - pth quantile of vector x.
sd(x) - standard deviation of vector x.
var(x) - variance of vector x.
IQR(x) - Inter Quartile Range (IQR) of vector x.
diff(range(x)) - total range of vector x.

first(x) - The first element of vector x.
last(x) - The last element of vector x.
nth(x, n) - The nth element of vector x.
n() - The number of rows in the data.frame or group of observations that summarise() describes.
n_distinct(x) - The number of unique values in vector x.
```

##### %&gt;% Operator

```
mean(c(1, 2, 3, NA), na.rm = TRUE)
c(1, 2, 3, NA) %>% mean(na.rm = TRUE)
```

```
# Write the 'piped' version of the English sentences.
hflights %>%
        mutate(diff = TaxiOut - TaxiIn) %>%
        filter(!is.na(diff)) %>%
        summarise(avg = mean(diff))
```

```
> # Chain together mutate(), filter() and summarise()
> hflights %>%
  mutate(RealTime = ActualElapsedTime + 100,
              mph = Distance / RealTime * 60) %>%
        filter(!is.na(mph), mph < 70) %>%
        summarise(n_less = n(),
                  n_dest = n_distinct(Dest),
                  min_dist = min(Distance),
                  max_dist = max(Distance))
# A tibble: 1 × 4
  n_less n_dest min_dist max_dist
   <int>  <int>    <int>    <int>
1    673     12       79      253
```

```
> # Finish the command with a filter() and summarise() call
> hflights %>%
    mutate(RealTime = ActualElapsedTime + 100, mph = Distance / RealTime * 60) %>%
    filter(mph < 105 | Cancelled == 1 | Diverted == 1) %>%
    summarise(n_non = n(),
              n_dest = n_distinct(Dest),
              min_dist = min(Distance),
              max_dist = max(Distance))
# A tibble: 1 × 4
  n_non n_dest min_dist max_dist
  <int>  <int>    <int>    <int>
1  4294     84       79     2007
```

```
> # Count the number of overnight flights
> hflights %>%
  filter(!is.na(DepTime), !is.na(ArrTime), DepTime > ArrTime) %>%
  summarise( num = n())
# A tibble: 1 × 1
    num
  <int>
1   265
```

### Group by

### ![](/assets/Screen Shot 2017-11-14 at 8.03.37 AM.png)'

Combination of unique records

```
# create a combination of unique carriers and detinations
hflights %>%
group_by(UniqueCarrier, Dest)
```

```r
> # Make an ordered per-carrier summary of hflights
> # get the mean of cancelled flights
> # summarise without delays not equal to NA
> # order from low to high by average arrival delay as percentage
> hflights %>%
     group_by(UniqueCarrier) %>%
     summarise(p_canc = mean(Cancelled == 1) * 100,
               avg_delay = mean(ArrDelay, na.rm = TRUE)) %>%
     arrange(avg_delay, p_canc)
# A tibble: 15 × 3
        UniqueCarrier     p_canc  avg_delay
                <chr>      <dbl>      <dbl>
1          US_Airways  1.1848341 -1.0721154
2                Mesa 14.2857143 -0.8333333
3            American  1.5873016 -0.1161290
4             AirTran  0.8888889  0.7117117
5              Alaska  0.0000000  1.0625000
6            Frontier  0.0000000  4.0000000
7             JetBlue  4.1666667  4.0000000
8         Continental  0.7165377  6.0759329
9  Atlantic_Southeast  3.0567686  6.2927928
10             United  2.5773196  6.5925926
11              Delta  2.0746888  7.1440678
12          Southwest  1.6185477  7.5294903
13         ExpressJet  1.5290102  7.6131367
14     American_Eagle  1.8140590  7.8101852
15            SkyWest  1.6059296  9.1620429
```

```r
 # Ordererd overview of average arrival delays per carrie
> hflights %>%
  #filter hflights without NA and positive
       filter(!is.na(ArrDelay), ArrDelay > 0) %>%
        #group by carrier
        group_by(UniqueCarrier) %>%
        #summarise the mean per carrier
        summarise(avg = mean(ArrDelay)) %>%
        #add new variable rank from average
        mutate( rank = rank(avg)) %>%
        #arrange by rank
        arrange(rank)
# A tibble: 15 × 3
UniqueCarrier avg rank
<chr> <dbl> <dbl>
        1 Frontier 13.42500 1
2 Mesa 14.00000 2
3 US_Airways 20.19084 3
4 Continental 21.83245 4
5 American 23.07921 5
6 ExpressJet 23.45569 6
7 SkyWest 24.31395 7
8 Southwest 25.18831 8
9 AirTran 26.28788 9
10 United 28.03488 10
11 Alaska 28.30000 11
12 Delta 33.97872 12
13 JetBlue 36.63636 13
14 American_Eagle 39.07407 14
15 Atlantic_Southeast 39.23171 15
```

```r
> # dplyr and hflights (with translated carrier names) are pre-loaded
>
> # How many airplanes only flew to one destination?
> hflights %>%
  # group by airplane number
    group_by(TailNum) %>%
  # summarize by unique destination
    summarise(ndest = n_distinct(Dest)) %>%
  #filter airplanes that goes to only 1 destination
    filter(ndest == 1) %>%
  # sumarize by new column nplanes
    summarise(nplanes = n())
# A tibble: 1 × 1
  nplanes
    <int>
1    1039
>
> # Find the most visited destination for each carrier
> hflights %>%
  # create a combination carriers and destination
    group_by(UniqueCarrier, Dest) %>%
  #summarise by new variable n, UniqueCarrier-Dest combination
    summarise(n = n()) %>%
  #add column rank and descend by n
    mutate(rank = rank(desc(n))) %>%
  # select only the number one, most visited destination for each carrier
    filter(rank == 1)
Source: local data frame [15 x 4]
Groups: UniqueCarrier [15]

        UniqueCarrier  Dest     n  rank
                <chr> <chr> <int> <dbl>
1             AirTran   ATL   211     1
2              Alaska   SEA    32     1
3            American   DFW   186     1
4      American_Eagle   DFW   234     1
5  Atlantic_Southeast   DTW    89     1
6         Continental   LAX   417     1
7               Delta   ATL   219     1
8          ExpressJet   CRP   313     1
9            Frontier   DEN    78     1
10            JetBlue   JFK    72     1
11               Mesa   CLT     6     1
12            SkyWest   COS   143     1
13          Southwest   DAL   839     1
14         US_Airways   CLT   237     1
15             United   ORD    59     1
```

Connect to mysql db

```rr
> # Set up a connection to the mysql database
> my_db <- src_mysql(dbname = "dplyr",
                     host = "courses.csrrinzqubik.us-east-1.rds.amazonaws.com",
                     port = 3306,
                     user = "student",
                     password = "datacamp")
>
> # Reference a table within that source: nycflights
> nycflights <- tbl(my_db, "dplyr")
>
> # glimpse at nycflights
> glimpse(nycflights)
Observations: NA
Variables: 17
$ id        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17...
$ year      <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 201...
$ month     <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ...
$ day       <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ...
$ dep_time  <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 558, 558, 55...
$ dep_delay <int> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2, -2, -1, ...
$ arr_time  <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 753, 849, 8...
$ arr_delay <int> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -3, 7, -14,...
$ carrier   <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV", "B6", "AA...
$ tailnum   <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN", "N39463...
$ flight    <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79, 301, 49,...
$ origin    <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR", "LGA", "...
$ dest      <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL", "IAD", "...
$ air_time  <int> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138, 149, 158...
$ distance  <int> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 944, 733, 10...
$ hour      <int> 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 6, 6, ...
$ minute    <int> 17, 33, 42, 44, 54, 54, 55, 57, 57, 58, 58, 58, 58, 58, 5...
>
> # Ordered, grouped summary of nycflights
>   nycflights %>%
    group_by(carrier) %>%
    summarise(n_flights = n(), avg_delay = mean(arr_delay)) %>%
   arrange(avg_delay)
Source:   query [?? x 3]
Database: mysql 5.6.34-log [student@courses.csrrinzqubik.us-east-1.rds.amazonaws.com:/dplyr]
Warning message: Decimal MySQL column 2 imported as numeric
   carrier n_flights avg_delay
     <chr>     <dbl>     <dbl>
1       AS       714   -9.8613
2       HA       342   -6.9152
3       AA     32729    0.3556
4       DL     48110    1.6289
5       VX      5162    1.7487
6       US     20536    2.0565
7       UA     58665    3.5045
8       9E     18460    6.9135
9       B6     54635    9.3565
10      WN     12275    9.4675
# ... with more rows
```

## Exploratory Data Analysis: Case Study

filter in

```
# Vector of four countries to examine
countries <- c("United States", "United Kingdom",
               "France", "India")

# Filter by_year_country: filtered_4_countries
filtered_4_countries <- by_year_country %>%
  filter(country %in% countries)

# Line plot of % yes in four countries
ggplot(filtered_4_countries, aes(year, percent_yes, color = country)) +
  geom_line()
```

Facet ggplot

```
# Vector of six countries to examine
countries <- c("United States", "United Kingdom",
               "France", "Japan", "Brazil", "India")

# Filtered by_year_country: filtered_6_countries
filtered_6_countries <- by_year_country %>%
filter(country %in% countries)

# Line plot of % yes over time faceted by country
ggplot(filtered_6_countries, aes(x = year, y = percent_yes)) +
  geom_line() +
  facet_wrap(~ country)
```

### Linear regression

lm gets the linear model

summary\(\) shows intercept and splope

```
> # Percentage of yes votes from the US by year: US_by_year
> US_by_year <- by_year_country %>%
    filter(country == "United States")
>
> # Print the US_by_year data
> US_by_year
# A tibble: 34 x 4
    year       country total percent_yes
   <dbl>         <chr> <int>       <dbl>
 1  1947 United States    38   0.7105263
 2  1949 United States    64   0.2812500
 3  1951 United States    25   0.4000000
 4  1953 United States    26   0.5000000
 5  1955 United States    37   0.6216216
 6  1957 United States    34   0.6470588
 7  1959 United States    54   0.4259259
 8  1961 United States    75   0.5066667
 9  1963 United States    32   0.5000000
10  1965 United States    41   0.3658537
# ... with 24 more rows
>
> # Perform a linear regression of percent_yes by year: US_fit
> US_fit <- lm(percent_yes ~ year, data = US_by_year)
>
> # Perform summary() on the US_fit object
> summary(US_fit)

Call:
lm(formula = percent_yes ~ year, data = US_by_year)

Residuals:
      Min        1Q    Median        3Q       Max
-0.222491 -0.080635 -0.008661  0.081948  0.194307

Coefficients:
              Estimate Std. Error t value Pr(>|t|)
(Intercept) 12.6641455  1.8379743   6.890 8.48e-08 ***
year        -0.0062393  0.0009282  -6.722 1.37e-07 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.1062 on 32 degrees of freedom
Multiple R-squared:  0.5854,    Adjusted R-squared:  0.5724
F-statistic: 45.18 on 1 and 32 DF,  p-value: 1.367e-07
```

### broom

```
library(broom)
```

```
# Load the broom package
library(broom)

# Call the tidy() function on the US_fit object
tidy(US_fit)
```

```{r}
> ## Linear regression of percent_yes by year for US
> US_by_year <- by_year_country %>%
    filter(country == "United States")
> US_fit <- lm(percent_yes ~ year, US_by_year)
>
> # Fit model for the United Kingdom
> UK_by_year <- by_year_country %>%
    filter(country == "United Kingdom")
> UK_fit <- lm(percent_yes ~ year, UK_by_year)
>
> # Create US_tidied and UK_tidied
> US_tidied <- tidy(US_fit)
> UK_tidied <- tidy(UK_fit)
>
> # Combine the two tidied models
> bind_rows(tidy(US_fit), tidy(UK_fit))
         term     estimate    std.error statistic      p.value
1 (Intercept) 12.664145512 1.8379742715  6.890274 8.477089e-08
2        year -0.006239305 0.0009282243 -6.721764 1.366904e-07
3 (Intercept) -3.266547873 1.9577739504 -1.668501 1.049736e-01
4        year  0.001869434 0.0009887262  1.890750 6.774177e-0
```

##### nest

nest\(-country\) will nest all information in separate pieces

```
> # Load the tidyr package
> library(tidyr)
>
> # Nest all columns besides country
> by_year_country %>%
  nest(-country)
# A tibble: 200 x 2
                           country              data
                             <chr>            <list>
 1                     Afghanistan <tibble [34 x 3]>
 2                       Argentina <tibble [34 x 3]>
 3                       Australia <tibble [34 x 3]>
 4                         Belarus <tibble [34 x 3]>
 5                         Belgium <tibble [34 x 3]>
 6 Bolivia, Plurinational State of <tibble [34 x 3]>
 7                          Brazil <tibble [34 x 3]>
 8                          Canada <tibble [34 x 3]>
 9                           Chile <tibble [34 x 3]>
10                        Colombia <tibble [34 x 3]>
# ... with 190 more rows
```

```
> head(by_year_country)
# A tibble: 6 x 4
   year                         country total percent_yes
  <dbl>                           <chr> <int>       <dbl>
1  1947                     Afghanistan    34   0.3823529
2  1947                       Argentina    38   0.5789474
3  1947                       Australia    38   0.5526316
4  1947                         Belarus    38   0.5000000
5  1947                         Belgium    38   0.6052632
6  1947 Bolivia, Plurinational State of    37   0.5945946
> # All countries are nested besides country
> nested <- by_year_country %>%
    nest(-country)
>
> # Print the nested data for Brazil
> nested$data[7]
[[1]]
# A tibble: 34 x 3
    year total percent_yes
   <dbl> <int>       <dbl>
 1  1947    38   0.6578947
 2  1949    64   0.4687500
 3  1951    25   0.6400000
 4  1953    26   0.7307692
 5  1955    37   0.7297297
 6  1957    34   0.7352941
 7  1959    54   0.5370370
 8  1961    76   0.5526316
 9  1963    32   0.7812500
10  1965    41   0.6097561
# ... with 24 more rows
```

Nest and unest

```
# All countries are nested besides country
nested <- by_year_country %>%
  nest(-country)

# Unnest the data column to return it to its original form
nested %>%
  unnest(data)
```

##### map

`map`uses for apply an operation in the list

broom package takes each model and takes into data fame

`map(v, ~ . * 10)`  then `~`starts `.` is each item in the list , then multiply each item by 10

> v &lt;- list\(1, 2, 3\)
> map\(v, ~ . \* \`0\)
>     \[\[1\]\]
>     \[1\] 10

```
[[2]]
[1] 20

[[3]]
[1] 30
```

steps

![](/assets/nest4steps.png)

```
> ## Load tidyr and purrr
> library(tidyr)
> library(purrr)
>
> #check data
> head(by_year_country)
# A tibble: 6 x 4
   year                         country total percent_yes
  <dbl>                           <chr> <int>       <dbl>
1  1947                     Afghanistan    34   0.3823529
2  1947                       Argentina    38   0.5789474
3  1947                       Australia    38   0.5526316
4  1947                         Belarus    38   0.5000000
5  1947                         Belgium    38   0.6052632
6  1947 Bolivia, Plurinational State of    37   0.5945946
>
> # Perform a linear regression on each item in the data column
> by_year_country %>%
    nest(-country) %>%
    mutate(model = map(data, ~ lm(percent_yes ~ year, .)))
# A tibble: 200 x 3
                           country              data    model
                             <chr>            <list>   <list>
 1                     Afghanistan <tibble [34 x 3]> <S3: lm>
 2                       Argentina <tibble [34 x 3]> <S3: lm>
 3                       Australia <tibble [34 x 3]> <S3: lm>
 4                         Belarus <tibble [34 x 3]> <S3: lm>
 5                         Belgium <tibble [34 x 3]> <S3: lm>
 6 Bolivia, Plurinational State of <tibble [34 x 3]> <S3: lm>
 7                          Brazil <tibble [34 x 3]> <S3: lm>
 8                          Canada <tibble [34 x 3]> <S3: lm>
 9                           Chile <tibble [34 x 3]> <S3: lm>
10                        Colombia <tibble [34 x 3]> <S3: lm>
# ... with 190 more rows
```

Tidy each linear regression model

```
> # Load the broom package
> library(broom)
>
> # Add another mutate that applies tidy() to each model
> by_year_country %>%
    nest(-country) %>%
    mutate(model = map(data, ~ lm(percent_yes ~ year, data = .))) %>%
    mutate(tidied = map(model, tidy))
# A tibble: 200 x 4
                           country              data    model
                             <chr>            <list>   <list>
 1                     Afghanistan <tibble [34 x 3]> <S3: lm>
 2                       Argentina <tibble [34 x 3]> <S3: lm>
 3                       Australia <tibble [34 x 3]> <S3: lm>
 4                         Belarus <tibble [34 x 3]> <S3: lm>
 5                         Belgium <tibble [34 x 3]> <S3: lm>
 6 Bolivia, Plurinational State of <tibble [34 x 3]> <S3: lm>
 7                          Brazil <tibble [34 x 3]> <S3: lm>
 8                          Canada <tibble [34 x 3]> <S3: lm>
 9                           Chile <tibble [34 x 3]> <S3: lm>
10                        Colombia <tibble [34 x 3]> <S3: lm>
# ... with 190 more rows, and 1 more variables: tidied <list>
```

**Unnesting the dataframe**

```
> # Add one more step that unnests the tidied column
> country_coefficients <-by_year_country  %>%
    nest(-country) %>%
    mutate(model = map(data, ~ lm(percent_yes ~ year, data = .)),
           tidied = map(model, tidy)) %>%
    unnest(tidied)
>
> # Print the resulting country_coefficients variable
> country_coefficients
# A tibble: 399 x 6
       country        term      estimate    std.error statistic      p.value
         <chr>       <chr>         <dbl>        <dbl>     <dbl>        <dbl>
 1 Afghanistan (Intercept) -11.063084650 1.4705189228 -7.523252 1.444892e-08
 2 Afghanistan        year   0.006009299 0.0007426499  8.091698 3.064797e-09
 3   Argentina (Intercept)  -9.464512565 2.1008982371 -4.504984 8.322481e-05
 4   Argentina        year   0.005148829 0.0010610076  4.852773 3.047078e-05
 5   Australia (Intercept)  -4.545492536 2.1479916283 -2.116159 4.220387e-02
 6   Australia        year   0.002567161 0.0010847910  2.366503 2.417617e-02
 7     Belarus (Intercept)  -7.000692717 1.5024232546 -4.659601 5.329950e-05
 8     Belarus        year   0.003907557 0.0007587624  5.149908 1.284924e-05
 9     Belgium (Intercept)  -5.845534016 1.5153390521 -3.857575 5.216573e-04
10     Belgium        year   0.003203234 0.0007652852  4.185673 2.072981e-04
# ... with 389 more rows
```

Filter to get only `year` coefficients, as `(Intercept)` wont be used

```
> # Print the country_coefficients dataset
> country_coefficients
# A tibble: 399 x 6
       country        term      estimate    std.error statistic      p.value
         <chr>       <chr>         <dbl>        <dbl>     <dbl>        <dbl>
 1 Afghanistan (Intercept) -11.063084650 1.4705189228 -7.523252 1.444892e-08
 2 Afghanistan        year   0.006009299 0.0007426499  8.091698 3.064797e-09
 3   Argentina (Intercept)  -9.464512565 2.1008982371 -4.504984 8.322481e-05
 4   Argentina        year   0.005148829 0.0010610076  4.852773 3.047078e-05
 5   Australia (Intercept)  -4.545492536 2.1479916283 -2.116159 4.220387e-02
 6   Australia        year   0.002567161 0.0010847910  2.366503 2.417617e-02
 7     Belarus (Intercept)  -7.000692717 1.5024232546 -4.659601 5.329950e-05
 8     Belarus        year   0.003907557 0.0007587624  5.149908 1.284924e-05
 9     Belgium (Intercept)  -5.845534016 1.5153390521 -3.857575 5.216573e-04
10     Belgium        year   0.003203234 0.0007652852  4.185673 2.072981e-04
# ... with 389 more rows
>
> # Filter for only the slope terms
> filter(country_coefficients, term == 'year')
# A tibble: 199 x 6
                           country  term    estimate    std.error statistic
                             <chr> <chr>       <dbl>        <dbl>     <dbl>
 1                     Afghanistan  year 0.006009299 0.0007426499  8.091698
 2                       Argentina  year 0.005148829 0.0010610076  4.852773
 3                       Australia  year 0.002567161 0.0010847910  2.366503
 4                         Belarus  year 0.003907557 0.0007587624  5.149908
 5                         Belgium  year 0.003203234 0.0007652852  4.185673
 6 Bolivia, Plurinational State of  year 0.005802864 0.0009657515  6.008651
 7                          Brazil  year 0.006107151 0.0008167736  7.477164
 8                          Canada  year 0.001515867 0.0009552118  1.586943
 9                           Chile  year 0.006775560 0.0008220463  8.242310
10                        Colombia  year 0.006157755 0.0009645084  6.384346
# ... with 189 more rows, and 1 more variables: p.value <dbl>
```

Filter out significant countries with less than p value of 0.05

```
> ## Filter for only the slope terms
> slope_terms <- country_coefficients %>%
    filter(term == "year")
>
> # Add p.adjusted column, then filter
> slope_terms %>%
    mutate(p.adjusted = p.adjust(p.value)) %>%
    filter(p.adjusted < .05)
# A tibble: 61 x 7
                           country  term    estimate    std.error statistic
                             <chr> <chr>       <dbl>        <dbl>     <dbl>
 1                     Afghanistan  year 0.006009299 0.0007426499  8.091698
 2                       Argentina  year 0.005148829 0.0010610076  4.852773
 3                         Belarus  year 0.003907557 0.0007587624  5.149908
 4                         Belgium  year 0.003203234 0.0007652852  4.185673
 5 Bolivia, Plurinational State of  year 0.005802864 0.0009657515  6.008651
 6                          Brazil  year 0.006107151 0.0008167736  7.477164
 7                           Chile  year 0.006775560 0.0008220463  8.242310
 8                        Colombia  year 0.006157755 0.0009645084  6.384346
 9                      Costa Rica  year 0.006539273 0.0008119113  8.054171
10                            Cuba  year 0.004610867 0.0007205029  6.399512
# ... with 51 more rows, and 2 more variables: p.value <dbl>, p.adjusted <dbl>
>
```

arrange data asc y desc

```
> ## Filter by adjusted p-values
> filtered_countries <- country_coefficients %>%
    filter(term == "year") %>%
    mutate(p.adjusted = p.adjust(p.value)) %>%
    filter(p.adjusted < .05)
>
> # Sort for the countries increasing most quickly
> filtered_countries %>%
    arrange(desc(estimate))
# A tibble: 61 x 7
               country  term    estimate    std.error statistic      p.value
                 <chr> <chr>       <dbl>        <dbl>     <dbl>        <dbl>
 1        South Africa  year 0.011858333 0.0014003768  8.467959 1.595372e-08
 2          Kazakhstan  year 0.010955741 0.0019482401  5.623404 3.244186e-04
 3 Yemen Arab Republic  year 0.010854882 0.0015869058  6.840281 1.197855e-06
 4          Kyrgyzstan  year 0.009725462 0.0009884060  9.839541 2.379933e-05
 5              Malawi  year 0.009084873 0.0018111087  5.016194 4.480823e-05
 6  Dominican Republic  year 0.008055482 0.0009138578  8.814809 5.957258e-10
 7            Portugal  year 0.008020046 0.0017124482  4.683380 7.132640e-05
 8            Honduras  year 0.007717977 0.0009214260  8.376123 1.431724e-09
 9                Peru  year 0.007299813 0.0009764019  7.476238 1.645381e-08
10           Nicaragua  year 0.007075848 0.0010716402  6.602820 1.918043e-07
# ... with 51 more rows, and 1 more variables: p.adjusted <dbl>
>
> # Sort for the countries decreasing most quickly
> filtered_countries %>%
    arrange(estimate)
# A tibble: 61 x 7
                     country  term     estimate    std.error statistic
                       <chr> <chr>        <dbl>        <dbl>     <dbl>
 1        Korea, Republic of  year -0.009209912 0.0015453128 -5.959901
 2                    Israel  year -0.006852921 0.0011718657 -5.847873
 3             United States  year -0.006239305 0.0009282243 -6.721764
 4                   Belgium  year  0.003203234 0.0007652852  4.185673
 5                    Guinea  year  0.003621508 0.0008326598  4.349325
 6                   Morocco  year  0.003798641 0.0008603064  4.415451
 7                   Belarus  year  0.003907557 0.0007587624  5.149908
 8 Iran, Islamic Republic of  year  0.003911100 0.0008558952  4.569602
 9                     Congo  year  0.003967778 0.0009220262  4.303324
10                     Sudan  year  0.003989394 0.0009613894  4.149613
# ... with 51 more rows, and 2 more variables: p.value <dbl>, p.adjusted <dbl>
>
```

### Joining datasets

Join two datasets with description, so all

```{r}
> # Print the votes_processed dataset
> votes_processed
# A tibble: 353,547 x 6
    rcid session  vote ccode  year            country
   <dbl>   <dbl> <dbl> <int> <dbl>              <chr>
 1    46       2     1     2  1947      United States
 2    46       2     1    20  1947             Canada
 3    46       2     1    40  1947               Cuba
 4    46       2     1    41  1947              Haiti
 5    46       2     1    42  1947 Dominican Republic
 6    46       2     1    70  1947             Mexico
 7    46       2     1    90  1947          Guatemala
 8    46       2     1    91  1947           Honduras
 9    46       2     1    92  1947        El Salvador
10    46       2     1    93  1947          Nicaragua
# ... with 353,537 more rows
>
> # Print the descriptions dataset
> descriptions
# A tibble: 2,589 x 10
    rcid session       date   unres    me    nu    di    hr    co    ec
   <dbl>   <dbl>     <dttm>   <chr> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
 1    46       2 1947-09-04 R/2/299     0     0     0     0     0     0
 2    47       2 1947-10-05 R/2/355     0     0     0     1     0     0
 3    48       2 1947-10-06 R/2/461     0     0     0     0     0     0
 4    49       2 1947-10-06 R/2/463     0     0     0     0     0     0
 5    50       2 1947-10-06 R/2/465     0     0     0     0     0     0
 6    51       2 1947-10-02 R/2/561     0     0     0     0     1     0
 7    52       2 1947-11-06 R/2/650     0     0     0     0     1     0
 8    53       2 1947-11-06 R/2/651     0     0     0     0     1     0
 9    54       2 1947-11-06 R/2/651     0     0     0     0     1     0
10    55       2 1947-11-06 R/2/667     0     0     0     0     1     0
# ... with 2,579 more rows
>
> # Join them together based on the "rcid" and "session" columns
> votes_joined <-votes_processed %>%
   inner_join(descriptions, by = c("rcid", "session"))
> votes_joined
# A tibble: 353,547 x 14
    rcid session  vote ccode  year            country       date   unres    me
   <dbl>   <dbl> <dbl> <int> <dbl>              <chr>     <dttm>   <chr> <dbl>
 1    46       2     1     2  1947      United States 1947-09-04 R/2/299     0
 2    46       2     1    20  1947             Canada 1947-09-04 R/2/299     0
 3    46       2     1    40  1947               Cuba 1947-09-04 R/2/299     0
 4    46       2     1    41  1947              Haiti 1947-09-04 R/2/299     0
 5    46       2     1    42  1947 Dominican Republic 1947-09-04 R/2/299     0
 6    46       2     1    70  1947             Mexico 1947-09-04 R/2/299     0
 7    46       2     1    90  1947          Guatemala 1947-09-04 R/2/299     0
 8    46       2     1    91  1947           Honduras 1947-09-04 R/2/299     0
 9    46       2     1    92  1947        El Salvador 1947-09-04 R/2/299     0
10    46       2     1    93  1947          Nicaragua 1947-09-04 R/2/299     0
# ... with 353,537 more rows, and 5 more variables: nu <dbl>, di <dbl>,
#   hr <dbl>, co <dbl>, ec <dbl>
```

fiter for votes with colonialism  = 1

```{r}
# Filter for votes related to colonialism
filter(votes_joined, co == 1)
```

```{r}
> ## Load the ggplot2 package
> library(ggplot2)
>
> # Filter, then summarize percentage of voles that are yes by year: US_co_by_year
> US_co_by_year <- votes_joined %>%
    filter(country == "United States", co == 1) %>%
    group_by(year) %>%
    summarize(percent_yes = mean(vote == 1))
>
> # Graph the % of "yes" votes over time
> ggplot(US_co_by_year, aes(year, percent_yes)) +
    geom_line()
```

### Tidy data

`gather()` reshapes number of columnand collapses into two, key and value

![](/assets/gather_tidy.png)

### gather and filter

```{r}
> # Load the tidyr package
> library(tidyr)
>
> #votes_joined head
> head(votes_joined)
# A tibble: 6 x 14
   rcid session  vote ccode  year            country       date   unres    me
  <dbl>   <dbl> <dbl> <int> <dbl>              <chr>     <dttm>   <chr> <dbl>
1    46       2     1     2  1947      United States 1947-09-04 R/2/299     0
2    46       2     1    20  1947             Canada 1947-09-04 R/2/299     0
3    46       2     1    40  1947               Cuba 1947-09-04 R/2/299     0
4    46       2     1    41  1947              Haiti 1947-09-04 R/2/299     0
5    46       2     1    42  1947 Dominican Republic 1947-09-04 R/2/299     0
6    46       2     1    70  1947             Mexico 1947-09-04 R/2/299     0
# ... with 5 more variables: nu <dbl>, di <dbl>, hr <dbl>, co <dbl>, ec <dbl>
>
> # Gather the six me/nu/di/hr/co/ec columns
> votes_joined %>%
  gather(topic, has_topic, me:ec)
# A tibble: 2,121,282 x 10
    rcid session  vote ccode  year            country       date   unres topic
   <dbl>   <dbl> <dbl> <int> <dbl>              <chr>     <dttm>   <chr> <chr>
 1    46       2     1     2  1947      United States 1947-09-04 R/2/299    me
 2    46       2     1    20  1947             Canada 1947-09-04 R/2/299    me
 3    46       2     1    40  1947               Cuba 1947-09-04 R/2/299    me
 4    46       2     1    41  1947              Haiti 1947-09-04 R/2/299    me
 5    46       2     1    42  1947 Dominican Republic 1947-09-04 R/2/299    me
 6    46       2     1    70  1947             Mexico 1947-09-04 R/2/299    me
 7    46       2     1    90  1947          Guatemala 1947-09-04 R/2/299    me
 8    46       2     1    91  1947           Honduras 1947-09-04 R/2/299    me
 9    46       2     1    92  1947        El Salvador 1947-09-04 R/2/299    me
10    46       2     1    93  1947          Nicaragua 1947-09-04 R/2/299    me
# ... with 2,121,272 more rows, and 1 more variables: has_topic <dbl>
>
> # Perform gather again, then filter
> votes_gathered <- votes_joined %>%
  gather(topic, has_topic, me:ec) %>%
  filter(has_topic > 0)
```

recode\(\) to show description

```
> ## Replace the two-letter codes in topic: votes_tidied
> votes_tidied <- votes_gathered %>%
    mutate(topic = recode(topic,
                          me = "Palestinian conflict",
                          nu = "Nuclear weapons and nuclear material",
                          di = "Arms control and disarmament",
                          hr = "Human rights",
                          co = "Colonialism",
                          ec = "Economic development"))
>
> #str(votes_tidied)
> str(votes_tidied)
Classes 'tbl_df', 'tbl' and 'data.frame':    350032 obs. of  10 variables:
 $ rcid     : atomic  77 77 77 77 77 77 77 77 77 77 ...
  ..- attr(*, "comment")= chr ""
 $ session  : atomic  2 2 2 2 2 2 2 2 2 2 ...
  ..- attr(*, "comment")= chr ""
 $ vote     : atomic  1 1 3 1 1 2 1 2 2 1 ...
  ..- attr(*, "comment")= chr ""
 $ ccode    : atomic  2 20 40 41 42 70 90 91 92 93 ...
  ..- attr(*, "comment")= chr ""
 $ year     : atomic  1947 1947 1947 1947 1947 ...
  ..- attr(*, "comment")= chr ""
 $ country  : chr  "United States" "Canada" "Cuba" "Haiti" ...
 $ date     : POSIXct, format: "1947-11-06" "1947-11-06" ...
 $ unres    : chr  "R/2/1424" "R/2/1424" "R/2/1424" "R/2/1424" ...
 $ topic    : chr  "Palestinian conflict" "Palestinian conflict" "Palestinian conflict" "Palestinian conflict" ...
 $ has_topic: num  1 1 1 1 1 1 1 1 1 1 ...
```

Summarize by country, year and topic

```
> # Print votes_tidied
> votes_tidied
# A tibble: 350,032 x 10
    rcid session  vote ccode  year            country       date    unres
   <dbl>   <dbl> <dbl> <int> <dbl>              <chr>     <dttm>    <chr>
 1    77       2     1     2  1947      United States 1947-11-06 R/2/1424
 2    77       2     1    20  1947             Canada 1947-11-06 R/2/1424
 3    77       2     3    40  1947               Cuba 1947-11-06 R/2/1424
 4    77       2     1    41  1947              Haiti 1947-11-06 R/2/1424
 5    77       2     1    42  1947 Dominican Republic 1947-11-06 R/2/1424
 6    77       2     2    70  1947             Mexico 1947-11-06 R/2/1424
 7    77       2     1    90  1947          Guatemala 1947-11-06 R/2/1424
 8    77       2     2    91  1947           Honduras 1947-11-06 R/2/1424
 9    77       2     2    92  1947        El Salvador 1947-11-06 R/2/1424
10    77       2     1    93  1947          Nicaragua 1947-11-06 R/2/1424
# ... with 350,022 more rows, and 2 more variables: topic <chr>,
#   has_topic <dbl>
>
> # Summarize the percentage "yes" per country-year-topic
> by_country_year_topic <- votes_tidied %>%
    group_by(country, year, topic) %>%
    summarize(total = n(), percent_yes = mean(vote == 1)) %>%
    ungroup()
>
> # Print by_country_year_topic
> by_country_year_topic
# A tibble: 26,968 x 5
       country  year                                topic total percent_yes
         <chr> <dbl>                                <chr> <int>       <dbl>
 1 Afghanistan  1947                          Colonialism     8   0.5000000
 2 Afghanistan  1947                 Economic development     1   0.0000000
 3 Afghanistan  1947                         Human rights     1   0.0000000
 4 Afghanistan  1947                 Palestinian conflict     6   0.0000000
 5 Afghanistan  1949         Arms control and disarmament     3   0.0000000
 6 Afghanistan  1949                          Colonialism    22   0.8636364
 7 Afghanistan  1949                 Economic development     1   1.0000000
 8 Afghanistan  1949                         Human rights     3   0.0000000
 9 Afghanistan  1949 Nuclear weapons and nuclear material     3   0.0000000
10 Afghanistan  1949                 Palestinian conflict    11   0.8181818
# ... with 26,958 more rows
```

### Visualizing trends

```
> ## Load the ggplot2 package
> library(ggplot2)
>
> # Filter by_country_year_topic for just the US
> US_by_country_year_topic <- by_country_year_topic %>%
    filter(country == "United States")
>
> # Plot % yes over time for the US, faceting by topic
> ggplot(US_by_country_year_topic, aes(year, percent_yes)) +
    geom_line() +
    facet_wrap(~ topic)
```

nest topic into country

```
> # Load purrr, tidyr, and broom
> library(purrr)
> library(tidyr)
> library(broom)
>
> # Print by_country_year_topic
> by_country_year_topic
# A tibble: 26,968 x 5
       country  year                                topic total percent_yes
         <chr> <dbl>                                <chr> <int>       <dbl>
 1 Afghanistan  1947                          Colonialism     8   0.5000000
 2 Afghanistan  1947                 Economic development     1   0.0000000
 3 Afghanistan  1947                         Human rights     1   0.0000000
 4 Afghanistan  1947                 Palestinian conflict     6   0.0000000
 5 Afghanistan  1949         Arms control and disarmament     3   0.0000000
 6 Afghanistan  1949                          Colonialism    22   0.8636364
 7 Afghanistan  1949                 Economic development     1   1.0000000
 8 Afghanistan  1949                         Human rights     3   0.0000000
 9 Afghanistan  1949 Nuclear weapons and nuclear material     3   0.0000000
10 Afghanistan  1949                 Palestinian conflict    11   0.8181818
# ... with 26,958 more rows
>
> # Fit model on the by_country_year_topic dataset
> country_topic_coefficients <- by_country_year_topic %>%
    nest(-country, -topic) %>%
    mutate(model = map(data, ~ lm(percent_yes ~ year, data = .)),
           tidied = map(model, tidy)) %>%
    unnest(tidied)
Warning message: essentially perfect fit: summary may be unreliable
>
> # Print country_topic_coefficients
> country_topic_coefficients
# A tibble: 2,383 x 7
       country                        topic        term      estimate
         <chr>                        <chr>       <chr>         <dbl>
 1 Afghanistan                  Colonialism (Intercept)  -9.196506325
 2 Afghanistan                  Colonialism        year   0.005106200
 3 Afghanistan         Economic development (Intercept) -11.476390441
 4 Afghanistan         Economic development        year   0.006239157
 5 Afghanistan                 Human rights (Intercept)  -7.265379964
 6 Afghanistan                 Human rights        year   0.004075877
 7 Afghanistan         Palestinian conflict (Intercept) -13.313363338
 8 Afghanistan         Palestinian conflict        year   0.007167675
 9 Afghanistan Arms control and disarmament (Intercept) -13.759624843
10 Afghanistan Arms control and disarmament        year   0.007369733
# ... with 2,373 more rows, and 3 more variables: std.error <dbl>,
#   statistic <dbl>, p.value <dbl>
```

Filter only terms = year, then add p.adjust value less than 0.05 \(significant\)

```
> # Create country_topic_filtered
>
> country_topic_filtered <- country_topic_coefficients %>%
  filter(term == "year") %>%
  mutate(p.adjusted = p.adjust(p.value)) %>%
  filter(p.adjusted < 0.05)
```

filter only vanuatu

```
> # Create vanuatu_by_country_year_topic
> vanuatu_by_country_year_topic <- by_country_year_topic %>%
  filter(country == "Vanuatu")
>
> # Plot of percentage "yes" over time, faceted by topic
> ggplot(vanuatu_by_country_year_topic, aes(year, percent_yes)) +
    geom_line() +
    facet_wrap(~ topic)
```

## joins

##### Keys

primary key is on the master data table

foreign key is the key on the second databae

key can be a combination of two values

```
> head(artists)
# A tibble: 6 × 3
   first     last instrument
   <chr>    <chr>      <chr>
1  Jimmy  Buffett     Guitar
2 George Harrison     Guitar
3   Mick   Jagger     Vocals
4    Tom    Jones     Vocals
5   Davy    Jones     Vocals
6   John   Lennon     Guitar
> head(bands)
# A tibble: 6 × 3
      first     last         band
      <chr>    <chr>        <chr>
1      John   Bonham Led Zeppelin
2 John Paul    Jones Led Zeppelin
3     Jimmy     Page Led Zeppelin
4    Robert    Plant Led Zeppelin
5    George Harrison  The Beatles
6      John   Lennon  The Beatles
> # Complete the code to join artists to bands
> bands2 <- left_join(bands, artists, by = c("first", "last"))
>
> # Examine the results
> bands2
# A tibble: 13 × 4
       first      last               band instrument
       <chr>     <chr>              <chr>      <chr>
1       John    Bonham       Led Zeppelin       <NA>
2  John Paul     Jones       Led Zeppelin       <NA>
3      Jimmy      Page       Led Zeppelin     Guitar
4     Robert     Plant       Led Zeppelin       <NA>
5     George  Harrison        The Beatles     Guitar
6       John    Lennon        The Beatles     Guitar
7       Paul McCartney        The Beatles       Bass
8      Ringo     Starr        The Beatles      Drums
9      Jimmy   Buffett  The Coral Reefers     Guitar
10      Mick    Jagger The Rolling Stones     Vocals
11     Keith  Richards The Rolling Stones     Guitar
12   Charlie     Watts The Rolling Stones       <NA>
13    Ronnie      Wood The Rolling Stones       <NA>A right
```

##### A right join to reverse

```
> bands2 <- left_join(bands, artists, by = c("first", "last"))
> bands3 <- right_join(artists, bands, by = c("first", "last"))
>
> # Check that bands3 is equal to bands2
> setequal(bands2, bands3)
TRUE
```

##### variations of joins

```r
> ## Join albums to songs using inner_join()
> inner_join(songs, albums, by = "album")
# A tibble: 3 × 6
            song                album  first      last        band  year
           <chr>                <chr>  <chr>     <chr>       <chr> <int>
1  Come Together           Abbey Road   John    Lennon The Beatles  1969
2       Dream On            Aerosmith Steven     Tyler   Aerosmith  1973
3 Hello, Goodbye Magical Mystery Tour   Paul McCartney The Beatles  1967
>
> # Join bands to artists using full_join()
> full_join(artists, bands, by = c("first", "last"))
# A tibble: 21 × 4
    first      last instrument               band
    <chr>     <chr>      <chr>              <chr>
1   Jimmy   Buffett     Guitar  The Coral Reefers
2  George  Harrison     Guitar        The Beatles
3    Mick    Jagger     Vocals The Rolling Stones
4     Tom     Jones     Vocals               <NA>
5    Davy     Jones     Vocals               <NA>
6    John    Lennon     Guitar        The Beatles
7    Paul McCartney       Bass        The Beatles
8   Jimmy      Page     Guitar       Led Zeppelin
9     Joe     Perry     Guitar               <NA>
10  Elvis   Presley     Vocals               <NA>
# ... with 11 more rows
```

##### Pipes

```r
> # Find guitarists in bands dataset (don't change)
> temp <- left_join(bands, artists, by = c("first", "last"))
> temp <- filter(temp, instrument == "Guitar")
> select(temp, first, last, band)
# A tibble: 5 × 3
   first     last               band
   <chr>    <chr>              <chr>
1  Jimmy     Page       Led Zeppelin
2 George Harrison        The Beatles
3   John   Lennon        The Beatles
4  Jimmy  Buffett  The Coral Reefers
5  Keith Richards The Rolling Stones
>
> # Reproduce code above using pipes
> bands %>%
    left_join(artists, by = c("first", "last")) %>%
    filter(instrument == "Guitar") %>%
    select(first, last, band)
# A tibble: 5 × 3
   first     last               band
   <chr>    <chr>              <chr>
1  Jimmy     Page       Led Zeppelin
2 George Harrison        The Beatles
3   John   Lennon        The Beatles
4  Jimmy  Buffett  The Coral Reefers
5  Keith Richards The Rolling Stones
```

##### Pipes and joins

```r
> goal
# A tibble: 3 × 6
  first      last instrument        band             song                album
  <chr>     <chr>      <chr>       <chr>            <chr>                <chr>
1   Tom     Jones     Vocals        <NA> It's Not Unusual     Along Came Jones
2  John    Lennon     Guitar The Beatles    Come Together           Abbey Road
3  Paul McCartney       Bass The Beatles   Hello, Goodbye Magical Mystery Tour
> head(artists)
# A tibble: 6 × 3
   first     last instrument
   <chr>    <chr>      <chr>
1  Jimmy  Buffett     Guitar
2 George Harrison     Guitar
3   Mick   Jagger     Vocals
4    Tom    Jones     Vocals
5   Davy    Jones     Vocals
6   John   Lennon     Guitar
> head(bands)
# A tibble: 6 × 3
      first     last         band
      <chr>    <chr>        <chr>
1      John   Bonham Led Zeppelin
2 John Paul    Jones Led Zeppelin
3     Jimmy     Page Led Zeppelin
4    Robert    Plant Led Zeppelin
5    George Harrison  The Beatles
6      John   Lennon  The Beatles
> head(songs)
# A tibble: 4 × 4
              song                album  first      last
             <chr>                <chr>  <chr>     <chr>
1    Come Together           Abbey Road   John    Lennon
2         Dream On            Aerosmith Steven     Tyler
3   Hello, Goodbye Magical Mystery Tour   Paul McCartney
4 It's Not Unusual     Along Came Jones    Tom     Jones
> # Examine the contents of the goal dataset
> goal
# A tibble: 3 × 6
  first      last instrument        band             song                album
  <chr>     <chr>      <chr>       <chr>            <chr>                <chr>
1   Tom     Jones     Vocals        <NA> It's Not Unusual     Along Came Jones
2  John    Lennon     Guitar The Beatles    Come Together           Abbey Road
3  Paul McCartney       Bass The Beatles   Hello, Goodbye Magical Mystery Tour
>
> # Create goal2 using full_join() and inner_join()
> goal2 <- artists %>%
    full_join(bands, by = c("first", "last")) %>%
    inner_join(songs, by = c("first", "last"))
>
> # Check that goal and goal2 are the same
> setequal(goal, goal2)
TRUE
```

##### full\_join on 3 sets

```r
> # Create one table that combines all information
> artists %>%
    full_join(bands, by = c("first", "last")) %>%
    full_join(songs, by = c("first", "last")) %>%
    full_join(albums, by = c("album", "band"))
# A tibble: 29 × 7
    first      last instrument               band             song
    <chr>     <chr>      <chr>              <chr>            <chr>
1   Jimmy   Buffett     Guitar  The Coral Reefers             <NA>
2  George  Harrison     Guitar        The Beatles             <NA>
3    Mick    Jagger     Vocals The Rolling Stones             <NA>
4     Tom     Jones     Vocals               <NA> It's Not Unusual
5    Davy     Jones     Vocals               <NA>             <NA>
6    John    Lennon     Guitar        The Beatles    Come Together
7    Paul McCartney       Bass        The Beatles   Hello, Goodbye
8   Jimmy      Page     Guitar       Led Zeppelin             <NA>
9     Joe     Perry     Guitar               <NA>             <NA>
10  Elvis   Presley     Vocals               <NA>             <NA>
# ... with 19 more rows, and 2 more variables: album <chr>, year <int>
```

##### semi joins

returns a copy of the dataset. Gets a copy of the first set without additional columns. It is easier to semijoin on the second dataset

```r
> # View the output of semi_join()
> artists %>%
    semi_join(songs, by = c("first", "last"))
# A tibble: 3 × 3
  first      last instrument
  <chr>     <chr>      <chr>
1  John    Lennon     Guitar
2  Paul McCartney       Bass
3   Tom     Jones     Vocals
>
> # Create the same result as before with a long code
> artists %>%
    right_join(songs, by = c("first", "last")) %>%
    filter( !is.na(instrument)) %>%
    select(first, last, instrument)
# A tibble: 3 × 3
  first      last instrument
  <chr>     <chr>      <chr>
1  John    Lennon     Guitar
2  Paul McCartney       Bass
3   Tom     Jones     Vocals
>
```

##### count the records of semi joins

```r
> albums %>%
    # Collect the albums made by a band
    semi_join(bands, by = "band") %>%
    # Count the albums made by a band
    nrow()
[1] 5
```

##### Anti-joins

shows which records do NOT match from the second database. Which ones doesnt have matches \(i.e. gramma errors\)

```r
## Return rows of artists that don't have bands info
> #artists
> head(artists)
# A tibble: 6 × 3
   first     last instrument
   <chr>    <chr>      <chr>
1  Jimmy  Buffett     Guitar
2 George Harrison     Guitar
3   Mick   Jagger     Vocals
4    Tom    Jones     Vocals
5   Davy    Jones     Vocals
6   John   Lennon     Guitar
>
> #bands
> head(bands)
# A tibble: 6 × 3
      first     last         band
      <chr>    <chr>        <chr>
1      John   Bonham Led Zeppelin
2 John Paul    Jones Led Zeppelin
3     Jimmy     Page Led Zeppelin
4    Robert    Plant Led Zeppelin
5    George Harrison  The Beatles
6      John   Lennon  The Beatles
>
> #anti join
> artists %>%
    anti_join(bands, by = c("first", "last"))
# A tibble: 8 × 3
  first    last instrument
  <chr>   <chr>      <chr>
1 Nancy  Wilson     Vocals
2 Brian  Wilson     Vocals
3   Joe   Walsh     Guitar
4  Paul   Simon     Guitar
5 Elvis Presley     Vocals
6   Joe   Perry     Guitar
7  Davy   Jones     Vocals
8   Tom   Jones     Vocals
>
```

check misspelling

```r
# Check whether album names in labels are mis-entered
labels %>%
  anti_join(albums, by = "album")
```

check count

```
> ## Determine which key joins labels and songs
> labels
# A tibble: 9 × 2
                      album           label
                      <chr>           <chr>
1                Abbey Road           Apple
2         A Hard Days Night      Parlophone
3      Magical Mystery Tour      Parlophone
4           Led Zeppelin IV        Atlantic
5 The Dark Side of the Moon         Harvest
6          Hotel California          Asylum
7                   Rumours Warner Brothers
8                 Aerosmith        Columbia
9          Beggar's Banquet           Decca
> songs
# A tibble: 4 × 4
              song                album  first      last
             <chr>                <chr>  <chr>     <chr>
1    Come Together           Abbey Road   John    Lennon
2         Dream On            Aerosmith Steven     Tyler
3   Hello, Goodbye Magical Mystery Tour   Paul McCartney
4 It's Not Unusual     Along Came Jones    Tom     Jones
>
> # Check your understanding
> songs %>%
    # Find the rows of songs that match a row in labels
    semi_join(labels, by = "album") %>%
    # Number of matches between labels and songs
    nrow()
[1] 3
```

set operations

```

```

##### Binds
rbind and cbind , not to be used
bind_rows combine two datasets in the same order. Sometimes cannot be told if the row 1 of first dataset belongs to row1 of the second datasets
Returns a tibboe that can be easy
Can handle dataframes
If binded are passed as character string, then it is set as ID

pass two dates and set `.id` argument for the ID Example for merging two data frames named `band1` and `band2` and creating a new dataset naming all data from each band with their respective (Beatles or Stones)
`bind_rows(Beatles = band1,  Stones = Band2, .id = "Band")`

```
> # Examine side_one and side_two
> side_one
# A tibble: 5 × 2
                      song     length
                     <chr>     <time>
1              Speak to Me  5400 secs
2                  Breathe  9780 secs
3               On the Run 12600 secs
4                     Time 24780 secs
5 The Great Gig in the Sky 15300 secs
> side_two
# A tibble: 5 × 2
                 song     length
                <chr>     <time>
1               Money 23400 secs
2         Us and Them 28260 secs
3 Any Colour You Like 12240 secs
4        Brain Damage 13800 secs
5             Eclipse  7380 secs
>
> # Bind side_one and side_two into a single dataset
> side_one %>%
   bind_rows(side_two)
# A tibble: 10 × 2
                       song length
                      <chr>  <dbl>
1               Speak to Me   5400
2                   Breathe   9780
3                On the Run  12600
4                      Time  24780
5  The Great Gig in the Sky  15300
6                     Money  23400
7               Us and Them  28260
8       Any Colour You Like  12240
9              Brain Damage  13800
10                  Eclipse   7380
```

##### Bind bind_rows

Binding rows of a tibble
`discography` contains a data frame of each album by The Jimi Hendrix Experience and the year of the album.

`jimi` contains a list of data frames of album tracks, one for each album released by The Jimi Hendrix Experience.

```{r}
> ## Examine discography and jimi
> discography
# A tibble: 3 × 2
                album  year
                <chr> <int>
1 Are You Experienced  1967
2  Axis: Bold as Love  1967
3   Electric Ladyland  1968
> jimi
$`Are You Experienced`
# A tibble: 10 × 2
                       song     length
                      <chr>     <time>
1               Purple Haze  9960 secs
2          Manic Depression 13560 secs
3                   Hey Joe 12180 secs
4          May This Be Love 11640 secs
5        I Don't Live Today 14100 secs
6       The Wind Cries Mary 12060 secs
7                      Fire  9240 secs
8  Third Stone from the Sun 24000 secs
9                 Foxy Lady 11700 secs
10     Are You Experienced? 14100 secs

$`Axis: Bold As Love`
# A tibble: 13 × 2
                   song     length
                  <chr>     <time>
1                   EXP  6900 secs
2     Up from the Skies 10500 secs
3  Spanish Castle Magic 10800 secs
4   Wait Until Tomorrow 10800 secs
5      Ain't No Telling  6360 secs
6           Little Wing  8640 secs
7            If 6 was 9 19920 secs
8    You Got Me Floatin  9900 secs
9  Castles Made of Sand  9960 secs
10        She's So Fine  9420 secs
11       One Rainy Wish 13200 secs
12    Little Miss Lover  8400 secs
13         Bold as Love 15060 secs

$`Electric Ladyland`
# A tibble: 16 × 2
                                         song     length
                                        <chr>     <time>
1                      And the Gods Made Love  4860 secs
2   Have You Ever Been (To Electric Ladyland)  7860 secs
3                           Crosstown Traffic  8700 secs
4                                Voodoo Chile 54000 secs
5                         Little Miss Strange 10320 secs
6                       Long Hot Summer Night 12420 secs
7                            Come On (Part 1) 14940 secs
8                                  Gypsy Eyes 13380 secs
9                Burning of the Midnight Lamp 13140 secs
10                      Rainy Day, Dream Away 13320 secs
11     1983... (A Merman I Should Turn to Be) 49140 secs
12 Moon, Turn the Tides... Gently Gently Away  3720 secs
13              Still Raining, Still Dreaming 15900 secs
14                         House Burning Down 16380 secs
15                   All Along the Watchtower 14460 secs
16               Voodoo Child (Slight Return) 18720 secs
>
> jimi %>%
    # Bind jimi into a single data frame
    bind_rows(.id = "album") %>%
    # Make a complete data frame
    left_join(discography)
Joining, by = "album"
# A tibble: 39 × 4
                 album                     song length  year
                 <chr>                    <chr>  <dbl> <int>
1  Are You Experienced              Purple Haze   9960  1967
2  Are You Experienced         Manic Depression  13560  1967
3  Are You Experienced                  Hey Joe  12180  1967
4  Are You Experienced         May This Be Love  11640  1967
5  Are You Experienced       I Don't Live Today  14100  1967
6  Are You Experienced      The Wind Cries Mary  12060  1967
7  Are You Experienced                     Fire   9240  1967
8  Are You Experienced Third Stone from the Sun  24000  1967
9  Are You Experienced                Foxy Lady  11700  1967
10 Are You Experienced     Are You Experienced?  14100  1967
# ... with 29 more rows

```

##### Bind columns

```{r}
> # Examine hank_years and hank_charts
> hank_years
# A tibble: 67 × 2
    year                                    song
   <int>                                   <chr>
1   1947                         Move It On Over
2   1947    My Love for You (Has Turned to Hate)
3   1947 Never Again (Will I Knock on Your Door)
4   1947    On the Banks of the Old Ponchartrain
5   1947                            Pan American
6   1947             Wealth Won't Save Your Soul
7   1948                   A Mansion on the Hill
8   1948                           Honky Tonkin'
9   1948                         I Saw the Light
10  1948                   I'm a Long Gone Daddy
# ... with 57 more rows
> hank_charts
# A tibble: 67 × 2
                              song  peak
                             <chr> <int>
1  (I Heard That) Lonesome Whistle     9
2     (I'm Gonna) Sing, Sing, Sing    NA
3                 A Home in Heaven    NA
4            A Mansion on the Hill    12
5             A Teardrop on a Rose    NA
6        At the First Fall of Snow    NA
7       Baby, We're Really in Love     4
8                California Zephyr    NA
9                      Calling You    NA
10                Cold, Cold Heart     1
# ... with 57 more rows
>
> hank_years %>%
    # Reorder hank_years alphabetically by song title
    arrange(song) %>%
    # Select just the year column
    select(year) %>%
    # Bind the year column
    bind_cols(hank_charts) %>%
    # Arrange the finished dataset
    arrange(year, song)
# A tibble: 67 × 3
    year                                    song  peak
   <int>                                   <chr> <int>
1   1947                         Move It On Over     4
2   1947    My Love for You (Has Turned to Hate)    NA
3   1947 Never Again (Will I Knock on Your Door)    NA
4   1947    On the Banks of the Old Ponchartrain    NA
5   1947                            Pan American    NA
6   1947             Wealth Won't Save Your Soul    NA
7   1948                   A Mansion on the Hill    12
8   1948                           Honky Tonkin'    14
9   1948                         I Saw the Light    NA
10  1948             I'm So Lonesome I Could Cry     2
# ... with 57 more rows
```

##### data_frame

```{r}
# Make combined data frame using data_frame()
 data_frame(year = hank_year, song = hank_song, peak = hank_peak) %>%
   # Extract songs where peak equals 1
   filter(peak == 1)
# A tibble: 11 × 3
   year                                   song  peak
  <int>                                  <chr> <int>
1   1949                         Lovesick Blues     1
2   1950               Long Gone Lonesome Blues     1
3   1950                      Moanin the Blues     1
4   1950                  Why Dont You Love Me     1
5   1951                       Cold, Cold Heart     1
6   1951                       Hey Good Lookin'     1
7   1952 Ill Never Get Out of This World Alive     1
8   1952               Jambalaya (On the Bayou)     1
9   1953                               Kaw-Liga     1
10  1953        Take These Chains from My Heart     1
11  1953                    Your Cheatin' Heart     1
```



```{r}
> # Examine the contents of hank
> hank
$year
 [1] 1947 1947 1947 1947 1947 1947 1948 1948 1948 1948 1948 1949 1949 1949 1949
[16] 1949 1949 1949 1949 1950 1950 1950 1950 1950 1950 1950 1950 1951 1951 1951
[31] 1951 1951 1951 1951 1951 1952 1952 1952 1952 1952 1952 1953 1953 1953 1953
[46] 1953 1953 1954 1954 1954 1954 1955 1955 1955 1955 1955 1956 1956 1956 1956
[61] 1957 1957 1957 1958 1965 1966 1989

$song
 [1] "Move It On Over"
 [2] "My Love for You (Has Turned to Hate)"
 [3] "Never Again (Will I Knock on Your Door)"
 [4] "On the Banks of the Old Ponchartrain"
 [5] "Pan American"
 [6] "Wealth Won't Save Your Soul"
 [7] "A Mansion on the Hill"
 [8] "Honky Tonkin'"
 [9] "I Saw the Light"
[10] "I'm So Lonesome I Could Cry"
[11] "My Sweet Love Ain't Around"
[12] "I'm Satisfied with You"
[13] "Lost Highway"
[14] "Lovesick Blues"
[15] "Mind Your Own Business"
[16] "My Bucket's Got a Hole in It"
[17] "Never Again (Will I Knock on Your Door)"
[18] "Wedding Bells"
[19] "You're Gonna Change (Or I'm Gonna Leave)"
[20] "I Just Don't Like This Kind of Living"
[21] "Long Gone Lonesome Blues"
[22] "Moanin' the Blues"
[23] "My Son Calls Another Man Daddy"
[24] "Nobody's Lonesome for Me"
[25] "They'll Never Take Her Love from Me"
[26] "Why Don't You Love Me"
[27] "Why Should We Try Anymore"
[28] "(I Heard That) Lonesome Whistle"
[29] "Baby, We're Really in Love"
[30] "Cold, Cold Heart"
[31] "Crazy Heart"
[32] "Dear John"
[33] "Hey Good Lookin'"
[34] "Howlin' At the Moon"
[35] "I Can't Help It (If I'm Still in Love With You)"
[36] "Half as Much"
[37] "Honky Tonk Blues"
[38] "I'll Never Get Out of This World Alive"
[39] "Jambalaya (On the Bayou)"
[40] "Settin' the Woods on Fire"
[41] "You Win Again"
[42] "Calling You"
[43] "I Won't Be Home No More"
[44] "Kaw-Liga"
[45] "Take These Chains from My Heart"
[46] "Weary Blues from Waitin'"
[47] "Your Cheatin' Heart"
[48] "(I'm Gonna) Sing, Sing, Sing"
[49] "How Can You Refuse Him Now"
[50] "I'm a Long Gone Daddy"
[51] "You Better Keep It on Your Mind"
[52] "A Teardrop on a Rose"
[53] "At the First Fall of Snow"
[54] "Mother Is Gone"
[55] "Please Don't Let Me Love You"
[56] "Thank God"
[57] "A Home in Heaven"
[58] "California Zephyr"
[59] "Singing Waterfall"
[60] "There's a Tear in My Beer"
[61] "Leave Me Alone with the Blues"
[62] "Ready to Go Home"
[63] "The Waltz of the Wind"
[64] "Just Waitin'"
[65] "The Pale Horse and His Rider"
[66] "Kaw-Liga"
[67] "There's No Room in My Heart for the Blues"

$peak
 [1]  4 NA NA NA NA NA 12 14 NA  2 NA NA 12  1  5  2  6  2  4  5  1  1  9  9  5
[26]  1  9  9  4  1  4  8  1  3  2  2  2  1  1  2 10 NA  4  1  1  7  1 NA NA  6
[51] NA NA NA NA  9 NA NA NA NA  7 NA NA NA NA NA NA NA
>
> # Convert the hank list into a data frame
> as_data_frame(hank) %>%
    # Extract songs where peak equals 1
    filter(peak == 1)
# A tibble: 11 × 3
    year                                   song  peak
   <int>                                  <chr> <int>
1   1949                         Lovesick Blues     1
2   1950               Long Gone Lonesome Blues     1
3   1950                      Moanin' the Blues     1
4   1950                  Why Don't You Love Me     1
5   1951                       Cold, Cold Heart     1
6   1951                       Hey Good Lookin'     1
7   1952 I'll Never Get Out of This World Alive     1
8   1952               Jambalaya (On the Bayou)     1
9   1953                               Kaw-Liga     1
10  1953        Take These Chains from My Heart     1
11  1953
```

##### bind rows of a tibble which is a list of dataframes

```{r}
> # Examine the contents of michael
> michael %>%

  # as_data_frame(michael) %>%
    bind_rows( .id = "album") %>%
    group_by(album) %>%
    mutate(rank = min_rank(peak)) %>%
    filter(rank == 1) %>%
    select(-rank, -peak)
Source: local data frame [16 x 2]
Groups: album [10]

              album                           song
              <chr>                          <chr>
1   Got to Be There                  Rockin' Robin
2               Ben                            Ben
3        Music & Me           With a Child's Heart
4  Forever, Michael       Just a Little Bit of You
5      Off the Wall Don't Stop 'Til You Get Enough
6      Off the Wall                  Rock with You
7          Thriller                        Beat It
8          Thriller                    Billie Jean
9               Bad                            Bad
10              Bad       The Way You Make Me Feel
11              Bad              Man in the Mirror
12              Bad   I Just Can't Stop Loving You
13              Bad                    Dirty Diana
14        Dangerous                 Black or White
15          HIStory              You Are Not Alone
16       Invincible              You Rock My World
> # Examine the contents of michael
> michael
$`Got to Be There`
# A tibble: 10 × 2
                                song  peak
                               <chr> <int>
1                  Ain'T No Sunshine    NA
2           I Wanna be Where You Are    NA
3  Girl Don't Take Your Love from Me    NA
4                   In Our Small Way    NA
5                    Got to Be There     4
6                      Rockin' Robin     2
7                   Wings of My Love    NA
8      Maria (You Were the Only One)    NA
9   Love is Here and Now You're Gone    NA
10               You've Got a Friend    NA

$Ben
# A tibble: 10 × 2
                              song  peak
                             <chr> <int>
1                              Ben     1
2           Greatest Show On Earth    NA
3  People Make the World Go 'Round    NA
4           We've Got a Good Thing    NA
5      Everybody's Somebody's Fool    NA
6                          My Girl    NA
7    What Goes Around Comes Around    NA
8                 In Our Small Way    NA
9        Shoo-Be-Doo-Be-Doo-Da-Day    NA
10      You Can Cry On My Shoulder    NA

$`Music & Me`
# A tibble: 10 × 2
                     song  peak
                    <chr> <int>
1    With a Child's Heart    50
2                Up Again    NA
3  All the Things You Are    NA
4                   Happy    NA
5               Too Young    NA
6          Doggin' Around    NA
7                Euphoria    NA
8            Morning Glow    NA
9            Johnny Raven    NA
10           Music and Me    NA

$`Forever, Michael`
# A tibble: 10 × 2
                       song  peak
                      <chr> <int>
1        We're Almost There    54
2              Take Me Back    NA
3      One Day in Your Life    NA
4    Cinderella Stay Awhile    NA
5         We've Got Forever    NA
6  Just a Little Bit of You    23
7             You Are There    NA
8                Dapper-Dan    NA
9              Dear Michael    NA
10    I'll Come Home to You    NA

$`Off the Wall`
# A tibble: 10 × 2
                             song  peak
                            <chr> <int>
1  Don't Stop 'Til You Get Enough     1
2                   Rock with You     1
3           Working Day and Night    NA
4                Get on the Floor    NA
5                    Off the Wall    10
6                      Girlfriend    NA
7            She's Out of My Life    10
8                 I Can't Help It    NA
9        It's the Falling in Love    NA
10            Burn This Disco Out    NA

$Thriller
# A tibble: 9 × 2
                         song  peak
                        <chr> <int>
1  Wanna Be Startin' Somethin     5
2                Baby be Mine    NA
3            The Girl is Mine     2
4                    Thriller     4
5                     Beat It     1
6                 Billie Jean     1
7                Human Nature     7
8 P.Y.T. (Pretty Young Thing)    10
9         The Lady in My Life    NA

$Bad
# A tibble: 10 × 2
                           song  peak
                          <chr> <int>
1                           Bad     1
2      The Way You Make Me Feel     1
3                   Speed Demon    NA
4                 Liberian Girl    NA
5             Just Good Friends    NA
6            Another Part of Me    11
7             Man in the Mirror     1
8  I Just Can't Stop Loving You     1
9                   Dirty Diana     1
10              Smooth Criminal     7

$Dangerous
# A tibble: 14 × 2
                       song  peak
                      <chr> <int>
1                       Jam    26
2  Why You Wanna Trip on Me    NA
3             In the Closet     6
4        She Drives Me Wild    NA
5         Remember the Time     3
6    Can't Let Her Get Away    NA
7            Heal the World    27
8            Black or White     1
9                 Who Is It    14
10            Give In to Me    NA
11        Will You Be There     7
12           Keep the Faith    NA
13            Gone Too Soon    NA
14                Dangerous    NA

$HIStory
# A tibble: 30 × 2
                           song  peak
                          <chr> <int>
1                   Billie Jean    NA
2      The Way You Make Me Feel    NA
3                Black or White    NA
4                 Rock with You    NA
5          She's Out of My Life    NA
6                           Bad    NA
7  I Just Can't Stop Loving You    NA
8             Man in the Mirror    NA
9                      Thriller    NA
10                      Beat It    NA
# ... with 20 more rows

$Invincible
# A tibble: 16 × 2
                song  peak
               <chr> <int>
1        Unbreakable    NA
2       Heartbreaker    NA
3         Invincible    NA
4      Break of Dawn    NA
5    Heaven Can Wait    NA
6  You Rock My World    10
7        Butterflies    NA
8         Speechless    NA
9         2000 Watts    NA
10   You Are My Life    NA
11           Privacy    NA
12   Don't Walk Away    NA
13               Cry    NA
14 The Lost Children    NA
15  Whatever Happens    NA
16        Threatened    NA
>
> # as_data_frame(michael) %>%
>   bind_rows(michael, .id = "album") %>%
    group_by(album) %>%
    mutate(rank = min_rank(peak)) %>%
    filter(rank == 1) %>%
    select(-rank, -peak)
Source: local data frame [16 x 2]
Groups: album [10]

              album                           song
              <chr>                          <chr>
1   Got to Be There                  Rockin' Robin
2               Ben                            Ben
3        Music & Me           With a Child's Heart
4  Forever, Michael       Just a Little Bit of You
5      Off the Wall Don't Stop 'Til You Get Enough
6      Off the Wall                  Rock with You
7          Thriller                        Beat It
8          Thriller                    Billie Jean
9               Bad                            Bad
10              Bad       The Way You Make Me Feel
11              Bad              Man in the Mirror
12              Bad   I Just Cant Stop Loving You
13              Bad                    Dirty Diana
14        Dangerous                 Black or White
15          HIStory              You Are Not Alone
16       Invincible              You Rock My World

```

##### binding different factor country_topic_coefficients

```{r}
> ##
> #sixties stores year as numeric (double)
> #seventies stores year as factor
> #change the column year to read factor as character and convert to numeric
> #result will be a tibble with year as numeric (double)
> seventies
# A tibble: 10 × 3
     year                      album                 band
   <fctr>                      <chr>                <chr>
1    1970 Bridge Over Troubled Water  Simon and Garfunkel
2    1971     Jesus Christ Superstar      Various Artists
3    1972                    Harvest           Neil Young
4    1973      The World is a Ghetto                  War
5    1974  Goodbye Yellow Brick Road           Elton John
6    1975 Elton Johns Greatest Hits           Elton John
7    1976             Peter Frampton Frampton Comes Alive
8    1977                    Rumours        Fleetwood Mac
9    1978       Saturday Night Fever             Bee Gees
10   1979                 Billy Joel          52nd Street
> sixties
# A tibble: 10 × 3
    year                          album                            band
   <dbl>                          <chr>                           <chr>
1   1960             The Sound of Music          Original Broadway Cast
2   1961                        Camelot          Original Broadway Cast
3   1962                West Side Story                      Soundtrack
4   1963                West Side Story                      Soundtrack
5   1964                  Hello, Dolly!          Original Broadway Cast
6   1965                   Mary Poppins                      Soundtrack
7   1966 Whipped Cream & Other Delights Herb Alpert & The Tijuana Brass
8   1967            More of The Monkees                     The Monkees
9   1968           Are You Experienced?     The Jimi Hendrix Experience
10  1969             In-A-Gadda-Da-Vida                  Iron Butterfly
> seventies %>%
    # Coerce seventies$year into a useful numeric
    mutate(year = as.numeric(as.character(year))) %>%
    # Bind the updated version of seventies to sixties
    bind_rows(sixties) %>%
    arrange(year)
# A tibble: 20 × 3
    year                          album                            band
   <dbl>                          <chr>                           <chr>
1   1960             The Sound of Music          Original Broadway Cast
2   1961                        Camelot          Original Broadway Cast
3   1962                West Side Story                      Soundtrack
4   1963                West Side Story                      Soundtrack
5   1964                  Hello, Dolly!          Original Broadway Cast
6   1965                   Mary Poppins                      Soundtrack
7   1966 Whipped Cream & Other Delights Herb Alpert & The Tijuana Brass
8   1967            More of The Monkees                     The Monkees
9   1968           Are You Experienced?     The Jimi Hendrix Experience
10  1969             In-A-Gadda-Da-Vida                  Iron Butterfly
11  1970     Bridge Over Troubled Water             Simon and Garfunkel
12  1971         Jesus Christ Superstar                 Various Artists
13  1972                        Harvest                      Neil Young
14  1973          The World is a Ghetto                             War
15  1974      Goodbye Yellow Brick Road                      Elton John
16  1975     Elton Johns Greatest Hits                      Elton John
17  1976                 Peter Frampton            Frampton Comes Alive
18  1977                        Rumours                   Fleetwood Mac
19  1978           Saturday Night Fever                        Bee Gees
20  1979                     Billy Joel                     52 Street
>
```

### Advance join

What can go wrong?
missing keys or duplicates Keys

##### Missing key values
one dataset with missing values
if possible, you can remove the data with filter
Example with two data sets namesNA and plays

```{r}
namesNA %>%
 filter(!is.na(name)) %>%
 left_join(playes, by = "name")
```
`rownames_to_column` can add a rowname to get a key
`rownames_to_column(noNames, var = "name")` will return a copy of the dataframe with a new column to be used

```{r}
> # Load the tibble package
> library(tibble)
>
> stage_songs %>%
    # Add row names as a column named song
    rownames_to_column(var = "song") %>%
    # Left join stage_writers to stage_songs
   left_join(stage_writers)
Joining, by = "song"
                    song              musical year            composer
1   Children Will Listen       Into the Woods 1986    Stephen Sondheim
2                  Maria      West Side Story 1957   Leonard Bernstein
3                 Memory                 Cats 1981 Andrew Lloyd Webber
4 The Music of the Night Phantom of the Opera 1986 Andrew Lloyd Webber

```



##### duplicate keys
If the primary set has keys, dplyr will keep the duplicates

##### Missing Keys
removing NAs from a joined datasets

```{r}
> ##singers
> singers
# A tibble: 2 × 2
               movie                singer
               <chr>                 <chr>
1               <NA> Arnold Schwarzenegger
2 The Sound of Music         Julie Andrews
> #two_Songs
> two_songs
# A tibble: 2 × 2
                 song              movie
                <chr>              <chr>
1            Do-Re-Mi The Sound of Music
2 A Spoonful of Sugar               <NA>
> # Examine the result of joining singers to two_songs
> two_songs %>% inner_join(singers, by = "movie")
# A tibble: 2 × 3
                 song              movie                singer
                <chr>              <chr>                 <chr>
1            Do-Re-Mi The Sound of Music         Julie Andrews
2 A Spoonful of Sugar               <NA> Arnold Schwarzenegger
>
> # Remove NA's from key before joining
> two_songs %>%
    filter(!is.na(movie)) %>%
    inner_join(singers, by = "movie")
# A tibble: 1 × 3
      song              movie        singer
     <chr>              <chr>         <chr>
1 Do-Re-Mi The Sound of Music Julie Andrews
>
```

### Defining the Keys

it is not necessary to mention the key, but it can be missmatched.
To match colums along datasets we need to declare it as vector

`left_join(members, plays, by = c("member" = "name"))` will join members dataset with plays dataset and have member column equal to name column as key. Can be used with single or multiple columns

`left_join(playsWith, plays, by "name")` in this case, dataset playsWith will be merged with plays and a suffix by default will be added

##### add a subset of Keys
left join two datasets by movie and then rename each column.

```{r}
> #movie_studios
> movie_studios
# A tibble: 10 × 2
                     movie                  name
                     <chr>                 <chr>
1      The Road to Morocco    Paramount Pictures
2             Going My Way    Paramount Pictures
3           Anchors Aweigh   Metro-Goldwyn-Mayer
4  Till the Clouds Roll By   Metro-Goldwyn-Mayer
5          White Christmas    Paramount Pictures
6          The Tender Trap   Metro-Goldwyn-Mayer
7             High Society   Metro-Goldwyn-Mayer
8        The Joker is Wild    Paramount Pictures
9                 Pal Joey     Columbia Pictures
10                 Can-Can Twentieth-Century Fox
>
> #movie_years
> movie_years
# A tibble: 10 × 3
                     movie          name  year
                     <chr>         <chr> <int>
1      The Road to Morocco   Bing Crosby  1942
2             Going My Way   Bing Crosby  1944
3           Anchors Aweigh Frank Sinatra  1945
4  Till the Clouds Roll By Frank Sinatra  1946
5          White Christmas   Bing Crosby  1954
6          The Tender Trap Frank Sinatra  1955
7             High Society   Bing Crosby  1956
8        The Joker is Wild Frank Sinatra  1957
9                 Pal Joey Frank Sinatra  1957
10                 Can-Can Frank Sinatra  1960
>
> movie_years %>%
    # Left join movie_studios to movie_years
    left_join(movie_studios, by = "movie") %>%
    # Rename the columns: artist and studio
    rename(artist = name.x, studio = name.y)
# A tibble: 10 × 4
                     movie        artist  year                studio
                     <chr>         <chr> <int>                 <chr>
1      The Road to Morocco   Bing Crosby  1942    Paramount Pictures
2             Going My Way   Bing Crosby  1944    Paramount Pictures
3           Anchors Aweigh Frank Sinatra  1945   Metro-Goldwyn-Mayer
4  Till the Clouds Roll By Frank Sinatra  1946   Metro-Goldwyn-Mayer
5          White Christmas   Bing Crosby  1954    Paramount Pictures
6          The Tender Trap Frank Sinatra  1955   Metro-Goldwyn-Mayer
7             High Society   Bing Crosby  1956   Metro-Goldwyn-Mayer
8        The Joker is Wild Frank Sinatra  1957    Paramount Pictures
9                 Pal Joey Frank Sinatra  1957     Columbia Pictures
10                 Can-Can Frank Sinatra  1960 Twentieth-Century Fo
```

##### Mis-matched key names
When there are two datasets and the column names do not match

```{r}
> # Identify the key column
> elvis_songs
# A tibble: 5 × 2
                                  name          movie
                                 <chr>          <chr>
1 (You're So Square) Baby I Don't Care Jailhouse Rock
2         I Can't Help Falling in Love    Blue Hawaii
3                       Jailhouse Rock Jailhouse Rock
4                       Viva Las Vegas Viva Las Vegas
5                    You Don't Know Me       Clambake
> elvis_movies
# A tibble: 4 × 2
            name  year
           <chr> <int>
1 Jailhouse Rock  1957
2    Blue Hawaii  1961
3 Viva Las Vegas  1963
4       Clambake  1967
>
> elvis_movies %>%
    # Left join elvis_songs to elvis_movies by this column
    left_join(elvis_songs, by = c("name" = "movie")) %>%
    # Rename columns
    rename(movie = name, song = name.y)
# A tibble: 5 × 3
           movie  year                                 song
           <chr> <int>                                <chr>
1 Jailhouse Rock  1957 (You're So Square) Baby I Don't Care
2 Jailhouse Rock  1957                       Jailhouse Rock
3    Blue Hawaii  1961         I Can't Help Falling in Love
4 Viva Las Vegas  1963                       Viva Las Vegas
5       Clambake  1967                    You Don't Know Me

```

##### Mis matched and rename columns

```{r}
> # Identify the key columns
> movie_directors
# A tibble: 10 × 3
                      name        director                studio
                     <chr>           <chr>                 <chr>
1           Anchors Aweigh   George Sidney   Metro-Goldwyn-Mayer
2                  Can-Can     Walter Lang Twentieth-Century Fox
3             Going My Way     Leo McCarey    Paramount Pictures
4             High Society Charles Walters   Metro-Goldwyn-Mayer
5                 Pal Joey   George Sidney     Columbia Pictures
6        The Joker is Wild   Charles Vidor    Paramount Pictures
7      The Road to Morocco    David Butler    Paramount Pictures
8          The Tender Trap Charles Walters   Metro-Goldwyn-Mayer
9  Till the Clouds Roll By   Richard Whorf   Metro-Goldwyn-Mayer
10         White Christmas  Michael Curtiz    Paramount Pictures
> movie_years
# A tibble: 10 × 3
                     movie          name  year
                     <chr>         <chr> <int>
1      The Road to Morocco   Bing Crosby  1942
2             Going My Way   Bing Crosby  1944
3           Anchors Aweigh Frank Sinatra  1945
4  Till the Clouds Roll By Frank Sinatra  1946
5          White Christmas   Bing Crosby  1954
6          The Tender Trap Frank Sinatra  1955
7             High Society   Bing Crosby  1956
8        The Joker is Wild Frank Sinatra  1957
9                 Pal Joey Frank Sinatra  1957
10                 Can-Can Frank Sinatra  1960
>
> movie_years %>%
    # Left join movie_directors to movie_years
    left_join(movie_directors, by = c("movie" = "name")) %>%
    # Arrange the columns using select()
    select(year, movie, artist = name, director, studio)
# A tibble: 10 × 5
    year                   movie        artist        director
   <int>                   <chr>         <chr>           <chr>
1   1942     The Road to Morocco   Bing Crosby    David Butler
2   1944            Going My Way   Bing Crosby     Leo McCarey
3   1945          Anchors Aweigh Frank Sinatra   George Sidney
4   1946 Till the Clouds Roll By Frank Sinatra   Richard Whorf
5   1954         White Christmas   Bing Crosby  Michael Curtiz
6   1955         The Tender Trap Frank Sinatra Charles Walters
7   1956            High Society   Bing Crosby Charles Walters
8   1957       The Joker is Wild Frank Sinatra   Charles Vidor
9   1957                Pal Joey Frank Sinatra   George Sidney
10  1960                 Can-Can Frank Sinatra     Walter Lang
# ... with 1 more variables: studio <chr>
```

### Joining multiple tables with purr

The best way to combine multiple tables is with reduce

```{r}
install.packages("purrr")
library(purr)
```

Example of using reduce

```{r}
tables <- list(surnames, names, plays)
reduce(tables, left_join, by = "name")
```

### Case Study Lahman Database

##### Universal keys

```{r}
# Examine lahmanNames
lahmanNames

# Find variables in common
reduce(lahmanNames, intersect)
```

###### Common Keys
binding the datasets

```{r}
lahmanNames %>%
  # Bind the data frames in lahmanNames. Creating a dataframe with all rows
  bind_rows(.id = "dataframe") %>%
  # Group the result by var. Because we need to identify var
  group_by(var) %>%
  # Tally the number of appearances. Order by appearance
  tally() %>%
  # Filter the data. Keep only the ones mayor to 1
  filter(n > 1) %>%
  # Arrange the results.
  arrange(desc(n))
```

```{r}
> lahmanNames %>%
    # Bind the data frames in lahmanNames
    bind_rows(.id = "dataframe") %>%
    # Group the result by var
    group_by(var) %>%
    # Tally the number of appearances
    tally() %>%
    # Filter the data
    filter(n > 1) %>%
    # Arrange the results
    arrange(desc(n))
# A tibble: 59 × 2
        var     n
      <chr> <int>
1    yearID    21
2  playerID    19
3      lgID    17
4    teamID    13
5         G    10
6         L     6
7         W     6
8        BB     5
9        CS     5
10       GS     5
# ... with 49 more rows

```
`yearID`, `playerID`, `lgID`, `teamID` are the most common variable namesNA

###### Player ID

Which datasets use `playerID`

```{r}
lahmanNames %>%
  # Bind the data frames
  bind_rows(.id = "dataframe") %>%
  # Filter the results
  filter(var == "playerID") %>%
  # Extract the dataframe variable
  `$`("dataframe")
  ```

  ```{r}
  > lahmanNames %>%
    # Bind the data frames
    bind_rows(.id = "dataframe") %>%
    # Filter the results
    filter(var == "playerID") %>%
    # Extract the dataframe variable
    `$`("dataframe")
 [1] "AllstarFull"         "Appearances"         "AwardsManagers"
 [4] "AwardsPlayers"       "AwardsShareManagers" "AwardsSharePlayers"
 [7] "Batting"             "BattingPost"         "CollegePlaying"
[10] "Fielding"            "FieldingOF"          "FieldingPost"
[13] "HallOfFame"          "Managers"            "ManagersHalf"
[16] "Master"              "Pitching"            "PitchingPost"
[19] "Salaries"

```

##### Salaries (unique)
Create a players list from Master and checking that there are unique records
```{r}
> players <- Master %>%
    # Return one row for each distinct player
    distinct(playerID, nameFirst, nameLast)
```

##### Find all players not shown in Salaries
using antijoin by playerId we found out that 13,888 players do not have salaries.

```{r}
> players %>%
    # Find all players who do not appear in Salaries
    anti_join(Salaries, by = "playerID") %>%
    # Count them
    count()
# A tibble: 1 × 1
      n
  <int>
1 13888
```

##### players with missing salaries in appearances

```{r}
> players %>%
    anti_join(Salaries, by = "playerID") %>%
    # How many unsalaried players appear in Appearances?
    semi_join(Appearances, by = "playerID") %>%
    count()
# A tibble: 1 × 1
      n
  <int>
1 13695
```

##### How many games

How many games each of unpaid salaried players played?

```{r}
> players %>%
    # Find all players who do not appear in Salaries
    anti_join(Salaries, by = "playerID") %>%
    # Join them to Appearances
    left_join(Appearances, by = "playerID") %>%
    # Calculate total_games for each player
    group_by(playerID) %>%
    summarize(total_games = sum(G_all, na.rm = TRUE)) %>%
    # Arrange in descending order by total_games
    arrange(desc(total_games))
# A tibble: 13,888 × 2
    playerID total_games
       <chr>       <int>
1  yastrca01        3308
2  aaronha01        3298
3   cobbty01        3034
4  musiast01        3026
5   mayswi01        2992
6  robinbr01        2896
7  kalinal01        2834
8  collied01        2826
9  robinfr02        2808
10 wagneho01        2794
# ... with 13,878 more rows
```

### check at bat players

```{r}
> players %>%
    # Find unsalaried players
    anti_join(Salaries, by = "playerID") %>%
    # Join Batting to the unsalaried players
    left_join(Batting, by = "playerID") %>%
    # Group by player
    group_by(playerID) %>%
    # Sum at-bats for each player
    summarize(total_at_bat = sum(AB, na.rm = TRUE)) %>%
    # Arrange in descending order
    arrange(desc(total_at_bat))
# A tibble: 13,888 × 2
    playerID total_at_bat
       <chr>        <int>
1  aaronha01        12364
2  yastrca01        11988
3   cobbty01        11434
4  musiast01        10972
5   mayswi01        10881
6  robinbr01        10654
7  wagneho01        10430
8  brocklo01        10332
9  ansonca01        10277
10 aparilu01        10230
# ... with 13,878 more rows
```

#### How players have been nominated in hall of frame

```{r}
> # Find the distinct players that appear in HallOfFame
> nominated <- HallOfFame %>%
    distinct(playerID)
>
> nominated %>%
    # Count the number of players in nominated
    count()
# A tibble: 1 × 1
      n
  <int>
1  1239
>
> nominated_full <- nominated %>%
    # Join to Master
    left_join(Master, by = "playerID") %>%
    # Return playerID, nameFirst, nameLast
    select(playerID, nameFirst, nameLast)

```

There were 1,239 nominees for the Hall of Fame. We now have a dataset of everyone nominated!

####  Find and filter with inducted

```{r}
> # Find distinct players in HallOfFame with inducted == "Y"
> inducted <- HallOfFame %>%
    filter(inducted == "Y") %>%
    distinct(playerID)
>
> inducted %>%
    # Count the number of players in inducted
    count()
# A tibble: 1 × 1
      n
  <int>
1   312
>
> inducted_full <- inducted %>%
    # Join to Master
    left_join(Master, by = "playerID") %>%
    # Return playerID, nameFirst, nameLast
    select(playerID, nameFirst, nameLast)

```

#### tally and filter

```{r}
> # Tally the number of awards in AwardsPlayers by playerID
> nAwards <- AwardsPlayers %>%
    group_by(playerID) %>%
    tally()
>
> nAwards %>%
    # Filter to just the players in inducted
    semi_join(inducted, by = "playerID") %>%
    # Calculate the mean number of awards per player
    summarize(avg_n = mean(n, na.rm = TRUE))
# A tibble: 1 × 1
     avg_n
     <dbl>
1 11.95161
>
> nAwards %>%
    # Filter to just the players in nominated
    semi_join(nominated, by = "playerID") %>%
    # Filter to players NOT in inducted
    anti_join(inducted, by = "playerID") %>%
    # Calculate the mean number of awards per player
    summarize(avg_n = mean(n, na.rm = TRUE))
# A tibble: 1 × 1
     avg_n
     <dbl>
1 4.226923
```

#### Salary Analysis
```{r}
> # Find the players who are in nominated, but not inducted
> notInducted <- nominated %>%
    setdiff(inducted)
>
> Salaries %>%
    # Find the players who are in notInducted
    semi_join(notInducted, by = "playerID") %>%
    # Calculate the max salary by player
    group_by(playerID) %>%
    summarize(max_salary = max(salary, na.rm = TRUE)) %>%
    # Calculate the average of the max salaries
    summarize(avg_salary = mean(max_salary, na.rm = TRUE))
# A tibble: 1 × 1
  avg_salary
       <dbl>
1    4677737
>
> # Repeat for players who were inducted
> Salaries %>%
    semi_join(inducted, by = "playerID") %>%
    group_by(playerID) %>%
    summarize(max_salary = max(salary, na.rm = TRUE)) %>%
    summarize(avg_salary = mean(max_salary, na.rm = TRUE))
# A tibble: 1 × 1
  avg_salary
       <dbl>
1    5079720
```

#### Retirement
Players cannot be nominated until five years after retirement. Is this true? Check last appaearances for users nominated to Hall of Fame

```{r}
> Appearances %>%
    # Filter Appearances against nominated
    semi_join(nominated, by = "playerID") %>%
    # Find last year played by player
    group_by(playerID) %>%
    summarize(last_year = max(yearID)) %>%
    # Join to full HallOfFame
    left_join(HallOfFame, by = "playerID") %>%
    # Filter for unusual observations
    filter(last_year >= yearID)
# A tibble: 39 × 10
    playerID last_year yearID         votedBy ballots needed votes inducted
       <chr>     <int>  <int>           <chr>   <int>  <int> <int>   <fctr>
1  cissebi01      1938   1937           BBWAA     201    151     1        N
2  cochrmi01      1937   1936           BBWAA     226    170    80        N
3   deandi01      1947   1945           BBWAA     247    186    17        N
4   deandi01      1947   1946    Final Ballot     263    198    45        N
5   deandi01      1947   1946 Nominating Vote     202     NA    40        N
6   deandi01      1947   1947           BBWAA     161    121    88        N
7  dickebi01      1946   1945           BBWAA     247    186    17        N
8  dickebi01      1946   1946 Nominating Vote     202     NA    40        N
9  dickebi01      1946   1946    Final Ballot     263    198    32        N
10 dimagjo01      1951   1945           BBWAA     247    186     1        N
# ... with 29 more rows, and 2 more variables: category <fctr>,
#   needed_note <chr>

````
