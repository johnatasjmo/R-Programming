# Downloading Files



Download data

```
if(!file.exists("data")) {
  dir.create("data")}
fileUrl<-"https://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"
download.file(fileUrl,destfile="./data/cameras.csv",method="curl")
list.files("./data")
dateDownloaded <- date()
```

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

Data Table

```
library(data.table)

DF=data.frame(x=rnorm(9),y=rep(c("a","b","c"),each=3),z=rnorm(9))
head(DF,3)
DT=data.table(x=rnorm(9),y=rep(c("a","b","c"),each=3),z=rnorm(9))head(DT,3)

# See all data tables in Memory
tables()

# Subsetting rows
DT[2,]
DT[DT$y=="a",]  
DT[c(2,3)]  

# Calculating values for variables with expressions
DT[,list(mean(x),sum(z))]  

# Adding new columns w: adds a new value
DT[,w:=z^2]


DT[,m:={tmp<-(x+z); log2(tmp+5)}] # multiple operations

# plyr like operations
DT[,a:=x>0]  # add a variable a that is equals to true if X > 0
DT[,b:=mean(x+w),by=a]  # take the mean of x+w when a = true, write gruped by A

# Special Variable
# .N  An integer, length 1, containing the number of elements of a factor level
set.seed(123);
DT<-data.table(x=sample(letters[1:3],1E5,TRUE))
DT[, .N,by=x]  # count the number of times grouped by the x variable

# Key (rapid) joint
DT<-data.table(x=rep(c("a","b","c"),each=100),y=rnorm(300))
setkey(DT,x)  # set the key to be variable X
DT['a'] # subsets only values = a 

# faster than merging with data frame
DT1 <- data.table(x=c('a', 'a', 'b', 'dt1'), y=1:4)
DT2 <- data.table(x=c('a', 'b', 'dt2'), z=5:7)
setkey(DT1, x); setkey(DT2, x) #set a key on both 
merge(DT1, DT2) # merge both tables


# Fast reading
big_df<-data.frame(x=rnorm(1E6),y=rnorm(1E6)) # create a big data brame 
#set a temporary file
file<-tempfile()
#write
write.table(big_df,file=file,row.names=FALSE,col.names=TRUE,sep="\t",quote=FALSE)
#time on system
system.time(fread(file))
# time with read.table - slower
system.time(read.table(file, header=TRUE, sep="\t"))

```



