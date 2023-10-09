# R Basics
## Important
* **Almost everything is a vector in R!** Number 2 is technically a 1-element vector and can be used wherever a vector is needed.
  * for that reason `c(X)` is always synonymous to X, thus redundant, thus there's never a reason to use `c(2)`
* Some types are subtypes of others. Use `is.<type>(variable)` instead of `typeof(variable) == "type"` when possible
* `T` and `F` are theoretically synonymous to `TRUE` and `FALSE` respectively. However, it's also possible to use `T` and `F`
    as variable names, thus overriding their values. **Do not do that**, unless you are ready for major confusion later!
* If we need help with something, `?something` can be used in R console - for example `?sqrt` will show help for `sqrt`
* Function arguments can always be passed either as positional or as named

## Basics
### Variables & Types
```r
var1 <- 123
123 -> var1   # synonymous
a <- b <- c <- 3  # multiple assignments at once

a <- 3        # typeof(a) == "double"
b <- 3.14     # typeof(b) == "double"
z <- 5L       # typeof(z) == "integer"
# 9 is synonymous to 9.0
# .7 is synonymous to 0.7
# 1.4e2 is synonymous to 140

c <- 3+2i     # typeof(c) == "complex"
d <- 'abc'    # or "abc"; typeof(d) == "character"
e <- T        # or TRUE;  typeof(e) == "logical"
f <- F        # or FALSE; typeof(f) == "logical"

print(5) == print(5L) == print(5.0)  # all will display as just 5, not to make things too easy :)
```

### Type conversions and checks
```r
as.integer(3.99) == 3L
as.numeric(5L) == 5.0
as.character(15) == "15"
as.double("17") == 17.0
# etc.

is.numeric(10L) == TRUE
# etc.
```

### Non-standard values
#### `NULL`, `NA` and `NaN`
```r
NaN  # Not a Number (it's a numeric value, representing that value is not a number)
NA   # Not Applicable
NULL  # represents total lack of value

c(1, 2, NULL, NA, NaN, 4) == c(1, 2, NA, NaN, 4)  # NULL is not a value at all, will be ignored

is.numeric(NaN)  == TRUE
is.numeric(NA)   == FALSE
is.numeric(NULL) == FALSE

is.nan(...)   # will return TRUE for NaN
is.na(...)    # will return TRUE for both NA and NaN
is.null(...)  # will return TRUE for NULL
# Do not check these using == operator; always use the checks above

2 + NaN  # => NaN
2 + NA   # => NA
2 > NaN  # => NA (does not return NaN, because > operator does not return a number)
# 2 + NaN == NaN: will equal NA; even though technically true, all comparisons on NaN return NA

# Many functions have the possibility to ignore NAs and NaNs
sum(c(1, 2, NA, NaN, 4))          # => NA
sum(c(1, 2, NA, NaN, 4), na.rm=T) # => 7
```

#### Infinity
```r
is.numeric(Inf)  == TRUE
is.numeric(-Inf) == TRUE

12 / 0 == Inf   # not an error
-7 / 0 == -Inf
```

### Operations on numbers
```r
a <- 5.1
b <- 1.9

# +, -, *, <, <=, >, >= and == work normally

a / b == 2.684211 # standard division
a %/% b == 2      # integer division
a %% b == 1.3     # mod form division

# for bitwise operations use functions "bitwAnd", "bitwOr" etc.

2 ^ 3 == 8
log(8, 2) == 3  # log(x) is natural logarithm, log2(x) and log10(x) are 2- and 10-based respectively
exp(1) == 2.718282  # exp(x) means e ^ x

sum(1, 2, 3) == 6
max(5, 1, 3) == 5
min(5, 1, 3) == 1
range(5, 1, 3) == c(1, 5)  # min and max

sqrt(3) == 1.732051
abs(-4.7) == 4.7
ceiling(1.1) == 2
floor(4.9) == 4
round(123.456) == round (123.456, 0) == 123
round(123.456, 2) == 123.46
```

### Logic
```
&& and & - AND
|| and | - OR
xor()    - XOR (not an operator, just a function)

& and | can be used on vectors, && and || cannot

isTRUE(), isFALSE() - useful when we are not 100% sure that something is a logical value
```

### Strings
```r
# ' and " are always synonymous, also in multiline strings
a <- 'abc'
b <- "abc"
a == b

c <- 'abc
def'
d <- 'abc\ndef'
c == d

print("a\tb")  # "a\tb"
cat("a\tb")    # a   b  (respects special characters)

nchar("1234567") == 7
grepl("45", "1234567") == T  # checks if a substring is present
paste("abc", "def", "ghi") == "abc def ghi"
paste0("a", "b", "c") == abc  # synonymous to paste(..., sep='') - does not insert spaces
```

