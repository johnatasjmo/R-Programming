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

     m &lt;- matrix\(nrow = 2, ncol = 3\)  
     m  
         \[,1\] \[,2\] \[,3\]  
         \[1,\] NA NA NA  
         \[2,\] NA NA NA  
     dim\(m\)  
         \[1\] 2 3  
     attributes\(m\)  
         $dim  
         \[1\] 2 3  
         \`

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

##### Select from matrix

```
# All Rows and All Columns
df[,]
# First row and all columns
df[1,]
# First two rows and all columns
df[1:2,]
# First and third row and all columns
df[ c(1,3), ]
# First Row and 2nd and third column
df[1, 2:3]
# First, Second Row and Second and Third COlumn
df[1:2, 2:3]
# Just First Column with All rows
df[, 1]
# First and Third Column with All rows
df[,c(1,3)]
```

##### Lists

List are speciat tupe of vector that contains elements of different classes.

```
> x <- list(1, "a", TRUE, 1 + 4i)
> x
[[1]]
[1] 1
[[2]]
[1] "a"
[[3]]
[1] TRUE
[[4]]
[1] 1+4i
```

Create an empty list

```
> x <- vector("list", length = 5)
```

##### Factors

Factors are used to represent categorical data and can e unordered or ordered. Examples "Male" , "Female"

```
> x <- factor(c("yes", "yes", "no", "yes", "no"))
> x
[1] yes yes no yes no
Levels: no yes
> table(x)
x
no yes
2 3
```

##### Missing Values

• is.na\(\) is used to test objects if they are NA  
• is.nan\(\) is used to test for NaN

```
> ## Create a vector with NAs in it
> x <- c(1, 2, NA, 10, 3)

> ## Return a logical vector indicating which elements are NA
> is.na(x)
[1] FALSE FALSE TRUE FALSE FALSE

> ## Return a logical vector indicating which elements are NaN
> is.nan(x)
[1] FALSE FALSE FALSE FALSE FALSE

> ## Now create a vector with both NA and NaN values
> x <- c(1, 2, NaN, NA, 4)
> is.na(x)
[1] FALSE FALSE TRUE TRUE FALSE
> is.nan(x)
[1] FALSE FALSE TRUE FALSE FALSE
```
##### Data Frames

Data frames are used to store tabular data in R.
Unlikely matrices, data frames can store different classes of objects in each column.

Data frames are usually created by reading in a dataset using read.table() and read.csv().
data.matrix() converts a data frame to a matrix.

```
> x <- data.frame(foo = 1:4, bar = c(T, T, F, F))
> x
foo bar
1 1 TRUE
2 2 TRUE
3 3 FALSE
4 4 FALSE
> nrow(x)
[1] 4
> ncol(x)
[1] 2
```


##### Names
```
> x <- 1:3
> names(x)
NULL
> names(x) <- c("New York", "Seattle", "Los Angeles")
> x
New York Seattle Los Angeles
1 2 3
> names(x)
[1] "New York" "Seattle" "Los Angeles"

> x <- list("Los Angeles" = 1, Boston = 2, London = 3)

> m <- matrix(1:4, nrow = 2, ncol = 2)
> dimnames(m) <- list(c("a", "b"), c("c", "d"))
> m
c d
a 1 3
b 2 4

# colnames and rownames
> colnames(m) <- c("h", "f")
> rownames(m) <- c("x", "z")
> m
h f
x 1 3
z 2 4
```
