Looping

lapply: loop over a list and evaluate a function on each element

sapply: same as lapply but try to simplify the result

apply: apply a function ver the margins of an array

tapply: apply a function over a subset of a vector

mpply: multivariat to lapply

slplit

lapply\(list, function\), always returns a list

runif : gets vectors

apply

```
x<- matrix(rnorm(200), 20, 10)  #creates a matrix with 20 rows and 10 columns
apply(x, 2, mean)  # apply the mean to x and return the second column
apply(x, 2, sum) # for each row calculate the sum
```

rowSums

rowMeans

colSums

ColMeans

apply\(x, 1, quantile, probs = c\(0.25, 0.75\)\)



##### lapply and sapply

`sapply()` tries to guess the correct format, `vapply()` allows to specify it explicity.

```
sapply(flags, unique) # returns unique function as list for each of flags columns
table(flags$animate) # as table how many values on each

#get the mean of animated characters on flag per landmass
> tapply(flags$animate, flags$landmass, mean)
        1         2         3         4         5         6 
0.4193548 0.1764706 0.1142857 0.1346154 0.1538462 0.3000000 


```