### if, while, for
```r
if (condition) { instructions } else if (condition) { instruction } else { instruction }
while (condition) { instructions }  # break and next (in other languages "continue") can be used
for (abc in collection) { instructions }  # collection can be a vector, list, array etc. Anything iterable
for (i in 0:10) { instructions }  # repeats 11 times (we use numbers 0-10 as a collection)
```

### Functions
```r
abc <- function(a, b = 0) { return(a + b) }
abc(4, 8) == 12
abc(4) == 4  # default value of parameter b has been used
```

### Vectors
```r
vec <- c(7, 2, 3)
# vectors can have one type:
#     c(1, "2") gets casted to c("1", "2")
#     c(7, FALSE) gets casted to c(7, 1)
#     c(1, "2", TRUE) gets casted to c("1", "2", "TRUE")
#     c(5, 2+7i) gets casted to c(5+0i, 2+7i)

5 : 8      # synonymous to c(5, 6, 7, 8); useful for example in for loops
1.7 : 6.3  # c(1.7, 2.7, 3.7, 4.7, 5.7)
seq(from=0, to=88, by=22)  # c(0, 22, 44, 66, 88)

length(vec) == 3
sort(vec)  # c(2, 3, 7), can also sort strings and logicals (F < T)
vec[2] == 2  # indexing starts with 1
vec[-2]  # c(7, 3); takes all but second element
# vec[1] <- 100  would change the value of the first element

vec[1:2]  # c(7, 2); synonymous to vec[c(1,2)]
vec[c(1, 3, 3, 2)]  # c(7, 3, 3, 2)

rep(1, 7)         # 1 1 1 1 1 1 1  <- still a vector, just a clearer notation
rep(1:3, times=3) # 1 2 3 1 2 3 1 2 3; synonymous to rep(1:3, 3)
rep(1:3, each=3)  # 1 1 1 2 2 2 3 3 3

append(vec, 1)  # returns c(7, 2, 3, 1); does not alter original variable
c(vec, c(8, 9)) # 7 2 3 8 9 (combines vectors)

8 %in% c(1, 2, 8) == TURE  # checks if 8 is in the vector
```

#### Vector operations (very tricky, but important)
Reminder: **almost everything is a vector**. That explains why `length("abc") == 3`. That is because `"abc"` is a vector
containing just one string. Operators mentioned before can usually be used on vectors, as long as **length of the longer
one has to be a multiple of shorter one's length - we can use them on vectors of length 4-4, 4-2, 1-19, 3-9 etc. The
shorter one is then used like it was repeated enough times to make lengths equal. For an example, let's use multiplying:
```r
# by the way, == can also be used on vectors - it produces a vector of TRUE/FALSE values
2 * 3 == 6  # vectors of length 1 and 1. Just multiplied
c(1, 2) * c(3, 4) == c(3, 8)  # first elements multiplied, second elements multiplied
c(1, 2) * 3 == c(3, 6)  # 3 is treated like c(3, 3)
c(1, 2) * c(4, 5, 6, 7) == c(4, 10, 6, 14)  # c(1, 2) is treated as c(1, 2, 1, 2)
c(1, 2) * c(4, 5, 6)  # ERROR - 3 is not a multiple of 2

# by the way, c(X) is always synonymous to X, thus redundant
```

#### Vector filtering
```r
v <- c(3, 9, 5, 1)

v[c(T, F, F, T)] == c(3, 1)  # only elements where value is TRUE are taken
v[v > 4] == c(9, 5)  # only elements larget than 4 will be taken
# v > 4 creates a vector c(F, T, T, F),  which is later used to filter targets
v[c(T, F)] == c(3, 5)  # same as usual, length of a vector can be a sub-multiple
```

### Lists
Lists don't judge. They can have multiple types of data at once. `list("abc", 6, c(T, F))`
consists of three vectors (yes, primitive data is still a vector). Unlike vectors, lists can be multidimensional:
```r
# merging lists
L <- list(1, "abc")  # list of 1 and "abc"
LL <- list(L, L)  # list of two lists - 1 and "abc", 1 and "abc"
LLL <- list(LL, LL)  # list of two lists, each of them has two lists...

# merging vectors
C <- c(1, 2)  # 1 2
CC <- c(C, C)  # 1 2 1 2 (flattened)
CCC <- c(CC, CC)  # 1 2 1 2 1 2 1 2 (flattened)

# combinations
CL <- c(L, L)  # list containing 1, "abc", 1, "abc"
CLL <- c(LL, LL)  # list containing four lists (equal to c(L, L, L, L)
CLLL <- c(LLL, LLL)  # list containing four lists (equal to c(LL, LL, LL, LL))
# as we can see, lists got merged, without flattening
```

