# Basics

##### Entering input

```
> x <- 1
[1] 1
> x
```

Print

```
> x <- 11:30
> x
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
> x <- 0:6
```


