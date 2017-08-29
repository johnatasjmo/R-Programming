# Simulation & Profiling

##### Str function

A diagnostic function and alternative summary

```
> f <- gl(40,10)
> summary (f)
 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 
10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 
22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 
10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 
> str(f)
 Factor w/ 40 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
> head(f)
[1] 1 1 1 1 1 1
40 Levels: 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 ... 40
```

Simulation

rnorm: generate random Normal variates with a given meand and standard deviation

dnorm: evaluate the Normal probability density \(with a given mean/SD\) at a point \(or vector of points\)

pnorm: evaluate the cumulative distribution function for a Normal distribution

rpolis: generate random Poisson variates with a given rate

d for density

r for random number generation

p for cumulative distribution

q for quartile function

```
dnorm(x, mean = 0, sd = 1, log = FALSE)
pnorm(q, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE)
qnorm(p, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE)
rnorm(n, mean = 0, sd = 1)
```

```
> x <- rnorm(10)
> x
 [1]  2.0724152 -0.3363667 -0.9007546 -0.2624824
 [5]  1.2511648 -0.7227638  0.6022470 -1.2073834
 [9]  1.5341297  0.7468664
> x <- rnorm(10,20,2) # 10 numbers with mean of 10 and sd of 2
> x
 [1] 21.04184 20.54080 18.34019 22.33039 20.67315
 [6] 17.54794 22.56533 15.78623 21.55598 23.5179
> summary(x)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
```

##### Seed on randon numbers

Set se sequence or random numbers every time is set, for reproduction

```
> set.seed(1)
> rnorm(5)
[1] -0.6264538  0.1836433 -0.8356286  1.5952808
[5]  0.3295078
> rnorm(5)
[1] -0.8204684  0.4874291  0.7383247  0.5757814
[5] -0.3053884
> set.seed(1)
> rnorm(5)
[1] -0.6264538  0.1836433 -0.8356286  1.5952808
[5]  0.3295078
```

```
dpois(x, lambda, log = FALSE)
ppois(q, lambda, lower.tail = TRUE, log.p = FALSE)
qpois(p, lambda, lower.tail = TRUE, log.p = FALSE)
rpois(n, lambda)
```

```
> rpois (10,1) #randonm probability of 10 records with lambda of 1
[1] 0 0 1 1 2 1 1 4 1 2

> ppois(2,2)  # probability of posaain randon variable is less than or equal to 2
[1] 0.6766764
```

### Profiling

To examina how much tom is spend in different parts, to optimize your code.

* First design, then optimize. Collect data

`system.time()` it gives time in seconds

**user time: **amount of time charged to CPU

**elapsed time:** "walk clock" time

Usually both times are the same, but can be different. Parallel package can process in parallel.

MAC: vecLib/Accelerate

```
system.time(readLines("http://www.jhsph.edu"))

s <- hilbert(100)
system.time(svd(x)) # this will be executed in 2 CPUS and faster
```

##### The R Profiler

`rprof()`

`summaryRprof()`

do not use sustem.time and rprof together. Default sampling 0.02 seconds, if run fast, do not need to use it.

`by.total`

`by.self` is more useful as substract low level functions.

`sample.interval`



### Swirl: looking at data

Check the class of the database

