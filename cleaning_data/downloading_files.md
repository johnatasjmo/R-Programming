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

List files in directory

```
list.files()
```

Importing Data

```
# Option A
pools <- read.csv("swimming_pools.csv", stringsAsFactors = TRUE)

# Option B
pools <- read.csv("swimming_pools.csv", stringsAsFactors = FALSE)
```

Path

```
# Path to the hotdogs.txt file: path
path <- file.path("data", "hotdogs.txt")

#Path to the hotdogs.txt file inside wd
path <- "hotdogs.txt"

#read table txt
hotdogs <- read.table(path,  sep = "", col.names = c("type", "calories", "sodium"))
```



