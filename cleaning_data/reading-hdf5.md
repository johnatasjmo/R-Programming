Reading HDF5

Used for storing large data sets, supports storing range of data types, heirarchicaly

Groups containing zero or more data sets and metadata. Have a group header with group name and list of attributes. Have a group symbol talbe with a list of objects in group

Datasets multidimanesional array of data elements with data. Header with name, data type, dataspace, storage layout. Have a data array with data.

[hdfgroup](htpp://www.hdfgroup.org)

```r
source("http://bioconductor.org/biocLite.R")
biocLite("rhdf5")
```

```r
library(rhdf5)
```

```r
created = h5createFile("example.h5")
```

```r
created = h5createGroup("example.h5","foo") # create foo group on file example.h5
created = h5createGroup("example.h5","baa")
created = h5createGroup("example.h5","baa/foobaa")
> h5ls("example.h5")
  group   name     otype dclass dim
0     /    baa H5I_GROUP           
1  /baa foobaa H5I_GROUP           
2     /    foo H5I_GROUP
```

```r
A = matrix(1:10,nr=5,nc=2)
h5write(A, "example.h5","foo/A")
B = array(seq(0.1,2.0,by=0.1),dim=c(5,2,2))
attr(B, "scale") <- "liter"
h5write(B, "example.h5", "foo/foobaa/B")
h5ls("example.h5")
```

Resources

[bioconductor](https://bioconductor.org/packages/release/bioc/html/rhdf5.html)

