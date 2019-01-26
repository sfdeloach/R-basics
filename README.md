# Data Science: R Basics

## HarvardX: PH125.1x

### Section 1: R Basics, Functions, and Data Types

#### VARIABLE ASSIGNMENT

```R
a <- 5 # preferred
b = 1  # may cause confusion with ==
```

#### FUNCTIONS

```R
print(a)    # explicit way to print variable a
ls()        # name of objects stored in workspace
help("log")
?log
args(log)   # provides argument list
data()      # available data sets

sqrt(x)
log(x)      # defaults to natural log base
log(x, 10)
pi          # constant
Inf         # constant
```

#### DATA TYPES

```R
class(a)       # data.frame, Factor, num, char, etc
library(lib)
data("data")
str(x)         # structure of variable x
head(x)        # first six lines in table x
names(x)       # names of columns in table x
x$col_name     # accessor to a column, returns a vector
length(x$col)  # number of entries in vector
identical(a,b) # compares vars a & b, returns logical
```

#### NOTES

- A ```Factor``` is analagous to an *enumeration*
- A ```data.frame``` is analagous to a two-dimensional structure like a *SQL table*
- ```sprintf("%f", var)``` is a C-like way to print
- Five basic atomic classes:
  - character
  - numeric (real numbers)
  - integer
  - complex
  - logical (True/False)

### Section 2: Vectors, Sorting

#### VECTORS

using concatenate ```c()```:

```R
> codes <- c(380, 124, 818)
> country <- c("italy", "canada", "egypt")

# add names to codes
> names(codes) <- country

# name columns in one expression
> codes <- c(italy=380, canada=124, egypt=818)
```

using sequence ```seq()```:

```R
> seq(1, 10)    # [1] 1 2 3 4 5 6 7 8 9 10
> seq(1, 10, 2) # [1] 1 3 5 7 9
> 1:10          # [1] 1 2 3 4 5 6 7 8 9 10

# length.out sets the length of the output
> x <- seq(0, 100, length.out = 5)
> x
[1]   0  25  50  75 100
```

subsetting:

```R
> codes[2] # not zero based!
canada
   124

# multi-entry vector, return 1 and 3
> codes[c(1,3)]
italy egypt
  380   818

# multi-entry vector, return 1 thru 2
> codes[c(1:2)]
italy canada
  380    124

# access by name
> codes["italy"]
italy
  380

# multi-entry creation by name
> codes[c("italy","egypt")]
italy egypt
  380   818
```

#### VECTOR COERCION

In general, coercion is an attempt by R
to be flexible with data types.
When an entry does not match the expected,
R tries to guess what we meant before throwing it in there.
But this can also lead to confusion.
Failing to understand coercion can drive programmers crazy
when attempting to code in R, since it behaves quite differently from most
other languages.

Vectors must be all of the same type, however, the following script will not produce an error:

```R
> x <- c(1,"canada",3)
> x
[1] "1"      "canada" "3"
> class(x)
[1] "character"
```

R includes functions to force vectors into a specfied class

```R
> x <- 1:5
> y <- as.character(x)
> y
[1] "1" "2" "3" "4" "5"
> as.numeric(y)
[1] 1 2 3 4 5
```

missing data and NAs (not to be confused with NaN)

```R
> x <- c("1","b","3")
> as.numeric(x)
[1]  1 NA  3
Warning message:
NAs introduced by coercion
```

#### SORTING
