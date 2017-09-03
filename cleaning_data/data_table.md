# Data Table package

Faster than data frames

```r
#load
library(data.table)

# Data frame
DF=data.frame(x=rnorm(9),y=rep(c("a","b","c"),each=3),z=rnorm(9))
head(DF,3)

# Data table
DT=data.table(x=rnorm(9),y=rep(c("a","b","c"),each=3),z=rnorm(9))head(DT,3)

# See all data tables in memory
tables()

# Subsetting rows
DT[2,]   # first spot
DT[DT$y=="a",]   # y values equal to a
DT[c(2,3)]  # subset only based on rows!

# Calculating values for variables with expressions. Passing a list of functions like colums
# no need to use parenthesis
DT[,list(mean(x),sum(z))]  
DT[,table(y]  # all data y

# Adding new columns w: adds a new value z
DT[,w:=z^2]

# data table can be copied

# Expression assign to the temp variable x+z and then take the log2 and add 5
DT[,m:={tmp<-(x+z); log2(tmp+5)}] # multiple operations

# plyr like operations
DT[,a:=x>0]  # add a variable a that is equals to true if X > 0
DT[,b:=mean(x+w),by=a]  # take the mean of x+w when a = true, write gruped by A

# Special Variable
# .N  An integer, length 1, containing the number of elements of a factor level
set.seed(123);
DT<-data.table(x=sample(letters[1:3],1E5,TRUE)) # create a large numbers of letters
DT[, .N,by=x]  # .N counts the number of times grouped by the x variable 

# Key (rapid) joint
DT<-data.table(x=rep(c("a","b","c"),each=100),y=rnorm(300))
setkey(DT,x)  # set the key to be variable X  
DT['a'] # subsets only values x = a 

# faster than merging with data frame
DT1 <- data.table(x=c('a', 'a', 'b', 'dt1'), y=1:4) #creating table DT1
DT2 <- data.table(x=c('a', 'b', 'dt2'), z=5:7) # creating table DT2
setkey(DT1, x); setkey(DT2, x) #set a key on both equals to x
merge(DT1, DT2) # merge both tables


# Fast reading
big_df<-data.frame(x=rnorm(1E6),y=rnorm(1E6)) # create a big data brame 
#set a temporary file
file<-tempfile()
#write
write.table(big_df,file=file,row.names=FALSE,col.names=TRUE,sep="\t",quote=FALSE)
#time on system  with read 
system.time(fread(file))
# time with read.table - slower
system.time(read.table(file, header=TRUE, sep="\t"))
```

Example with data table

```
library(data.table) 
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
download.file(fileUrl, destfile = "america_comm_survey.csv")
dateDownloaded <- date()
data <- read.csv("america_comm_survey.csv")
head(data$VAL) # take a look at the contents of the VAL (property worth) variable
```

```
> head(data$VAL)
[1] 17 NA 18 19 20 15
```

```
DT = data.table(data)
DT[, .N, by=VAL==24]
```

```
> DT[, .N, by=VAL==24] 
     VAL    N
1: FALSE 4367
2:    NA 2076
3:  TRUE   53
```

```
# ANSWER = 53 AT $1m+ (NUMBER 24)
head(DT$FES, 20)
```

Other example

```
#install.packages("XML")
```

```
library
(
XML
)
library
(
RCurl
)
library
(
dplyr
)
fileXML
<
-
"https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml"
# had to remove s from https in above xml file. This method commented below:
#   fileXML 
<
- "http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml"
#   doc 
<
- xmlTreeParse(fileXML,useInternal = TRUE)
# using RCurl, can leave https.  Use getURL first, then parse with xmlParse
xData
<
-
getURL
(
fileXML
)
# This allows you to use https
doc
<
-
xmlParse
(
xData
)
rootNode
<
-
xmlRoot
(
doc
)
#xmlName(rootNode) # just displays top root node name
# one version, no data frame required - no need for zips, zips_dt 
sum
(
xpathSApply
(
rootNode
, 
"//zipcode"
, 
xmlValue
)
==
"21231"
)
```

```
## [1] 127
```

```
# another version, create data frame and find answer there
zips
<
-
xpathSApply
(
rootNode
,
"//zipcode"
, 
xmlValue
)
# getting the zip code data
zips_dt
<
-
data.frame
(
zips
, 
row.names
=
NULL
)
# creating a data frame from them
summary
(
zips_dt
$
zips
==
21231
)
# find count of 21231
```

```
##    Mode   FALSE    TRUE    NA's 
## logical    1200     127       0
```

```
# another method, finds count of True instance of 21231. Uses dplyr.
count
(
zips_dt
, 
zips
==
21231
)
```

```
## Source: local data frame [2 x 2]
## 
##   zips == 21231     n
##           (lgl) (int)
## 1         FALSE  1200
## 2          TRUE   127
```

```
# QUESTION 4 ANSWER        
127
```

```
## [1] 127
```



