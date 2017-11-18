# dplyr package

# tibble is special type of dataframe

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

```
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

```
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

```
# Filter for votes related to colonialism
filter(votes_joined, co == 1)
```



```
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


