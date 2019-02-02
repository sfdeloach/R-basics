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
print(x)    # explicit way to print a variable
ls()        # name of objects stored in workspace
rm(...)     # remove objects from workspace
help("fn")  # R documentation
?fn         # alternative method to invoke help
args(fn)    # provides argument list for function fn
data()      # available data sets

# small sample of base-included math functions
sqrt(x)
log(x)      # defaults to natural log base
log(x, 10)
pi          # constant
Inf         # constant
NA          # unable to infer type during coersion
NaN         # Not a number, i.e. 0/0
```

#### 1.3 DATA TYPES

```R
class(x)       # class information on variable x
library(lib)   # loads package `lib`
data("data")   # loads dataset `data`
str(x)         # structure of variable x
head(x)        # first six lines in table x
names(x)       # names of columns in table x
x$col_name     # accessor to a column, returns a vector
length(x$col)  # number of entries in vector
identical(a,b) # compares vars a & b, returns logical
```

##### NOTES

- A `Factor` is analagous to an _enumeration_
- A `data.frame` is analagous to a two-dimensional structure like a _SQL table_
- `sprintf("%f", var)` is a C-like way to print to the console
- Five basic atomic classes:
  - character
  - numeric (real numbers)
  - integer
  - complex
  - logical (True/False)

##### HOW TO CREATE A DATA FRAME (more in Section 3.2)

```R
temp <- c(35, 88, 42, 84, 81, 30)
city <- c("Beijing", "Lagos", "Paris", "Rio de Janeiro", "San Juan", "Toronto")
city_temps <- data.frame(name = city, temperature = temp)
> city_temps
            name temperature
1        Beijing          35
2          Lagos          88
3          Paris          42
4 Rio de Janeiro          84
5       San Juan          81
6        Toronto          30
```

### Section 2: Vectors, Sorting

#### 2.1 VECTORS

using concatenate `c()`, combine arguments to form a vector:

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

In general, coercion is an attempt by R to be flexible with data types. When an
entry does not match the expected, R tries to guess what we meant before
throwing it in there. But this can also lead to confusion. Failing to understand
coercion can drive programmers crazy when attempting to code in R, since it
behaves quite differently from most other languages.

Vectors must be all of the same type, however, the following script will not
produce an error:

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

A vector's order can be reversed by providing additional arguments to the
`sort()` function or by using `rev()`:

```R
> sort(x,decreasing=TRUE)
[1] 92 65 31 15  4
> rev(sort(x))
[1] 92 65 31 15  4
```

The function `order()` offers a similar sorting utility, however it sorts a
vector by its index:

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

The function `which.max()` returns the index and is stored in the variable
`i_max`. The same example works for `min()` and `which.min()`.

Using the example from before, the function `rank()` will rank the values in a
vector, lowest value is ranked #1:

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
|    31    |    4     |     2     |    3     |
|    4     |    15    |     3     |    1     |
|    15    |    31    |     1     |    2     |
|    92    |    65    |     5     |    5     |
|    65    |    92    |     4     |    4     |

#### 2.3 VECTOR ARITHMETIC

Arithmetic operations on vectors occur element-wise. A single value can be
applied to an entire vector using a number of operations. For example, let's
create a vector of heights measured in inches and use vector arithmetic to
convert the units to centimeters:

```R
> heights <- c(69,62,66,70,70,73,67,73,67,70)
> heights * 2.54
 [1] 175.26 157.48 167.64 177.80 177.80 185.42 170.18 185.42 170.18 177.80
```

Arithmetic operations on two vectors of similiar lengths will be applied on
values sharing the same index:

```R
> miles <- c(34,21,53,25,67)
> hour <- c(0.55,0.32,0.62,0.21,0.89)
> mph <- miles / hour
> mph
[1]  61.81818  65.62500  85.48387 119.04762  75.28090
```

### Section 3: Indexing, Data Wrangling, Plots

#### 3.1 INDEXING

Example demonstrating how to create a vector containing `makes` with greater
than 550 hp:

```R
# a contrived data frame
> imsa_cars
      make class  hp
