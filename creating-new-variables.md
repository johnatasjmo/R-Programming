Creating new variables

Creating sequences

Baltimore dataset

```
> setwd("~/rWorking/coursera/CleaningData/week3/data")        
> list.files()
[1] "restaurants.csv"
> restData <- read.csv("restaurants.csv")
```

```
> s1 <- seq(1,10, by=2) ;s1
[1] 1 3 5 7 9

Create a sequence of length
> s2 <- seq(1,10, length=3); s2
[1]  1.0  5.5 10.0

# create an index along the values
> x <- c(1,3,8,25,100); seq(along = x)
[1] 1 2 3 4 5
```



Subsetting variables

```
> # Find restaurants only in these two neibourhoods that are IN
> restData$nearMe = restData$neighborhood %in% c("Roland Park", "Homeland") 
> table(restData$nearMe)

FALSE  TRUE 
 1314    13
```

Creating binary variables

```
> #if zipcode is less than 0. return TRUE if condition returns TRUE, else is FALSE
> restData$zipWrong = ifelse(restData$zipCode < 0, TRUE, FALSE)
> # make a table where zipcode is wrong and less than 0. One value is wrong.
> table(restData$zipWrong, restData$zipCode < 0)
       
        FALSE TRUE
  FALSE  1326    0
  TRUE      0    1
```

Creting categorical variables

```
> # create zigGroups and with a cut that brakes in quantiles
> restData$zipGroups = cut(restData$zipCode, breaks = quantile(restData$zipCode))
> table(restData$zipGroups)

(-2.123e+04,2.12e+04]  (2.12e+04,2.122e+04] (2.122e+04,2.123e+04] 
                  337                   375                   282 
(2.123e+04,2.129e+04] 
                  332 
                  
> # create a table with zipgroups and zipcodes to see where values reside
> table(restData$zipGroups, restData$zipCode)
                       
                        -21226 21201 21202 21205 21206 21207 21208
  (-2.123e+04,2.12e+04]      0   136   201     0     0     0     0
  (2.12e+04,2.122e+04]       0     0     0    27    30     4     1
  (2.122e+04,2.123e+04]      0     0     0     0     0     0     0
  (2.123e+04,2.129e+04]      0     0     0     0     0     0     0
                       
                        21209 21210 21211 21212 21213 21214 21215
  (-2.123e+04,2.12e+04]     0     0     0     0     0     0     0
  (2.12e+04,2.122e+04]      8    23    41    28    31    17    54
  (2.122e+04,2.123e+04]     0     0     0     0     0     0     0
  (2.123e+04,2.129e+04]     0     0     0     0     0     0     0
                       
                        21216 21217 21218 21220 21222 21223 21224
  (-2.123e+04,2.12e+04]     0     0     0     0     0     0     0
  (2.12e+04,2.122e+04]     10    32    69     0     0     0     0
  (2.122e+04,2.123e+04]     0     0     0     1     7    56   199
  (2.123e+04,2.129e+04]     0     0     0     0     0     0     0
                       
                        21225 21226 21227 21229 21230 21231 21234
  (-2.123e+04,2.12e+04]     0     0     0     0     0     0     0
  (2.12e+04,2.122e+04]      0     0     0     0     0     0     0
  (2.122e+04,2.123e+04]    19     0     0     0     0     0     0
  (2.123e+04,2.129e+04]     0    18     4    13   156   127     7
                       
                        21237 21239 21251 21287
  (-2.123e+04,2.12e+04]     0     0     0     0
  (2.12e+04,2.122e+04]      0     0     0     0
  (2.122e+04,2.123e+04]     0     0     0     0
  (2.123e+04,2.129e+04]     1     3     2     1
```

Easier cutting \(easier way to group\)

```
> library(Hmisc)
> restData$zipGroups = cut2(restData$zipCode,g=4)
> table(restData$zipGroups)

[-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
           338            375            300            314 
```



