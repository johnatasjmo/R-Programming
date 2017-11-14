dplyr package

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



