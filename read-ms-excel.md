# Read MS Excel

`excel_sheets() ` list sheets

`read_excel()  i`mports data

**Install and load**

```
install.packages("readxls")
library(readxl)
```

##### Reading Excel file

```
#Load second sheet
read_excel("cities.xlsx", sheet = 2
read_excel("cities.xlsx", sheet = "year_2000")

# Print out the names of both spreadsheets
excel_sheets("urbanpop.xlsx")

```

```
cameraData<-read.xlsx("./data/cameras.xlsx",sheetIndex=1,header=TRUE)
head(cameraData)

## Reading specific rows and columns
colIndex<-2:3
rowIndex<-1:4
cameraDataSubset<-read.xlsx("./data/cameras.xlsx",sheetIndex=1,colIndex=colIndex,rowIndex=rowIndex)
cameraDataSubset
```

##### 

```
# The readxl package is already loaded
# Read the sheets, one by one
pop_1 <- read_excel("urbanpop.xlsx", sheet = 1)
pop_2 <- read_excel("urbanpop.xlsx", sheet = 2)
pop_3 <- read_excel("urbanpop.xlsx", sheet = 3)

# Put pop_1, pop_2 and pop_3 in a list: pop_list
pop_list <- list(pop_1, pop_2, pop_3)

# Display the structure of pop_list
str(pop_list)
```



