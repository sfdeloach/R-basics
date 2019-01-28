# Data Science: R Basics

## HarvardX: PH125.1x

### Section 1: R Basics, Functions, and Data Types

#### 1.2 BASICS

##### VARIABLE ASSIGNMENT

```R
a <- 5 # preferred
b = 1  # may cause confusion with ==
```

##### BASICS

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

#### 1.3 DATA TYPES

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

##### NOTES

-   A `Factor` is analagous to an _enumeration_
-   A `data.frame` is analagous to a two-dimensional structure like a _SQL table_
-   `sprintf("%f", var)` is a C-like way to print
-   Five basic atomic classes:
    -   character
    -   numeric (real numbers)
    -   integer
    -   complex
    -   logical (True/False)

##### HOW TO CREATE A DATA FRAME

```R
temp <- c(35, 88, 42, 84, 81, 30)
city <- c("Beijing", "Lagos", "Paris", "Rio de Janeiro", "San Juan", "Toronto")
city_temps <- data.frame(name = city, temperature = temp)
```

### Section 2: Vectors, Sorting

#### 2.1 VECTORS

using concatenate `c()`:

```R
> codes <- c(380, 124, 818)
> country <- c("italy", "canada", "egypt")

# add names to codes
> names(codes) <- country

# name columns in one expression
> codes <- c(italy=380, canada=124, egypt=818)
```

using sequence `seq()`:

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

##### VECTOR COERCION

In general, coercion is an attempt by R to be flexible with data types.
When an entry does not match the expected, R tries to guess what we meant
before throwing it in there. But this can also lead to confusion. Failing
to understand coercion can drive programmers crazy when attempting to code
in R, since it behaves quite differently from most other languages.

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

#### 2.2 SORTING

Take the following:

```R
> x = c(31,4,15,92,65)
> x
[1] 31  4 15 92 65
```

The vector `x` can be sorted in acsending order by the `sort()` function:

```R
> sort(x)
[1]  4 15 31 65 92
```

A vector's order can be reversed with the `rev()` function:

```R
> rev(sort(x))
[1]  92 65 31 15 4
```

The function `order()` offers a similar sorting utility, however it sorts a vector by its index:

```R
# create a new vector based on index numbers
> index = order(x)

> index
[1] 2 3 1 5 4

# sorted x vector
> x[index]
[1]  4 15 31 65 92

# proof that order creates a vector based on indices
> x[c(2,3,1,5,4)]
[1]  4 15 31 65 92
> identical(x[c(2, 3, 1, 5, 4)], x[index])
[1] TRUE
```

The following method demonstrates how to find the index that corresponds
with a min and max number in one column of a data frame, and how to use
the index to lookup the corresponding data in another column:

```R
> max(murders$total)
[1] 1257
> i_max <- which.max(murders$total)
> i_max
[1] 5
> murders$state[i_max]
[1] "California"
```

The function `which.max()` returns the index and is stored in the variable `i_max`. The same example works for `min()` and `which.min()`.

Using the example from before, the function `rank()` provides another utility for sorting:

```R
> x
[1] 31  4 15 92 65
> rank(x)
[1] 3 1 2 5 4
```

The first value in the ranked vector is 3 because 31 is the third smallest
number and the second number 4 is the smallest number, etc.

In summary:

| original | `sort()` | `order()` | `rank()` |
| :------: | :------: | :-------: | :------: |
|    31    |     4    |     2     |     3    |
|     4    |    15    |     3     |     1    |
|    15    |    31    |     1     |     2    |
|    92    |    65    |     5     |     5    |
|    65    |    92    |     4     |     4    |

#### 2.3 VECTOR ARITHMETIC

Arithmetic operations on vectors occur element-wise.  A single value
can be applied to an entire vector using a number of operations. For
example, let's create a vector of heights measured in inches and use
vector arithmetic to convert the units to centimeters:

```R
> heights <- c(69,62,66,70,70,73,67,73,67,70)
> heights * 2.54
 [1] 175.26 157.48 167.64 177.80 177.80 185.42 170.18 185.42 170.18 177.80
```

Arithmetic operations on two vectors of similiar lengths can be applied
in powerful combinations to provide useful results:

```R
> miles <- c(34,21,53,25,67)
> hour <- c(0.55,0.32,0.62,0.21,0.89)
> mph <- miles / hour
> mph
[1]  61.81818  65.62500  85.48387 119.04762  75.28090
```

### Section 3: Indexing, Data Wrangling, Plots

#### 3.1 INDEXING

#### 3.2 BASIC DATA WRANGLING

#### 3.3 BASIC PLOTS

### Section 4: Programming Basics

#### 4.1 INTRODUCTION TO R PROGRAMMING

#### 4.2 CONDITIONALS

#### 4.3 FUNCTIONS

#### 4.4 FOR LOOPS
