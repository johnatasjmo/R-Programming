Subsetting

```r
# creating a random matrix with NAs
> set.seed(13435)
> X <- data.frame("var1"=sample(1:5),"var2"=sample(6:10),"var3"=sample(11:15))
> X <- X[sample(1:5),]; X$var2[c(1,3)] = NA
> X
  var1 var2 var3
1    2   NA   15
4    1   10   11
2    3   NA   12
3    5    6   14
5    4    9   13
```

```r
# subsetting
> X[,1]
[1] 2 1 3 5 4
> X[,"var1"]
[1] 2 1 3 5 4


> X[1:2,"var2"] # output first two rows of X and second column of var2
[1] NA 10
```

```r
> X[(X$var1 <= 3 & X$var3 > 11)] # use of AND
  var1 var3
1    2   15
4    1   11
2    3   12
3    5   14
5    4   13

> X[(X$var1 <= 3 | X$var3 > 15),] # use of OR
  var1 var2 var3
1    2   NA   15
4    1   10   11
2    3   NA   12

> X[which(X$var2 > 8),] # dealing with NA values doesnt produce actual rows, use which.
  var1 var2 var3
4    1   10   11
5    4    9   13
```

Sorting

```r
> sort(X$var1)
[1] 1 2 3 4 5

> sort(X$var1, decreasing = TRUE)
[1] 5 4 3 2 1

> sort(X$var2, na.last=TRUE) # all NAs at last
[1]  6  9 10 NA NA

> X[order(X$var1),] # sort the data with order and apply to var1. Reorder rows by var1
  var1 var2 var3
4    1   10   11
1    2   NA   15
2    3   NA   12
5    4    9   13
3    5    6   14

> X[order(X$var1,X$var3),] # sorts by variable 1, then by var2
  var1 var2 var3
4    1   10   11
1    2   NA   15
2    3   NA   12
5    4    9   13
3    5    6   14
```

plyr package

use for command arrange

```r
install.packages("plyr")
library(plyr)
```

```r
> arrange(X,var1) #arrange by var1
  var1 var2 var3
1    1   10   11
2    2   NA   15
3    3   NA   12
4    4    9   13
5    5    6   14

> arrange(X,desc(var1)) # arrange by var1 descending
  var1 var2 var3
1    5    6   14
2    4    9   13
3    3   NA   12
4    2   NA   15
5    1   10   11
```

```r
> X$var4 <- rnorm(5) # add a new variable as extra column
> X
  var1 var2 var3       var4
1    2   NA   15  0.1875960
4    1   10   11  1.7869764
2    3   NA   12  0.4966936
3    5    6   14  0.0631830
5    4    9   13 -0.5361329

> Y <- cbind(X,rnorm(5)) # create a new data frame with a column named rnorm
> Y
  var1 var2 var3       var4    rnorm(5)
1    2   NA   15  0.1875960  0.62578490
4    1   10   11  1.7869764 -2.45083750
2    3   NA   12  0.4966936  0.08909424
3    5    6   14  0.0631830  0.47838570
5    4    9   13 -0.5361329  1.00053336

> Y <- cbind(rnorm(5),X)  # include rnorm at the first column, instead of last
> Y
    rnorm(5) var1 var2 var3       var4
1  0.5439561    2   NA   15  0.1875960
4  0.3304796    1   10   11  1.7869764
2 -0.9710917    3   NA   12  0.4966936
3 -0.9446847    5    6   14  0.0631830
5 -0.2967423    4    9   13 -0.5361329
```



