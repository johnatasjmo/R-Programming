# Reading with API

### APIs & JSON

jsonlite package

```
install.packages("jsonlite")
library(jsonlite)
```

```
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

```R
> # jsonlite is preloaded
> 
> # Definition of quandl_url
> quandl_url <- "https://www.quandl.com/api/v3/datasets/WIKI/FB/data.json?auth_token=i83asDsiWUUyfoypkgMz"
> 
> # Import Quandl data: quandl_data
> quandl_data <- fromJSON(quandl_url)
> 
> # Print structure of quandl_data
> str(quandl_url)
 chr "https://www.quandl.com/api/v3/datasets/WIKI/FB/data.json?auth_token=i83asDsiWUUyfoypkgMz"
> 
```

### Github API

```
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

### Other examples of API connect

### [r-lib/httr/demo](https://github.com/r-lib/httr/tree/master/demo)

[r-lib/httr](https://github.com/r-lib/httr)

