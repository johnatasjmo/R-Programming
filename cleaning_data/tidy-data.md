# tidyr Package

### Tidy Data

Observations on rows

Variables on columns

When column headers are variable is not tidy

Wide vs long is not tidy

##### Use of gather\(\)

```
> bmi_long <- gather(bmi, year, bmi_val, -Country)
> 
> # View the first 20 rows of the result
> head(bmi_long, n = 20)
               Country  year  bmi_val
1          Afghanistan Y1980 21.48678
2              Albania Y1980 25.22533
3              Algeria Y1980 22.25703
4              Andorra Y1980 25.66652
5               Angola Y1980 20.94876
6  Antigua and Barbuda Y1980 23.31424
7            Argentina Y1980 25.37913
8              Armenia Y1980 23.82469
9            Australia Y1980 24.92729
10             Austria Y1980 24.84097
11          Azerbaijan Y1980 24.49375
12             Bahamas Y1980 24.21064
13             Bahrain Y1980 23.97588
14          Bangladesh Y1980 20.51918
15            Barbados Y1980 24.36372
16             Belarus Y1980 24.90898
17             Belgium Y1980 25.09879
18              Belize Y1980 24.54345
19               Benin Y1980 20.80754
20             Bermuda Y1980 25.07881

> albania <- subset(bmi_long, Country == "Albania")
> head(albania)
    Country  year  bmi_val
2   Albania Y1980 25.22533
201 Albania Y1981 25.23981
400 Albania Y1982 25.25636
599 Albania Y1983 25.27176
798 Albania Y1984 25.27901
997 Albania Y1985 25.28669
```

##### Use of spread\(\)

```r
spread(data, key, value, fill = NA, convert = FALSE, drop = TRUE, sep = NULL)
```



```r
> # Apply spread() to bmi_long
> bmi_wide <- spread(bmi_long, year, bmi_val)
> 
> # View the head of bmi_wide
> head(bmi_wide)
              Country    Y1980    Y1981    Y1982    Y1983    Y1984    Y1985
1         Afghanistan 21.48678 21.46552 21.45145 21.43822 21.42734 21.41222
2             Albania 25.22533 25.23981 25.25636 25.27176 25.27901 25.28669
3             Algeria 22.25703 22.34745 22.43647 22.52105 22.60633 22.69501
4             Andorra 25.66652 25.70868 25.74681 25.78250 25.81874 25.85236
5              Angola 20.94876 20.94371 20.93754 20.93187 20.93569 20.94857
6 Antigua and Barbuda 23.31424 23.39054 23.45883 23.53735 23.63584 23.73109
     Y1986    Y1987    Y1988    Y1989    Y1990    Y1991    Y1992    Y1993
1 21.40132 21.37679 21.34018 21.29845 21.24818 21.20269 21.14238 21.06376
2 25.29451 25.30217 25.30450 25.31944 25.32357 25.28452 25.23077 25.21192
3 22.76979 22.84096 22.90644 22.97931 23.04600 23.11333 23.18776 23.25764
4 25.89089 25.93414 25.98477 26.04450 26.10936 26.17912 26.24017 26.30356
5 20.96030 20.98025 21.01375 21.05269 21.09007 21.12136 21.14987 21.13938
6 23.83449 23.93649 24.05364 24.16347 24.26782 24.36568 24.45644 24.54096
     Y1994    Y1995    Y1996    Y1997    Y1998    Y1999    Y2000    Y2001
1 20.97987 20.91132 20.85155 20.81307 20.78591 20.75469 20.69521 20.62643
2 25.22115 25.25874 25.31097 25.33988 25.39116 25.46555 25.55835 25.66701
3 23.32273 23.39526 23.46811 23.54160 23.61592 23.69486 23.77659 23.86256
4 26.36793 26.43569 26.50769 26.58255 26.66337 26.75078 26.83179 26.92373
5 21.14186 21.16022 21.19076 21.22621 21.27082 21.31954 21.37480 21.43664
6 24.60945 24.66461 24.72544 24.78714 24.84936 24.91721 24.99158 25.05857
     Y2002    Y2003    Y2004    Y2005    Y2006    Y2007    Y2008
1 20.59848 20.58706 20.57759 20.58084 20.58749 20.60246 20.62058
2 25.77167 25.87274 25.98136 26.08939 26.20867 26.32753 26.44657
3 23.95294 24.05243 24.15957 24.27001 24.38270 24.48846 24.59620
4 27.02525 27.12481 27.23107 27.32827 27.43588 27.53363 27.63048
5 21.51765 21.59924 21.69218 21.80564 21.93881 22.08962 22.25083
6 25.13039 25.20713 25.29898 25.39965 25.51382 25.64247 25.76602
```

##### Use of unite\(\) and separate\(\)

```
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
> mtcars_unite <- unite(mtcars, "vs_am", c("vs","am"))
> head(mtcars_unite)
                   mpg cyl disp  hp drat    wt  qsec vs_am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46   0_1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02   0_1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61   1_1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44   1_0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02   0_0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22   1_0    3    1

> mtcars_separate <- separate(mtcars_unite, vs_am, "vs", "am")
> head(mtcars_separate)
                   mpg cyl disp  hp drat    wt  qsec  vs gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46 0_1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02 0_1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61 1_1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44 1_0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02 0_0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22 1_0    3    1
```

Use of separate with vector when sep variable is needed

```
## sales3 is pre-loaded in your workspace

# Load tidyr
library(tidyr)

# Split event_date_time: sales4
sales4 <- separate(sales3, event_date_time, 
                   c("event_dt", "event_time"), sep = " ")

# Split sales_ord_create_dttm: sales5
sales5 <- separate(sales4, sales_ord_create_dttm, 
                   c("ord_create_dt", "ord_create_time"), sep = " ")
```

##### 

##### Use of gather\(\)

gather uses first value is dataset, second the name of the first column, third name of second column and lastly with - sign, the name of column that we do not wish to gather

```
## tidyr and dplyr are already loaded for you

# View the head of census
head(census)

# Gather the month columns
census2 <- gather(census, month, amount, -YEAR)

# Arrange rows by YEAR using dplyr's arrange
census2 <- arrange(census2, YEAR)

# View first 20 rows of census2
head(census2, 20)
```



