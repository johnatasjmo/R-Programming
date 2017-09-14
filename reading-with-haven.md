Reading with haven

Packages like:

* SAS: statistical, business analytics
* STATA: economist
* SPSS: social sciences

haven can deal with SAS, STATA and SPSS

Result: Rdataframe

```
install.package("haven")
library(haven)
```

`read_sas()` reads files and as a result has also labels inside. 

`read_stata()`_, `read_dta`_`()` reads STATA file. Converts numbers

`as_`_`factor()` will convert as factor some values i.e. `as_factor(ontime$Airline)`_`as.character(as_factor(ontime$Airline)`

SPSS

read\_spss\(\)

read\_por\(\)

read\_sav\(\)

![](/assets/Screen Shot 2017-09-14 at 9.28.54 AM.png)



