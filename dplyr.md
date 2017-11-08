dplyr package

tibble is special type of dataframe

package hflights

glimpse\(hflights\) uses more information to display

```r
two <- c("AA", "AS")
lut <- c("AA" = "American", 
         "AS" = "Alaska", 
         "B6" = "JetBlue")
two <- lut[two]
two
```

`select` removes columns and returns a modified copy

`filter` removes rows

`arranges` reorders

`mutate` changes values

`summarise` provides statistics

Observations in columns, variables in columns. 

Resources tidyr data package

Select

![](/assets/Screen Shot 2017-11-08 at 8.22.38 AM.png)

```r
library(dplyr)
library(hflights)

# Convert the hflights data.frame into a hflights tbl
hflights <- tbl_df(hflights)
# Display the hflights tbl
hflights
# Create the object carriers
carriers <- hflights$UniqueCarrier

# Both the dplyr and hflights packages are loaded into workspace
lut <- c("AA" = "American", "AS" = "Alaska", "B6" = "JetBlue", "CO" = "Continental", 
         "DL" = "Delta", "OO" = "SkyWest", "UA" = "United", "US" = "US_Airways", 
         "WN" = "Southwest", "EV" = "Atlantic_Southeast", "F9" = "Frontier", 
         "FL" = "AirTran", "MQ" = "American_Eagle", "XE" = "ExpressJet", "YV" = "Mesa")
lut
# Add the Carrier column to hflights
hflights$Carrier <- lut[hflights$UniqueCarrier]
# Glimpse at hflights
glimpse(hflights)

# shows unique cancelations codes
unique(hflights$CancellationCode)
# The lookup table created based on cancellation codes
lut <- c("A" = "carrier", "B" = "weather", "C" = "FFA", "D" = "security", "E" = "not cancelled")
# Add the Code column
hflights$Code <- lut[hflights$CancellationCode]
# Glimpse at hflights
glimpse(hflights)
```





