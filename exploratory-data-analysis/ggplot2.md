title

dimensions and aestethics

```
# A scatter plot has been made for you
ggplot(mtcars, aes(x = wt, y = mpg)) +
  geom_point()

# Replace ___ with the correct column
ggplot(mtcars, aes(x = wt, y = mpg, color = disp)) + geom_point()

# Replace ___ with the correct column
ggplot(mtcars, aes(x = wt, y = mpg, size = disp)) +  geom_point()
```



gem\_smooth\(\)

```
# Explore the diamonds data frame with str()
str(diamonds)

# Add geom_point() with +
ggplot(diamonds, aes(x = carat, y = price)) + geom_point()


# Add geom_point() and geom_smooth() with +
ggplot(diamonds, aes(x = carat, y = price)) + geom_point() + geom_smooth()


```

add smooth line and combine geom

```
# 1 - The plot you created in the previous exercise
ggplot(diamonds, aes(x = carat, y = price)) +
  geom_point() +
  geom_smooth()

# 2 - Copy the above command but show only the smooth line
ggplot(diamonds, aes(x = carat, y = price)) +
  geom_smooth()


# 3 - Copy the above command and assign the correct value to col in aes()
ggplot(diamonds, aes(x = carat, y = price)) +
  geom_smooth() +
  aes( color = clarity)
```