### Matrices
**Matrix is not a vector!** It's two-dimensional. Same as vectors, types will be cast to match
```r
M <- matrix(1:20, 4, 5)
# M will have 4 rows, 5 columns and will be filled with values from 1:20 by columns (column 1 will have 1 2 3 4)
# Vector of falues to fill can be a sub-multiple - for example 1:10, 1:5, 1:4, 1:2 (again, will be repeated to match)
# Using just "abc" will fill matrix with only "abc"s
matrix(1:20, 4, 5, byrow=T)  # similar, but will fill by rows instead of columns

nrow(M) == 4
ncol(M) == 5
dim(M) == c(4, 5)

cbind(1:4, c(7, 8), 5)  # col1 = 1 2 3 4; col2 = 7 8 7 8; col3 = 5 5 5 5 (merges vectors into one matrix by adding columns)
rbind(...)  # same, but by rows
# matrix of correct size (neither a multiple nor a sub-multiple) can also be used as an argument

t(M)  # returns transposed M matrix

M[2, 3] == 10  # 2nd ROW, third COLUMN
M[2,]  # whole 2nd row
M[,3]  # whole 3rd column
# instead of numbers, vectors can be used to access multiple rows/columns
# negative indices can also be used; M[-1, -1] will have first row and first column removed

M[c(T, F),]  # will take 1st and 3rd rows (same as with vector filtering; possible also for columns)
```

### Vector / matrix names
Vectors, matrix columns and rows can have names. When they do, elements can be accessed by both indices and names.
Among other uses, it makes it possible to filter a vector, and then access labels of the remaining values
```r
V <- 1:5
names(V) == NULL

names(V) <- c('a', 'b', 'c', 'd', 'e')
V[2] == V['b']
names(V[3]) == "c"

names(V) <- NULL  # removes names

M <- matrix(1:20, 4, 5)
rownames(M) <- c('A', 'B', 'C', 'D')
colnames(M) <- C("v", "w", "x", "y", "z")
```

### Data frames
```r
D <- data.frame (
  Training = c("Strength", "Stamina", "Other"),
  Pulse = c(100, 150, 120),
  Duration = c(60, 30, 45)
)

D[2] == c(100, 150, 120)  # returns a data frame, containing just the first column
D[["Pulse"]]  # synonymous
D$Pulse  # synonymous

D[,2]  # returns the first column as a vector

D[2,3] == 30  # 2nd row (record), 3rd column (Duration)
D[2, "Duration"] == 30  # synonymous to D$Duration[2]
D[2,]  # data frame with just the second record
# as always, vector can be given instead of an index to take more rows (records) / columns

rownames(D) == c('1', '2', '3')  # these are default row names
rownames(D)[c(1, 3)] <- c('aaa', 'bbb')
rownames(D) == c('aaa', '2', 'bbb')
# if we add a new row, its name will automatically become '4'

head(D, 2)  # data frame containing the two first records
head(D, -1) # data without last record
tail(D, 2)  # same, but last two
tail(D, -1) # data without first record

ncol(D) == 3  # there are 3 columns - Training, Pulse and Duration; synonymous to length(D)
nrow(D) == 3  # there are 3 records
dim(D) == c(3, 3)

rbind(Data_Frame, c("Cardio", 110, 110))  # add a row
cbind(Data_Frame, Steps = c(1000, 6000, 2000))  # add a column
# these can also be used to combine two data frames, but:
#    - for rbind, both data frames need to have similar column set
#    - for cbind, both data frames need to have same number of records (rows)

summary(D)  # shows a summary of data for the data frame

read.csv("abc/def.csv")  # reads a data frame from a file
```

### Factors
```r
W <- factor(c('a', 'b', 'c', 'c', 'c', 'b', 'a', 'b', 'b', 'b'))  # we don't use F, not to override global F <- FALSE
levels(W) == c('a', 'b', 'c')  # unique values

W[4] == 'c'
W[4] <- 'x'  # ERROR - value not among levels

length(W) == 10    # number of elements
length(levels(W))  # number of levels

table(W)  # returns a vector where values are number of particular level occurrences, and value names are level names
table(W)['b'] == 5  # number of 'b' occurrences
sort(table(W), decreasing=TRUE)  # levels sorted from the most common 

W2 <- factor(c('a', 'b', 'c'), levels=c('b', 'c', 'd'))
levels(W2) == c('b', 'c', 'd')
as.vector(W2) == c(NA, 'b', 'c')
# 'd' is now an allowed value despite not being in the original. However, 'a' is not allowed anymore, so it became NA
W2[1] <- 'a'  # ERROR
W2[1] <- 'd'  # ok
W2[1] <- NA   # ok
```

### Setting global options
For list of available options, use `?options`
```r
options()  # lists currently set options
options(scipen=5)  # sets an option
getOption("scipen") == 5  # gets an option

options(abcdef=8)  # custom options can also be set
getOption("abcdef") == 8

getOption("xxx")  # NULL - this option is not set
getOption("xxx", default=15) == 15  # default value to be returned when option is not found
```
