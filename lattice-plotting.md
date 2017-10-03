# Lattice Plotting System

* `library(lattice)` = load lattice system
* implemented using the `lattice` and `grid` packages
  * `lattice` package = contains code for producing _**Trellis**_ graphics \(independent from base graphics system\)
  * `grid` package = implements the graphing system; lattice build on top of grid
* all plotting and annotation is _**done with single function call**_
  * margins/spacing/labels set automatically for entire plot, good for putting multiple on the screen
  * good for conditioning plots $\rightarrow$ examining same plots over different conditions how y changes vs x across different levels of z
  * `panel` functions can be specified/customized to modify the subplots
* lattice graphics functions return an object of class "trellis", where as base graphics functions plot data directly to graphics device
  * print methods for lattice functions actually plots the data on graphics device
  * trellis objects are auto-printed
  * `trellis.par.set()` $\rightarrow$ can be used to set global graphic parameters for all trellis objects
* hard to annotate, awkward to specify entire plot in one function call
* cannot add to plot once created, panel/subscript functions hard to prepare

### `lattice` Functions and Parameters

* **Funtions**
  * `xyplot()` = main function for creating scatterplots
  * `bwplot()` = box and whiskers plots \(box plots\)
  * `histogram()` = histograms
  * `stripplot()` = box plot with actual points
  * `dotplot()` = plot dots on "violin strings"
  * `splom()` = scatterplot matrix \(like pairs\(\) in base plotting system\)
  * `levelplot()`/`contourplot()` = plotting image data
* **Arguments** for `xyplot(y ~ x | f * g, data, layout, panel)`
  * view x and y for every f and g
  * default blue open circles for data points
  * formula notation is used here \(`~`\) = left hand side is the y-axis variable, and the right hand side is the x-axis variable
  * `f`/`g` = conditioning/categorical variables \(optional\)
    * basically creates multi-panelled plots \(for different factor levels\)
    * `*` indicates interaction between two variables
    * intuitively, the xyplot displays a graph between x and y for every level of `f` and `g`
  * `data` = the data frame/list from which the variables should be looked up
    * if nothing is passed, the parent frame is used \(searching for variables in the workspace\)
    * if no other arguments are passed, defaults will be used
  * `layout` = specifies how the different plots will appear
    * `layout = c(5, 1)` = produces 5 subplots in a horizontal fashion
    * padding/spacing/margin automatically set
  * \[optional\] `panel` function can be added to control what is plotted inside each panel of the plot
    * `panel` functions receive x/y coordinates of the data points in their panel \(along with any additional arguments\)
    * `?panel.xyplot` = brings up documentation for the panel functions
    * **\*Note**: no base plot functions can be used for lattice plots \*

### `lattice` Example

\`\`\`{r fig.height = 4, fig.width = 6, fig.align='center'}
library\(lattice\)
set.seed\(10\)
x &lt;- rnorm\(100\)
f &lt;- rep\(0:1, each = 50\)
y &lt;- x + f - f \* x+ rnorm\(100, sd = 0.5\)
f &lt;- factor\(f, labels = c\("Group 1", "Group 2"\)\)

## Plot with 2 panels with custom panel function

xyplot\(y ~ x \| f, panel = function\(x, y, ...\) {

```
# call the default panel function for xyplot
panel.xyplot(x, y, ...)
# adds a horizontal line at the median
panel.abline(h = median(y), lty = 2)
# overlays a simple linear regression line
panel.lmline(x, y, col = 2)
```

}\)
\`\`\`

```
> library(lattice)
> library(datasets)
> head(airquality)
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
> xyplot(Ozone ~ Wind, data = airquality)
```

![](/assets/airquality.png width="400")



```
> ## convert month to a factor variable
> airquality <- transform(airquality, Month = factor(Month))
> head(airquality)
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
> xyplot(Ozone ~ Wind | Month, data = airquality, layout = c(5, 1))
```

![](/assets/airquality_transform.png)