```
> class(plants) # check the class of the database
[1] "data.frame

> dim(plants) # check rows and variables, 5166 rows and 10 variables
[1] 5166   10

> nrow(plants)
[1] 5166

> ncol(plants)
[1] 10

> object.size(plants) # check how much space is data base
644232 bytes

> names(plants)  # character vector of variable names
 [1] "Scientific_Name"      "Duration"            
 [3] "Active_Growth_Period" "Foliage_Color"       
 [5] "pH_Min"               "pH_Max"              
 [7] "Precip_Min"           "Precip_Max"          
 [9] "Shade_Tolerance"      "Temp_Min_F" 
 
 > head(plants)  # preview the data
               Scientific_Name          Duration
1                  Abelmoschus              <NA>
2       Abelmoschus esculentus Annual, Perennial
3                        Abies              <NA>
4               Abies balsamea         Perennial
5 Abies balsamea var. balsamea         Perennial
6                     Abutilon              <NA>
  Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min
1                 <NA>          <NA>     NA     NA         NA
2                 <NA>          <NA>     NA     NA         NA
3                 <NA>          <NA>     NA     NA         NA
4    Spring and Summer         Green      4      6         13
5                 <NA>          <NA>     NA     NA         NA
6                 <NA>          <NA>     NA     NA         NA
  Precip_Max Shade_Tolerance Temp_Min_F
1         NA            <NA>         NA
2         NA            <NA>         NA
3         NA            <NA>         NA
4         60        Tolerant        -43
5         NA            <NA>         NA
6         NA            <NA>         NA

> head(plants, 10) # shows the first 10 rows

> tail(plants) # shows the last rows
> tail(plants, 15) # shows the last 15 records
> summary(plants) # shows a summary of all columns, including NA's
> table(plants$Active_Growth_Period) # how many times a data ocurrs in the data with table active gworth

> str(plants)
'data.frame':	5166 obs. of  10 variables:
 $ Scientific_Name     : Factor w/ 5166 levels "Abelmoschus",..: 1 2 3 4 5 6 7 8 9 10 ...
 $ Duration            : Factor w/ 8 levels "Annual","Annual, Biennial",..: NA 4 NA 7 7 NA 1 NA 7 7 ...
 $ Active_Growth_Period: Factor w/ 8 levels "Fall, Winter and Spring",..: NA NA NA 4 NA NA NA NA 4 NA ...
 $ Foliage_Color       : Factor w/ 6 levels "Dark Green","Gray-Green",..: NA NA NA 3 NA NA NA NA 3 NA ...
 $ pH_Min              : num  NA NA NA 4 NA NA NA NA 7 NA ...
 $ pH_Max              : num  NA NA NA 6 NA NA NA NA 8.5 NA ...
 $ Precip_Min          : int  NA NA NA 13 NA NA NA NA 4 NA ...
 $ Precip_Max          : int  NA NA NA 60 NA NA NA NA 20 NA ...
 $ Shade_Tolerance     : Factor w/ 3 levels "Intermediate",..: NA NA NA 3 NA NA NA NA 2 NA ...
 $ Temp_Min_F          : int  NA NA NA -43 NA NA NA NA -13 NA ...
 
 
```

### Simulations

swirl 13 Simulation

```
> sample(1:6, 4, replace = TRUE)  # simulate rolling four six-sided dice 
[1] 2 1 6 2

# flips variable created for a sample of vector c(0,1)as binary,  with replacement and probabilities of c(0.3, 0.7)
> flips <- sample(c(0,1), 100, replace = TRUE, prob = c(0.3, 0.7))
> flips
  [1] 1 1 1 1 1 1 1 0 1 1 1 1 0 0 1 1 1 0 1 1 1 1 1 1 1 1 0 1 0 0 1 1 1 1
 [35] 1 1 1 1 1 1 1 1 1 1 1 1 1 0 0 0 0 1 0 1 0 0 1 0 0 1 1 1 0 1 0 1 1 1
 [69] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 0 0 0 1 1 0 1
 
 # now check the number of flips
 > sum(flips)
[1] 77

# binomial random variable of independent trials with specific number of sucess
> rbinom(1, size = 100, prob = 0.7)
[1] 59

> flips2 <- rbinom(100, size = 1, prob = 0.7) # see all 0s and 1s 
> flips2
  [1] 1 0 0 0 0 0 1 1 0 1 0 0 1 1 1 0 1 1 1 0 1 0 0 1 0 1 0 1 1 1 0 0 0 0
 [35] 1 1 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 0 0 0 1 0 0 1 1 0
 [69] 0 1 0 0 1 0 1 1 1 0 1 1 1 1 0 1 0 0 1 1 0 1 1 1 1 0 1 0 0 1 1 1
 
# 10 random numbers with normal distribution
  > rnorm(10)
 [1] -0.14710445 -0.67101186  0.02490357 -1.54296279  1.07708427
 [6] -0.23475420  1.95946200 -0.87981780  0.04064564  0.15525950
 
 # 10 numbers wtih mean of 100, standard deviation of 25
 > rnorm(10, mean =100, sd = 25)
 [1] 110.26467 123.32692  92.03604 142.98517  79.24166  94.10339
 [7] 101.04612 110.48012  74.67709 164.20260
 
 # 5 samples of rpois distribution with mean of 10
> rpois(5, 10) 

# ten times 5 samples of rpois with mean of 10
> replicate(100, rpois(5, 10))


```





