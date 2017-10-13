qplot

```
qplot(displ, hwy, data = mpg, color=drv, geom = c("point", "smooth"))
```

gray areas indicate the 95% confidence interval

![](/assets/plot_smooth.png)

Whiskers

```
qplot(drv, hwy, data = mpg, geom = "boxplot", color = manufacturer)
```

![](/assets/plot_whiskers.png)

Histograms

first variable is the one for the frequency

```
qplot(hwy, data = mpg, fill =  drv)
```

![](/assets/plot_hist_one.png)

Arrangement

```
qplot(displ, hwy, data = mpg, facets = . ~ drv)
```

![](/assets/plot_hist_arr1.png)

```
qplot(hwy, data = mpg, facets = drv ~ ., binwidth = 2)
```

![](/assets/plot_hist_arr2.png)



```

| We've created a vector containing integers that are multiples of 617 for you. It's called brk.
| Look at it now.

> brk
 [1]     0   617  1234  1851  2468  3085  3702  4319  4936  5553  6170  6787  7404  8021  8638
[16]  9255  9872 10489 11106 11723 12340 12957 13574 14191 14808 15425 16042 16659 17276 17893
[31] 18510 19127

| We've also created a vector containing the number of diamonds with prices between each pair of
| adjacent entries of brk. For instance, the first count is the number of diamonds with prices
| between 0 and $617, and the second is the number of diamonds with prices between $617 and
| $1234. Look at the vector named counts now.
> counts
 [1]  4611 13255  5230  4262  3362  2567  2831  2841  2203  1666  1445  1112   987   766   796
[16]   655   606   553   540   427   429   376   348   338   298   305   269   287   227   251
[31]    97


qplot(price,data=diamonds,binwidth=18497/30,fill=cut)
```



```
qplot(price,data=diamonds,geom="density",color=cut)
```

```
qplot(carat,price,data=diamonds, color=cut) + geom_smooth(method="lm")
```

```
qplot(carat,price,data=diamonds, color=cut, facets=.~cut) + geom_smooth(method="lm")
```



