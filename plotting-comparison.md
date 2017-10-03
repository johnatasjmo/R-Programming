## Base

1. “Artist’s palette” model
2. Start with blank canvas and build up from there
3. Start with plot function \(or similar\)
4. Use annotation functions to add/modify \(text, lines, points, axis\)

Pros:

Convenient, mirrors how we think of building plots and analyzing data

Cons:

1. Can’t go back once plot has started \(i.e. to adjust margins\);
2. need to plan in advance
3. Difficult to “translate” to others once a new plot has been created \(no graphical “language”\). Plot is just a series of R commands

## Lattice

Plots are created with a single function call \(xyplot, bwplot, etc.\)

Pros:

1. Most useful for conditioning types of plots: Looking at how y changes with x across levels of z
2. Thinks like margins/spacing set automatically because entire plot is specified at once
3. Good for putting many many plots on a screen

Cons:

1. Sometimes awkward to specify an entire plot in a single function call
2. Annotation in plot is not intuitive
3. Use of panel functions and subscripts difficult to wield and requires intense preparation
4. Cannot “add” to the plot once it’s created

## ggplot2

Pros:

1. Split the difference between base and lattice
2. Automatically deals with spacing, text, titles but also allows you to annotate by “adding”
3. Superficial similarity to lattice but generally easier/more intuitive to use
4. Default mode makes many choices for you \(but you can customize!\)

Resources

[https://learnr.wordpress.com/2009/08/26/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-final-part/](https://learnr.wordpress.com/2009/08/26/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-final-part/)

[https://learnr.wordpress.com/2009/07/20/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-part-6/](https://learnr.wordpress.com/2009/07/20/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-part-6/)

[https://learnr.wordpress.com/2009/06/28/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-part-1/](https://learnr.wordpress.com/2009/06/28/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-part-1/)

[https://learnr.wordpress.com/2009/06/29/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-part-2/](https://learnr.wordpress.com/2009/06/29/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-part-2/)

[https://www.r-bloggers.com/lattice-when-modeling-ggplot-when-publishing/](https://www.r-bloggers.com/lattice-when-modeling-ggplot-when-publishing/)

[https://flowingdata.com/2016/03/22/comparing-ggplot2-and-r-base-graphics/](https://flowingdata.com/2016/03/22/comparing-ggplot2-and-r-base-graphics/)

[Jeff Leek - Why I dont use ggplot2](https://simplystatistics.org/2016/02/11/why-i-dont-use-ggplot2/)

[Minimal Line Plot](http://motioninsocial.com/tufte/#minimal-line-plot)

[Head to head comparison ggplot2 and lattice](https://www.stat.ubc.ca/~jenny/STAT545A/block18_gapminderGgplot2VsLattice.html)

[Variance Explained - Why I use ggplot2](http://varianceexplained.org/r/why-I-use-ggplot2/)

[Time Series in Plot](http://www.fromthebottomoftheheap.net/2013/10/23/time-series-plots-with-lattice-and-ggplot/)

