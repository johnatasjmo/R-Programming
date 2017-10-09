# Missing Data

missing data is represented as `NA`

other sources can be different than `NA`

Inf and NaN

NaN : check if something is divided by zero

`any(is.na(df))` will check if there are NA values in `df` 

`sum(is.na(df))` sums all NA

`summary()` function also tells you if there are NAs

`complete.cases(df) ` complete TRUE if there are complete cases and FALSE if there are missing values

`na.omit(df)` removes any rows with missing values



```
> ## The stringr package is preloaded
> 
> # Replace all empty strings in status with NA
> social_df$status[social_df$status == " "] <- social_df$status[social_df$status == "NA"]
> 
> # Print social_df to the console
> social_df
   name n_friends         status
1 Sarah       244     Going out!
2   Tom        NA               
3 David       145 Movie night...
4 Alice        43
> 
> # Use complete.cases() to see which rows have no missing values
> complete.cases(social_df)
[1]  TRUE FALSE  TRUE  TRUE
> 
> # Use na.omit() to remove all rows with any missing values
> na.omit(social_df)
   name n_friends         status
1 Sarah       244     Going out!
3 David       145 Movie night...
4 Alice        43
> 
```



### Outliers and obvious errors





