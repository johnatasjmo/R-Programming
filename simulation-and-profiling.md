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

`by.total  `

`by.self` is more useful as substract low level functions. 

`sample.interval`











