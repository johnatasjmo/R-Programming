

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





