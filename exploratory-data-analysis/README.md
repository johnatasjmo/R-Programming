# Principle of Analytic Graphics

- **Principle 1: Show Comparisons**

  - always comparative (compared to what)
  - randomized trial - compare control group to test group
  - evidence for a hypothesis is always relative to another competing hypothesis

- **Principle 2: Show causality/mechanism/explanation/systematic structure**

  - form hypothesis to evidence showing a relationship (causal framework, why something happened)

- **Principle 3: Show multivariate data**

  - more than 2 variables because the real world is multivariate
  - show as much data on a plot as you can


```{r fig.height = 3, fig.width = 4, fig.align='center', echo=FALSE}

# install grid and png packages if not present

library(png) library(grid) grid.raster(readPNG("figures/1.jpg"))

```

slightly negative relationship between pollution and mortality when split up by season, the relationships are all positive $\rightarrow$ season = confounding variable


```{r fig.height = 4, fig.width = 6, fig.align='center', echo=FALSE}
grid.raster(readPNG("figures/2.jpg"))
```

- **Principle 4: Integration of evidence**

  - use as many modes of evidence/displaying evidence as possible (modes of data presentation)
  - integrate words/numbers/images/diagrams (information rich)
  - analysis should drive the tool

- **Principle 5: Describe/document evidence with appropriate labels/scales/sources**

  - add credibility to that data graphic

- **Principle 6: Content is the most important**

  - analytical presentations ultimately stand/fall depending on quality/relevance/integrity of content

## Exploratory Graphs ([examples](http://gallery.r-enthusiasts.com/))

- **Purpose**: understand data properties, find pattern in data, suggest modeling strategies, debug
- **Characteristics**: made quickly, large number produced, gain personal understanding, appearances and presentation are aren't as important

### One Dimension Summary of Data

- `summary(data)` = returns min, 1st quartile, median, mean, 3rd quartile, max
- `boxplot(data, col = “blue”)` = produces a box with middles 50% highlighted in the specified color

  - `whiskers` = $\pm 1.58IQR/\sqrt{n}$

    - IQR = interquartile range, Q$_3$ - Q$_1$

  - `box` = 25%, median, 75%

- `histograms(data, col = “green”)` = produces a histogram with specified breaks and color

  - `breaks = 100` = the higher the number is the smaller/narrower the histogram columns are

- `rug(data)` = density plot, add a strip under the histogram indicating location of each data point

- `barplot(data, col = wheat)` = produces a bar graph, usually for categorical data

- **Overlaying Features**

- `abline(h/v = 12)` = overlays horizontal/vertical line at specified location

  - `col = “red”` = specifies color
  - `lwd = 4` = line width
  - `lty = 2` = line type

### Two Dimensional Summaries

- multiple/overlay 1D plots (using lattice/ggplot2)
- **box plots**: `boxplot(pm25 ~ region, data = pollution, col = “red”)`

```{r fig.height = 3, fig.width = 4, fig.align='center', echo=FALSE} grid.raster(readPNG("figures/3.jpg"))

````

* **histogram**:
    * `par(mfrow = c(2, 1), mar = c(4, 4, 2, 1))` = set margin
    * `hist(subset(pollution, region == "east")$pm25, col = "green")` = first histogram
    * `hist(subset(pollution, region == "west")$pm25, col = "green")` = second histogram

```{r fig.height = 3, fig.width = 4, fig.align='center', echo=FALSE}
grid.raster(readPNG("figures/4.jpg"))
````

- **scatterplot**

  - `with(pollution, plot(latitude, pm25, col = region))`
  - `abline(h = 12, lwd = 2, lty = 2)` = plots horizontal dotted line
  - `plot(jitter(child, 4)~parent, galton)` = spreads out data points at the same position to simulate measurement error/make high frequency more visibble

