# Data Table package

Faster than data frames

```
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



