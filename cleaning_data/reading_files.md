# Reading Files

### Reading with utils package \(default\)

`read.table`is the default option

`read.csv`reads csv files with header = TRUE and sep ="'," by default

`read.delim`reads data separated by tabs with header = TRUE, sep ="\t"

##### Importing Data

```r
pools <- read.csv("swimming_pools.csv", stringsAsFactors = TRUE)

# Option B
pools <- read.csv("swimming_pools.csv", stringsAsFactors = FALSE)
```

```

```

##### 

##### Path

```r
# Path to the hotdogs.txt file: path
path <- file.path("data", "hotdogs.txt")

#Path to the hotdogs.txt file inside wd
path <- "hotdogs.txt"

#read table txt
hotdogs <- read.table(path,  sep = "",col.names = c("type", "calories", "sodium"))
```

##### 

##### Reading local file

```r
head(cameraData)

cameraData <- read.csv("./data/cameras.csv")
head(cameraData)
```

##### Reading XML and HTML

```r
library(XML)
fileUrl<-"http://www.w3schools.com/xml/simple.xml"
doc<-xmlTreeParse(fileUrl,useInternal=TRUE)
rootNode<-xmlRoot(doc)
xmlName(rootNode)   
names(rootNode)
rootNode[[1]] 
rootNode[[1]][[1]]  
xmlSApply(rootNode,xmlValue)
```

##### XPath

```r
# /nodeTop level node
# //nodeNode at any level
# node[@attr-name]Node with an attribute name
# node[@attr-name='bob']Node with attribute name attr-name='bob'
# Information from:http://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/XML.pdf

xpathSApply(rootNode,"//name",xmlValue)
xpathSApply(rootNode,"//price",xmlValue)
fileUrl <- "http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens"
doc <- htmlTreeParse(fileUrl,useInternal=TRUE)
scores <- xpathSApply(doc,"//li[@class='score']",xmlValue)
teams <- xpathSApply(doc,"//li[@class='team-name']",xmlValue)
scores
```

##### Reading JSON

```r
jsonData<fromJSON("https://api.github.com/users/jtleek/repos")
names(jsonData)
jsonData$name
names(jsonData$owner)
jsonData$owner$login

#Writing data frames to JSON
myjson<-toJSON(iris,pretty=TRUE)
cat(myjson)

#Convert back to JSON
iris2<-fromJSON(myjson)
head(iris2)
```

### Reading with readr package

Best to specify a vector and add it as `col_names`

`coltypes`_specifies the column types \( c = character, d =double, i = integer, l = logical , \_=skip\). Like `col`\_`types ="ccdd"`

##### Load library

```r
library(readr)
```

##### read\_delim usage

```r
# Column Names
properties <- c("area", "temp", "size", "storage", "method",
                "texture", "flavor", "moistness")
# Import potatoes.txt using read_delim(): potatoes
potatoes <- read_delim("potatoes.txt", delim = "\t", col_names = properties)
# Print out potatoes
potatoes
```

##### skip and n\_max

`skip` and `n_max`_for huge data files. _`skip = 2`_, _`n_max = 3`_will  skip two columns and read the next 3 records. However, col\_names must be specified in statement_

```r
# readr is already loaded

# Column names
properties <- c("area", "temp", "size", "storage", "method",
                "texture", "flavor", "moistness")

# Import 5 observations from potatoes.txt: potatoes_fragment
potatoes_fragment <- read_tsv("potatoes.txt", skip = 6, n_max = 5, col_names = properties)
```

##### col\_types

col\_types specifies the column types

```r
# readr is already loaded

# Column names
properties <- c("area", "temp", "size", "storage", "method",
                "texture", "flavor", "moistness")

# Import all data, but force all columns to be character: potatoes_char
potatoes_char <- read_tsv("potatoes.txt", col_types = "cccccccc", col_names = properties)

# Print out structure of potatoes_char
str(potatoes_char)
```

col_factor and col\_integer_

```r
# readr is already loaded

# Import without col_types
hotdogs <- read_tsv("hotdogs.txt", col_names = c("type", "calories", "sodium"))

# Display the summary of hotdogs
summary(hotdogs)


# The collectors you will need to import the data
fac <- col_factor(levels = c("Beef", "Meat", "Poultry"))
int <- col_integer()

# Edit the col_types argument to import the data correctly: hotdogs_factor
hotdogs_factor <- read_tsv("hotdogs.txt",
                           col_names = c("type", "calories", "sodium"),
                           col_types = list(fac, int, int))

# Display the summary of hotdogs_factor
summary(hotdogs_factor)
```

### Reading with data.table

fread\(\) automatically handles names, types, separators and its fast

Other factors can manually added

Load package

```r
library(data.table)
```

Import file

```r
# Import potatoes.csv with fread(): potatoes
potatoes <- fread("potatoes.csv")

# Print out potatoes
potatoes
```

Select

```r
# fread is already loaded

# Import columns 6 and 8 of potatoes.csv: potatoes
potatoes <- fread("potatoes.csv", select = c(6,8))

# Plot texture (x) and moistness (y) of potatoes
plot(x = potatoes$texture, y = potatoes$moistness)
```