1     Ford  GTLM 525
2 Cadillac   DPi 605
3    Mazda   DPi 605
4  Ferrari  GTLM 525

# <, <=, >, >=, ==, != are all acceptable relational operators
> index <- imsa_cars$hp > 550

# logicals can be used to index an existing vector
> index
[1] FALSE  TRUE  TRUE FALSE

# final result
> imsa_cars$make[index]
[1] Cadillac Mazda
```

Count the number of entities that meet a specified condition:

```R
# logicals are coerced to numerics, TRUE == 1, FALSE == 0
> sum(index)
[1] 2
```

Extract the indices of vector elements satisfying more than one logical
condition (i.e. Fords with at least 500 hp):

```R
# data frame from above slightly modified
> imsa_cars
      make class  hp
1     Ford  GTLM 525
2 Cadillac   DPi 605
3    Mazda   DPi 605
4  Ferrari  GTLM 525
5     Ford   GTD 515

# There is a difference in these logical operators:
#   &  element-wise, returns vector of same length
#   && single, returns value of first element
> ford_500_plus <- imsa_cars$make == "Ford" & imsa_cars$hp >= 500

# used as an index variable
> ford_500_plus
[1]  TRUE FALSE FALSE FALSE  TRUE

> imsa_cars$class[ford_500_plus]
[1] GTLM GTD
```

##### INDEXING FUNCTIONS

`which()` gives the ‘TRUE’ indices of a logical object, allowing for array
indices.

```R
> x <- c(TRUE,FALSE,FALSE,TRUE,TRUE,FALSE,FALSE,TRUE)
> which(x)
[1] 1 4 5 8
```

`match()` returns a vector of the positions of (first) matches of its first
argument in its second.

```R
> states <- c("FL","GA","AL","SC","NC","TN","MS","LA","TX","KY")
> match(c("NC","SC","TN"),states)
[1] 5 4 6
```

`%in%` is a more intuitive interface as a binary operator, which returns a
logical vector indicating if there is a match or not for its left operand.

```R
# using the same vector 'states' as defined earlier
> c("NC","MA","SC","CA","TN") %in% states
[1]  TRUE FALSE  TRUE FALSE  TRUE
```

#### 3.2 BASIC DATA WRANGLING

##### BASIC DATA WRANGLING

Advanced data analysis will require the manipulation of data tables. The `dplyr`
package is introduced to provide intuitive functionality

```R
# one-time installation of library
> install.packages("dplyr")

# once installed, load the package
> library(dplyr)
```

Load a CSV file for analysis:

```R
# as.is=TRUE will read strings as chars, otherwise strings are read as factors
> motorskills <- read.csv("motorskills.csv",header=TRUE,as.is=TRUE)
> motorskills
         date   name         agency time1 cones1 time2 cones2
1  01-31-2019   Bill      Altamonte   101      1    99      0
2  01-31-2019  Terry    Casselberry   104      1   101      0
3  01-31-2019    Tim Winter Springs   107      1   106      0
4  01-31-2019  David      Lake Mary   103      1   103      1
5  01-31-2019    Dan        Sanford   102      1   101      2
6  01-30-2019 Marcos       Longwood   109      1   104      0
7  01-30-2019  Kevin         Oviedo    95      3    99      1
8  01-30-2019  Allen       Maitland   107      0   104      0
9  01-30-2019   Matt    Winter Park   100      3   103      0
10 01-30-2019    Dom         Apopka    98      3   104      1
```

`mutate()` - adds new variables and preserves existing columns

```R
# add new column for the combined time
> motorskills <- mutate(motorskills, combined_time = time1 + time2)
> motorskills
         date   name         agency time1 cones1 time2 cones2 combined_time
