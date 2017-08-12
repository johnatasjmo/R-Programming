# Basics

##### Entering input

```
> x <- 1> print(x)
[1] 1
> x[1] 1
```

Print

```
> x <- 11:30
> x[1] 11 12 13 14 15 16 17 18 19 20 21 22[13] 23 24 25 26 27 28 29 30
```

Objects

R has five basic objects: characters, numeric, integer, complex, logical

Attributes: names, dimnames \(eg. matrices, arrays\), dimensions \(integer, numeric\), class, lenght, other user-defined

```
> x <- c(0.5, 0.6) ## numeric
> x <- c("a", "b", "c") ## character
> x <- c(1+0i, 2+4i) ## complex
```

Mixing objects

```
> y <- c(1.7, "a") ## character
> y <- c(TRUE, 2) ## numeric
> y <- c("a", TRUE) ## character
```



Explicit coercion

```
> x <- 0:6> class(x)[1] "integer"> as.numeric(x)[1] 0 1 2 3 4 5 6> as.logical(x)[1] FALSE TRUE TRUE TRUE TRUE TRUE TRUE> as.character(x)[1] "0" "1" "2" "3" "4" "5" "6"
```



