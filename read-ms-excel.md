# Read MS Excel

`excel_sheets()` list sheets

`read_excel()  i`mports data

`col_names = TRUE`imports the column names from MS Excel

`col_types = NULL`imports no column types and R will deduct

`col_types = c("text", "text")`will import column types as text. Other types are numeric, date and blank. blank wil not import the blank

`skip = 0`needs to be used with col\_names. Then R will skip the two first rows including rows including the name

`n_max` is not available yet. Under development 2 Sep 2017

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

```
# The readxl package is already loaded

# Read all Excel sheets with lapply(): pop_list
pop_list <- lapply(excel_sheets("urbanpop.xlsx"), read_excel, path = "urbanpop.xlsx")

# Display the structure of pop_list
str(pop_list)
```

```
# The readxl package is already loaded

# Import the the first Excel sheet of urbanpop_nonames.xlsx (R gives names): pop_a
pop_a <- read_excel("urbanpop_nonames.xlsx", col_names = FALSE, sheet = 1)

# Import the the first Excel sheet of urbanpop_nonames.xlsx (specify col_names): pop_b
cols <- c("country", paste0("year_", 1960:1966))
pop_b <- read_excel("urbanpop_nonames.xlsx", col_names = cols, sheet = 1)

# Print the summary of pop_a
summary(pop_a)

# Print the summary of pop_b
summary(pop_b)
```

```
# The readxl package is already loaded

# Import the second sheet of urbanpop.xlsx, skipping the first 21 rows: urbanpop_sel
urbanpop_sel <- read_excel("urbanpop.xlsx", col_names = FALSE, skip = 21, sheet = 2)

# Print out the first observation from urbanpop_sel
head(urbanpop_sel, 1)
```

### gdata Package

gdata reads.xls ith Perl and uses read.csv to read table. It is an extension. Usually to handle big excel files. This is under development.

Specific sheet names can be imported with sheet names or number

```
install.packages("gdata")
library(gdata)

read.xls("cities.xls")
```

```
# Load the gdata package
library(gdata)

# Import the second sheet of urbanpop.xls: urban_pop
urban_pop <- read.xls("urbanpop.xls", sheet = "1967-1974")

# Print the first 11 observations using head()
head(urban_pop, 11)
```

```
# The gdata package is alreaded loaded

# Column names for urban_pop
columns <- c("country", paste0("year_", 1967:1974))

# Finish the read.xls call
urban_pop <- read.xls("urbanpop.xls", sheet = 2,
                      skip = 50, header = FALSE, stringsAsFactors = FALSE,
                      col.names = columns)

# Print first 10 observation of urban_pop
head(urban_pop, 10)
```

```
# Add code to import data from all three sheets in urbanpop.xls
path <- "urbanpop.xls"
urban_sheet1 <- read.xls(path, sheet = 1, stringsAsFactors = FALSE)
urban_sheet2 <- read.xls(path, sheet = 2, stringsAsFactors = FALSE)
urban_sheet3 <- read.xls(path, sheet = 3, stringsAsFactors = FALSE)


# Extend the cbind() call to include urban_sheet3: urban
urban <- cbind(urban_sheet1, urban_sheet2[-1], urban_sheet3[-1])

# Remove all rows with NAs from urban: urban_clean
urban_clean <- na.omit(urban)

# Print out a summary of urban_clean
summary(urban_clean)
```

XLConnect

Work with Excel through R, a bride, editing, formating, etc.

XLConnect requires Java

Useful to decide when to start reading

```
install.packages("XLConnect")
library("XLConnect")
```

```
book <- loadWorkbook("cities.xlsx"
str(book)
excel_sheets("cities.xlsx")
readWorksheet(book, sheet = "year_2000")
readWorksheet(book, sheet = "year_2000",
                startRow =3, 
                endRow =4, 
                startCol =2,
                header = FALSE) 
```



