# Reading MySQL

### Install

```r
install.packages("RMySQL")
library("RMySQL")
```

##### Connect and list

```r
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

```r
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

```r
# select all from affy... table where misMatches variable is between 1 and 3, then fetch the query per quartile
> query <- dbSendQuery(hg19, "select * from affyU133Plus2 where misMatches between 1 and 3")
There were 16 warnings (use warnings() to see them)
> affyMis <- fetch(query); quantile(affyMis$misMatches)
  0%  25%  50%  75% 100% 
   1    1    2    2    3 

# fetch the top 10 record and clear the query results
> affyMisSmall <- fetch(query, n=10); dbClearResult(query); 
[1] TRUE

> dim(affyMisSmall)
[1] 10 22
```

```
# Load the DBI package
library(DBI)

# Edit dbConnect() call
con <- dbConnect(RMySQL::MySQL(), 
                 dbname = "tweater", 
                 host = "courses.csrrinzqubik.us-east-1.rds.amazonaws.com", 
                 port = 3306,
                 user = "student",
                 password = "datacamp")

# Build a vector of table names: tables
tables <- dbListTables(con)

# Display structure of tables
str(tables)

# Import the users table from tweater: users
users <- dbReadTable(con, "users")

# Print users
users

# Import tweat_id column of comments where user_id is 1: elisabeth
elisabeth <- dbGetQuery(con, "SELECT tweat_id FROM comments WHERE user_id = '1'")

# Print elisabeth
elisabeth

# Import post column of tweats where date is higher than '2015-09-21': latest
latest <- data.frame(dbGetQuery(con, "SELECT post FROM tweats WHERE date > '2015-09-21'"))

# Print latest
latest

# Create data frame specific
specific <- dbGetQuery(con, "SELECT message FROM comments WHERE tweat_id = '77' AND user_id > 4")

# Print specific
specific

# Create data frame short, select id and name where char length of name is less than 5
short <- data.frame(dbGetQuery(con, "SELECT id, name FROM users WHERE CHAR_LENGTH(name) < 5"))

# Print short
short

# INNER JOIN can also be used, this command will join two tables
guess <- data.frame(dbGetQuery(con, "SELECT post, message from tweats INNER JOIN comments on tweats.id = tweat_id WHERE tweat_id = 77")
guess
```

DBI internals

```
# Send query to the database
res <- dbSendQuery(con, "SELECT * FROM comments WHERE user_id > 4")

# Use dbFetch() twice
dbFetch(res, n = 2)
dbFetch(res)


# Clear res
dbClearResult(res)

# Create the data frame  long_tweats
long_tweats <- data.frame(dbGetQuery(con, "SELECT post, date FROM tweats WHERE char_length(post) > 40"))

# Print long_tweats
print(long_tweats)

# Disconnect from the database
dbDisconnect(con)


```



### Resources

[https://www.pantz.org/software/mysql/mysqlcommands.html](https://www.pantz.org/software/mysql/mysqlcommands.html)

[https://www.r-bloggers.com/mysql-and-r/](https://www.r-bloggers.com/mysql-and-r/)

