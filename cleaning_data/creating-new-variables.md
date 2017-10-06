# Creating new variables

### Types of variables

* character: "treatment", 123"
* numeric: 23.44, 120, NaN, Inf 
* integer:  4L, 1123L
* factor: factor\("hello\), factor\(8\)
* logical: TRUE, FALSE, NA

`class()`shows the variable class of each

`as.character`

`as.numeric`

`as.integer`

`as.factor`

`as.logical`

### Lubridate

```
> ymd("2015-08-25")
[1] "2015-08-25"
> ymd("2015 August 25")
[1] "2015-08-25"
> mdy("August, 25 2015")
[1] "2015-08-25"
> hms("13:33:00")
[1] "13H 33M 0S"
```

### Convert

```
# Make this evaluate to character
class("true")

# Make this evaluate to numeric
class(8484.00)

# Make this evaluate to integer
class(99L)

# Make this evaluate to factor
class(factor("factor"))

# Make this evaluate to logical
class(FALSE)
```

### String manipulation

`str_trim` trims white space

`str_pad` helps add white space

`str_detect`to check if there is a match

`str_replace` can also be replacing a total value

`tolower` letters in lowers

`toupper` letters in uppers

```
# Load the stringr package
library(stringr)

# Trim all leading and trailing whitespace
str_trim(c("   Filip ", "Nick  ", " Jonathan"))

# Pad these strings with leading zeros
str_pad(c("23485W", "8823453Q", "994Z"), 9, pad = "0")

## stringr has been loaded for you

# Look at the head of students2
head(students2)

# Detect all dates of birth (dob) in 1997
str_detect(students2$dob, "1997")

# In the sex column, replace "F" with "Female"...
students2$sex <- str_replace(students2$sex, "F", "Female")

# ...And "M" with "Male"
students2$sex <- str_replace(students2$sex, "M", "Male")

# View the head of students2
head(students2)
```

### Creating sequences

##### Baltimore dataset

```r
> list.files()
[1] "restaurants.csv"
> restData <- read.csv("restaurants.csv")
```

```r
> s1 <- seq(1,10, by=2) ;s1
[1] 1 3 5 7 9

Create a sequence of length
> s2 <- seq(1,10, length=3); s2
[1]  1.0  5.5 10.0

# create an index along the values
> x <- c(1,3,8,25,100); seq(along = x)
[1] 1 2 3 4 5
```

##### Subsetting variables

```r
> # Find restaurants only in these two neibourhoods that are IN
> restData$nearMe = restData$neighborhood %in% c("Roland Park", "Homeland") 
> table(restData$nearMe)

FALSE  TRUE 
 1314    13
```

##### Creating binary variables

```
> #if zipcode is less than 0. return TRUE if condition returns TRUE, else is FALSE
> restData$zipWrong = ifelse(restData$zipCode < 0, TRUE, FALSE)
> # make a table where zipcode is wrong and less than 0. One value is wrong.
> table(restData$zipWrong, restData$zipCode < 0)

        FALSE TRUE
  FALSE  1326    0
  TRUE      0    1
```

##### Creting categorical variables

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

##### Easier cutting \(easier way to group\)

```r
> library(Hmisc)
> restData$zipGroups = cut2(restData$zipCode,g=4)
> table(restData$zipGroups)

[-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
           338            375            300            314
```

##### Creating factor variables with `factor()`

```r
> # zipcode variable is an integer, we will convert it to a factor
> restData$zcf <- factor(restData$zipCode)
> restData$zcf[1:10]
 [1] 21206 21231 21224 21211 21223 21218 21205 21211 21205 21231
32 Levels: -21226 21201 21202 21205 21206 21207 21208 ... 21287
> class(restData$zcf)
[1] "factor"
```

Levels of factor variables

```r
> #create vector of yes no
> yesno <- sample(c("yes","no"), size=10, replace=TRUE)
> #turn into a factor variable, but treat level c in order yes then no
> yesnofac = factor(yesno,levels=c("yes","no"))
> relevel(yesnofac,ref="yes")
 [1] yes no  no  yes yes no  yes no  no  yes
Levels: yes no
> # change againg the order
> as.numeric(yesnofac)
 [1] 1 2 2 1 1 2 1 2 2 1
```

##### Cutting produces factor variables

```r
> library(Hmisc)
> restData$zipGroups = cut2(restData$zipCode, g=4)
> table(restData$zipGroups)

[-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
           338            375            300            314
```

##### using the mutate function

```r
> library(Hmisc)
> library(plyr)
# create a new version of the variable with mutate to apply to restData and apply zipGroups
# so the new data will be the old one with new groups
> restData2 = mutate(restData, zipGroups=cut2(zipCode,g=4))
> table(restData2$zipGroups)

[-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
           338            375            300            314
```