```{r fig.height = 3, fig.width = 4, message = F, warning = F, fig.align='center', echo=FALSE} grid.raster(readPNG("figures/5.jpg"))

````

```{r fig.height = 3, fig.width = 4, fig.align='center', message = F, warning = F, echo=FALSE}
library(UsingR); data(galton)
plot(jitter(child, 4)~parent, galton)
````

- **multiple scatter plots**

  - `par(mfrow = c(1, 2), mar = c(5, 4, 2, 1))` = sets margins
  - `with(subset(pollution, region == "west"), plot(latitude, pm25, main = "West"))` = left scatterplot
  - `with(subset(pollution, region == "east"), plot(latitude, pm25, main = "East"))` = right scatterplot

```{r fig.height = 4, fig.width = 6, fig.align='center', echo=FALSE} grid.raster(readPNG("figures/6.jpg"))

```

### Process of Making a Plot/Considerations
* where will plot be made? screen or file?
* how will plot be used? viewing on screen/web browser/print/presentation?
* large amount of data vs few points?
* need to be able to dynamically resize?
* **plotting system**: base, lattice, ggplot2?


## Base Plotting
* blank canvas, “artist’s palette”, start with plot function
* annotations - text, lines, points, axis
* convenient, but cannot go back when started (need to plan ahead)
* everything need to be manually set carefully to be able to achieve the desired effect (margins)
* core plotting/graphics engine in R encapsulated in the following
    * ***graphics***: plotting functions for vase graphing system (plot, hist, boxplot, text)
    * ***grDevices***: contains all the code implementing the various graphics devices (x11, PDF, PostScript, PNG, etc)

* ***Two phase***: initialize, annotate
* calling `plot(x, y)` or `hist(x)` will launch a graphics device and draw a plot on device
    * if no argument specified, default called
    * parameters documented in “`?par`"
    * ***Note**: it is some times necessary to convert column/variable to factor to make plotting easier *
        * `airquality <- transform(airquality, Month = factor(month))`

### Base Graphics Functions and Parameters
* **arguments**
    * `pch`: plotting symbol (default = open circle)
    * `lty`: line type (default is solid)
        * 0=blank, 1=solid (default), 2=dashed, 3=dotted, 4=dotdash, 5=longdash, 6=twodash
    * `lwd`: line width (integer)
    * `col`: plotting color (number string or hexcode, colors() returns vector of colors)
    * `xlab`, `ylab`: x-y label character strings
    * `cex`: numerical value giving the amount by which plotting text/symbols should be magnified relative to the default
        - `cex = 0.15 * variable`: plot size as an additional variable
* `par()` function = specifies global graphics parameters, affects all plots in an R session (can be overridden)
    * `las`: orientation of axis labels
    * `bg`: background color
    * `mar`: margin size (order = bottom left top right)
    * `oma`: outer margin size (default = 0 for all sides)
    * `mfrow`: number of plots per row, column (plots are filled row-wise)
    * `mfcol`: number of plots per row, column (plots are filled column-wise)
    * can verify all above parameters by calling `par("parameter")`
* **plotting functions**
    * `lines`: adds liens to a plot, given a vector of x values and corresponding vector of y values
    * `points`: adds a point to the plot
    * `text`: add text labels to a plot using specified x,y coordinates
    * `title`: add annotations to x,y axis labels, title, subtitles, outer margin
    * `mtext`: add arbitrary text to margins (inner or outer) of plot
    * `axis`: specify axis ticks

### Base Plot Example
```{r fig.height = 4, fig.width = 6, fig.align='center'}
library(datasets)
# type =“n” sets up the plot and does not fill it with data
with(airquality, plot(Wind, Ozone, main = "Ozone and Wind in New York City", type = "n"))
# subsets of data are plotted here using different colors
with(subset(airquality, Month == 5), points(Wind, Ozone, col = "blue"))
with(subset(airquality, Month != 5), points(Wind, Ozone, col = "red"))
legend("topright", pch = 1, col = c("blue", "red"), legend = c("May", "Other Months"))
model <- lm(Ozone ~ Wind, airquality)
# regression line is produced here
abline(model, lwd = 2)
```

