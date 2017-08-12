# Basics

##### Entering input

```
> x <- 1
> print(x)
[1] 1
> x
[1] 1
```

##### Print

```
> x <- 11:30
> x
[1] 11 12 13 14 15 16 17 18 19 20 21 22
[13] 23 24 25 26 27 28 29 30
```

##### Objects

R has five basic objects: characters, numeric, integer, complex, logical

Attributes: names, dimnames \(eg. matrices, arrays\), dimensions \(integer, numeric\), class, lenght, other user-defined

```
> x <- c(0.5, 0.6) ## numeric
> x <- c("a", "b", "c") ## character
> x <- c(1+0i, 2+4i) ## complex
```

##### Mixing objects

```
> y <- c(TRUE, 2) ## numeric
> y <- c("a", TRUE) ## character
```

##### Explicit coercion

```
> class(x)
[1] "integer"
> as.numeric(x)
[1] 0 1 2 3 4 5 6
> as.logical(x)
[1] FALSE TRUE TRUE TRUE TRUE TRUE TRUE
> as.character(x)
[1] "0" "1" "2" "3" "4" "5" "6"
```

##### Matrices

    > m <- matrix(nrow = 2, ncol = 3)
    > m
    [,1] [,2] [,3]
    [1,] NA NA NA
    [2,] NA NA NA
    > dim(m)
    [1] 2 3
    > attributes(m)
    $dim
    [1] 2 3
    `

Matrices are constructed column-wise starting upper left and running by columns

```
> m <- matrix(1:6, nrow = 2, ncol = 3)
> m
[,1] [,2] [,3]
[1,] 1 3 5
[2,] 2 4 6
```

Can also be created from vectors

```
> m <- 1:10
> m
[1] 1 2 3 4 5 6 7 8 9 10
> dim(m) <- c(2, 5)
> m
[,1] [,2] [,3] [,4] [,5]
[1,] 1 3 5 7 9
[2,] 2 4 6 8 10
```

column and row binding creation of matrices

```
> x <- 1:3
> y <- 10:12
> cbind(x, y)
x y
[1,] 1 10
[2,] 2 11
[3,] 3 12
> rbind(x, y)
[,1] [,2] [,3]
x 1 2 3
y 10 11 12
```



