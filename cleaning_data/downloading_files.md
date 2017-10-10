# Downloading Files

Download data

```r
if(!file.exists("data")) {
  dir.create("data")}
fileUrl<-"https://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"
download.file(fileUrl,destfile="./data/cameras.csv",method="curl")
list.files("./data")
dateDownloaded <- date()
```

List files in directory

```r
list.files()
```

if folder exists, delete and create a new one \(clean it

if folder exist, do not create one



