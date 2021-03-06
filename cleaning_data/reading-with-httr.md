# Reading with API

### jsonlite package

```r
install.packages("jsonlite")
library(jsonlite)
```

```r
> # Load the jsonlite package
> library(jsonlite)
> 
> # wine_json is a JSON
> wine_json <- '{"name":"Chateau Migraine", "year":1997, "alcohol_pct":12.4, "color":"red", "awarded":false}'
> 
> # Convert wine_json into a list: wine
> wine <- fromJSON(wine_json)
> 
> # Print structure of wine
> str(wine)
List of 5
 $ name       : chr "Chateau Migraine"
 $ year       : int 1997
 $ alcohol_pct: num 12.4
 $ color      : chr "red"
 $ awarded    : logi FALSE
>
```

```r
> # jsonlite is preloaded
> 
> # Definition of quandl_url
> quandl_url <- "https://www.quandl.com/api/v3/datasets/WIKI/FB/data.json?auth_token=i83asDsiWUUyfoypkgMz"
> 
> # Import Quandl data: quandl_data
> quandl_data <- fromJSON(quandl_url)
> 
> # Print structure of quandl_data
> str(quandl_data)
List of 1
 $ dataset_data:List of 10
  ..$ limit       : NULL
  ..$ transform   : NULL
  ..$ column_index: NULL
  ..$ column_names: chr [1:13] "Date" "Open" "High" "Low" ...
  ..$ start_date  : chr "2012-05-18"
  ..$ end_date    : chr "2017-09-08"
  ..$ frequency   : chr "daily"
  ..$ data        : chr [1:1336, 1:13] "2017-09-08" "2017-09-07" "2017-09-06" "2017-09-05" ...
  ..$ collapse    : NULL
  ..$ order       : NULL
>
```

```r
> # The package jsonlite is already loaded
> 
> # Definition of the URLs
> url_sw4 <- "http://www.omdbapi.com/?apikey=ff21610b&i=tt0076759&r=json"
> url_sw3 <- "http://www.omdbapi.com/?apikey=ff21610b&i=tt0121766&r=json"
> 
> # Import two URLs with fromJSON(): sw4 and sw3
> sw4 <- fromJSON(url_sw4)
> sw3 <- fromJSON(url_sw3)
> 
> # Print out the Title element of both lists
> sw4$Title
[1] "Star Wars: Episode IV - A New Hope"
> sw3$Title
[1] "Star Wars: Episode III - Revenge of the Sith"
> 
> # Is the release year of sw4 later than sw3?
> sw4$Year > sw3$Year
[1] FALSE
```

### JSON Objects

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

##### fromJSON\(\) and toJSON\(\)

```r
# jsonlite is already loaded

# Challenge 1
json1 <- '[1, 2, 3, 4, 5, 6]'
fromJSON(json1)

# Challenge 2
json2 <- '{"a": [1, 2, 3], "b":[4,5,6]}'
fromJSON(json2)

# jsonlite is already loaded

# Challenge 1
json1 <- '[[1, 2], [3, 4], [5, 6]]'
fromJSON(json1)

# Challenge 2
json2 <- '[{"a": 1, "b": 2}, {"a": 3, "b": 4}, {"a": 5, "b": 6}]'
fromJSON(json2)
```

```r
# jsonlite is already loaded

# URL pointing to the .csv file
url_csv <- "http://s3.amazonaws.com/assets.datacamp.com/production/course_1478/datasets/water.csv"

# Import the .csv file located at url_csv
water <- read.csv(url_csv, stringsAsFactors = FALSE)

# Convert the data file according to the requirements
water_json <- toJSON(water)

# Print out water_json
water_json
```

##### pretty json and minify\(\)

```r
# jsonlite is already loaded

# Convert mtcars to a pretty JSON: pretty_json
pretty_json <- toJSON(mtcars, pretty = TRUE)

# Print pretty_json
pretty_json

# Minify pretty_json: mini_json
mini_json <- minify(pretty_json)

# Print mini_json
mini_json
```

### Github API

```r
library(httr)

# 1. Find OAuth settings for github:
#    http://developer.github.com/v3/oauth/
oauth_endpoints("github")

# 2. To make your own application, register at 
#    https://github.com/settings/developers. Use any URL for the homepage URL
#    (http://github.com is fine) and  http://localhost:1410 as the callback url
#
#    Replace your key and secret below.
myapp <- oauth_app("github",
  key = "MYKEY",
  secret = "MYSECRET")

# 3. Get OAuth credentials
github_token <- oauth2.0_token(oauth_endpoints("github"), myapp)

# 4. Use API
gtoken <- config(token = github_token)
req <- GET("https://api.github.com/rate_limit", gtoken)
stop_for_status(req)
content(req)

# OR:
req <- with_config(gtoken, GET("https://api.github.com/rate_limit"))
stop_for_status(req)
content(req)
```

### 

### Other examples of API connect

### [r-lib/httr/demo](https://github.com/r-lib/httr/tree/master/demo)

[r-lib/httr](https://github.com/r-lib/httr)

