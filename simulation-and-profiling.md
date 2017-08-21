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
  15.79   18.89   20.86   20.39   22.14   23.52 
```



