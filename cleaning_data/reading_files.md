# Reading Files





Reading local file

```
cameraData<-read.table("./data/cameras.csv",sep=",",header=TRUE)
head(cameraData)

cameraData <- read.csv("./data/cameras.csv")
head(cameraData)
```

Reading Excel file

```
library(xlsx)
cameraData<-read.xlsx("./data/cameras.xlsx",sheetIndex=1,header=TRUE)
head(cameraData)

## Reading specific rows and columns
colIndex<-2:3
rowIndex<-1:4
cameraDataSubset<-read.xlsx("./data/cameras.xlsx",sheetIndex=1,colIndex=colIndex,rowIndex=rowIndex)
cameraDataSubset
```

## Reading XML and HTML

```
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

XPath

```
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

Reading JSON

```
library(jsonlite)
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