1  01-31-2019   Bill      Altamonte   101      1    99      0           200
2  01-31-2019  Terry    Casselberry   104      1   101      0           205
3  01-31-2019    Tim Winter Springs   107      1   106      0           213
4  01-31-2019  David      Lake Mary   103      1   103      1           206
5  01-31-2019    Dan        Sanford   102      1   101      2           203
6  01-30-2019 Marcos       Longwood   109      1   104      0           213
7  01-30-2019  Kevin         Oviedo    95      3    99      1           194
8  01-30-2019  Allen       Maitland   107      0   104      0           211
9  01-30-2019   Matt    Winter Park   100      3   103      0           203
10 01-30-2019    Dom         Apopka    98      3   104      1           202
```

`select()` - keeps only the variables you mention

```R
> motorskills <- select(motorskills, name, agency, combined_time)
> motorskills
     name         agency combined_time
1    Bill      Altamonte           200
2   Terry    Casselberry           205
3     Tim Winter Springs           213
4   David      Lake Mary           206
5     Dan        Sanford           203
6  Marcos       Longwood           213
7   Kevin         Oviedo           194
8   Allen       Maitland           211
9    Matt    Winter Park           203
10    Dom         Apopka           202
```

`filter()` - find rows/cases where conditions are true

```R
# note that the operator != may be used to remove data from table
> motorskills <- filter(motorskills, combined_time <= 200)
> motorskills
   name    agency combined_time
1  Bill Altamonte           200
2 Kevin    Oviedo           194
```

Combine functions with the pipe operator `%>%`

```R
# start the example with a fresh read from the file
> motorskills <- read.csv("motorskills.csv", header=TRUE)

# note that this next line is a multiline command
> motorskills %>% mutate(ave_time = (time1 + time2) / 2) %>%
+ select(name, agency, ave_time) %>% filter(ave_time <= 101.0)
   name    agency ave_time
1  Bill Altamonte      100
2 Kevin    Oviedo       97
3   Dom    Apopka      101
```

##### CREATING DATA FRAMES

Observe how R defaults reading a column of strings as factors, and not chars:

```R
> grades <- data.frame(names=c("Ann","Bill","Carol","Dan"),
+ exam_1 = c(94,92,93,90),
+ exam_2 = c(88,84,82,89))
> class(grades$names)
[1] "factor"
```

To override this default behavior, use the additional argument
`stringsAsFactors`:

```R
> grades <- data.frame(names=c("Ann","Bill","Carol","Dan"),
+ exam_1 = c(94,92,93,90),
+ exam_2 = c(88,84,82,89),
+ stringsAsFactors=FALSE)
> class(grades$names)
[1] "character"
```

#### 3.3 BASIC PLOTS

`plot(x-axis, y-axis)` - generic X-Y plotting

```R
> x <- -20:20
> y <- x * x
> data <- data.frame(x = x, y = y)
> plot(data$x,data$y)
# with additional arguments
> plot(data$x,data$y,type="l",main="y = x^2",xlab="x",ylab="y")
```

`hist(vals)` - simple histogram

```R
> norm <- rnorm(10000)
> hist(norm)
# another example overriding the bin size
hist(rnorm(10000),breaks=seq(-4,4,0.10))
```

`boxplot(vals~subsection, dataframe)` - A box plot is a graphical rendition of
statistical data based on the minimum, first quartile, median, third quartile,
and maximum. The term "box plot" comes from the fact that the graph looks like a
rectangle with lines extending from the top and bottom.

```R
> data <- data.frame(val=rnorm(40),area=c("N","S","E","W"))
> boxplot(val~area,data=data)
```

![boxplot](https://raw.githubusercontent.com/sfdeloach/R-basics/images/boxplot.png)

### Section 4: Programming Basics

#### 4.1 INTRODUCTION TO R PROGRAMMING

#### 4.2 CONDITIONALS

#### 4.3 FUNCTIONS

#### 4.4 FOR LOOPS
