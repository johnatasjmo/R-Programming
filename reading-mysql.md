# Reading MySQL

### Install

```
install.packages("RMySQL")
library("RMySQL")
```

##### Connect and list

```
ucscDb <- dbConnect(MySQL(), user="genome", host="genome-mysql.cse.ucsc.edu")
result <- dbGetQuery(ucscDb,"show databases;")
dbDisconnect(ucscDb); #result should be TRUE
result

hg19 <-dbConnect(MySQL(), user="genome", db="hg19", host="genome-mysql.cse.ucsc.edu")
allTables <- dbListTables(hg19)
length(allTables)
allTables[1:5]

dbListFields(hg19,"affyU133Plus2")
dbGetQuery(hg19, "select count(*) from affyU133Plus2")
```

```
> dbListFields(hg19,"affyU133Plus2")
 [1] "bin"         "matches"     "misMatches" 
 [4] "repMatches"  "nCount"      "qNumInsert" 
 [7] "qBaseInsert" "tNumInsert"  "tBaseInsert"
[10] "strand"      "qName"       "qSize"      
[13] "qStart"      "qEnd"        "tName"      
[16] "tSize"       "tStart"      "tEnd"       
[19] "blockCount"  "blockSizes"  "qStarts"    
[22] "tStarts"
```

```
> dbGetQuery(hg19, "select count(*) from affyU133Plus2")
  count(*)
1    58463
```

```
> query <- dbSendQuery(hg19, "select * from affyU133Plus2 where misMatches between 1 and 3")
There were 16 warnings (use warnings() to see them)
> affyMis <- fetch(query); quantile(affyMis$misMatches)
  0%  25%  50%  75% 100% 
   1    1    2    2    3 


> affyMisSmall <- fetch(query, n=10); dbClearResult(query); # clear the query results
[1] TRUE

> dim(affyMisSmall)
[1] 10 22
```

### Resources

[https://www.pantz.org/software/mysql/mysqlcommands.html](https://www.pantz.org/software/mysql/mysqlcommands.html)

[https://www.r-bloggers.com/mysql-and-r/](https://www.r-bloggers.com/mysql-and-r/)