### Multiple Plot Example

- ***Note**: typing `example(points)` in R will launch a demo of base plotting system and may provide some helpful tips on graphing *

```{r fig.height = 4, fig.width = 6, fig.align='center'}

# this expression sets up a plot with 1 row 3 columns, sets the margin and outer margins

par(mfrow = c(1, 3), mar = c(4, 4, 2, 1), oma = c(0, 0, 2, 0)) with(airquality, {

```
# here three plots are filled in with their respective titles
plot(Wind, Ozone, main = "Ozone and Wind")
plot(Solar.R, Ozone, main = "Ozone and Solar Radiation")
plot(Temp, Ozone, main = "Ozone and Temperature")
# this adds a line of text in the outer margin*
mtext("Ozone and Weather in New York City", outer = TRUE)}
)

```


$\pagebreak$

## Graphics Device
* A graphics device is something where you can make a plot appear
    * **window on screen** (screen device) = quick visualizations and exploratory analysis
    * **pdf** (file device) = plots that may be printed out or incorporated in to document
    * **PNG/JPEG** (file device) = plots that may be printed out or incorporated in to document
    * **scalable vector graphics** (SVG)
* When a plot is created in R, it has to be sent to a graphics device
* **Most common is screen device**
    * `quartz()` on Mac, `windows()` on Windows, `x11()` on Unix/Linux
    * `?Devices` = lists devices found
* **Plot creation**
    * screen device
        * call plot/xplot/qplot $\rightarrow$ plot appears on screen device $\rightarrow$ annotate as necessary $\rightarrow$ use
    * file devices
        * explicitly call graphics device $\rightarrow$ plotting function to make plot (write to file) $\rightarrow$ annotate as necessary $\rightarrow$ explicitly close graphics device with `dev.off()`
* **Graphics File Devices**
    * ***Vector Formats*** (good for line drawings/plots w/ solid colors, a modest number of points)
        * `pdf`: useful for line type graphics, resizes well, usually portable, not efficient if too many points
        * `svg`: XML based scalable vector graphics, support animation and interactivity, web based
        * `win.metafile`: Windows metafile format
        * `postscript`: older format, resizes well, usually portable, can create encapsulated postscript file, Windows often don’t have postscript viewer (postscript = predecessor of PDF)
    * ***Bitmap Formats*** (good for plots w/ large number of points, natural scenes/webbased plots)
        * `png`: Portable Network Graphics, good for line drawings/image with solid colors, uses lossless compression, most web browsers read this natively, good for plotting a lot of data points, does not resize well
        * `JPEG`: good for photographs/natural scenes/gradient colors, size efficient, uses lossy compression, good for plotting many points, does not resize well, can be read by almost any computer/browser, not great for line drawings (aliasing on edges)
        * `tiff`: common bitmap format supports lossless compression
        * `bmp`: native Windows bitmapped format
* **Multiple Open Graphics Devices**
    * possible to open multiple graphics devices (screen, file, or both)
    * plotting occurs only one device at a time
    * `dev.cur()` = returns the currently active device
    * every open graphics device is assigned an integer >= 2
    * `dev.set(<integer>)` = change the active graphics device
        `<integer>` = number associated with the graphics device you want to switch to
* **Copying plots**
    * `dev.copy()` = copy a plot from one device to another
    * `dev.copy2pdf()` = specifically for copying to PDF files
    * ***Note**: copying a plot is not an exact operation, so the result may not be identical to the original *
    * ***example***

```{r fig.height = 3, fig.width = 4, fig.align='center', results = 'hide'}
## Create plot on screen device
with(faithful, plot(eruptions, waiting))
## Add a main title
title(main = "Old Faithful Geyser data")
## Copy my plot to a PNG file
dev.copy(png, file = "geyserplot.png")
## Don't forget to close the PNG device!
dev.off()
````
