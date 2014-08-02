R-Programming
=============

---
  output: html_document
---

```{r include=FALSE}
rm(list=ls())
setwd("C:/Nuova Cartella/R Programming")
```

*Disclaimer*: The following material is mainly the transcription into the R shell, with comments, of the contents of the *swirl*'s course "*R Programming*". I'm currently unaware of any copyright issues. The rest are some digressions of mine on the previous topics.

# SWIRL - R Programming

### Istallation:
```{r, eval = FALSE}
install.packages("swirl")
library(swirl)
```

### Running:
```{r, eval = FALSE}
swirl()
```

### Let's get started!

## R Programming

### Basic Building Blocks
Performing a simple computation:
```{r}
5+7
```
Setting a *variable* (i.e. a container for values that can change) and displaying its value:
```{r}
x <- 5+7
x
```
Computations con involve previously set variables (x):
```{r}
y <- x -3
y
```

A **vector** is the simplest *data structure* (i.e. object that contains data) you can use in R. Even the simple variables are actually vectors of length 1 (e.g. x, y, ...)
```{r}
length(y)
str(y)
```
Keep in mind that vectors can store only objects of the **same type**. Vectors are built using the c() function (c stands for combine, meaning it takes its arguments and returns a vector structure holding them):
```{r}
z <- c(1.1, 9, 3.14)
length(z)
str(z)
z
```
c() function offers several way to combine objects, even taking other vectors as arguments:

```{r}
c(z, 555, z)
```
Numerical operators and numeric constants (2, 100, .... see below) are aware of the their arguments, and they behave politely:
```{r}
5+7
z
z * 2 + 100
```
In this case, since one argument of the * operator is a vector and the other is a constant, the operator * **expands 2 to a vector** of the same length of z and then  operates element-wise. Same happens for + and the constant 100: is reverted to a vector of 100s and + operates element-wise. The the result of the espression is indeed a vector. Same happens for ^, /, ... operators and for functions like sqrt() as well:
```{r}
sqrt(z + 1)    #
z/sqrt(z - 1)  # / operator in acrtion: everything is elementwise!
```
There's little quirk in this: operators apply this every time: when operators are used with vectors of different lengths, the shorter is adapted to the lengthof the longer recycling its elements. The case of z*2 s simply a particular case (2 is read as a vector of length 1):
```{r}
c(1, 2, 3, 4) + c(0, 10)
c(1, 2, 3, 4) + c(0, 10, 100)
```

### Sequences of Numbers
Sequences of numbers are created by the : operator:
```{r}
1:20
```
This means taking the 1st argument of :, repeat incrementing it by one until it reaches a value greather than the 2nd argument of:. Check of this condition is done **before** outputting the value. The created values are of the type of the 1st argument:
```{r}
pi:10
```
You can count downward:
```{r}
15:2
```
If you need a sequence of numbers with increment different than +/-1, you have to use the seq() function:
```{r}
seq(1,10)
seq(1, 10, 0.1)
```
The last parameter in that seq invokation os named 'by':
```{r}
seq(1, 10, by = 0.5)
```
This is important to note since seq() is flexible enough to let you make a sequence given its length, but you have to be carful to name the parameter. This code produces 30 evenly spaced numbers from 5 to 10 (interval is (10-5)/30):
```{r}
ss <- seq(5, 10, length = 30)
str(ss)
length(ss)
summary(ss)
```
And by the way, as you can notice, the outpur of seq() is a vector. Another possible use of seq is as follows:

```{r}
seq(along = ss)
seq_along(ss)
```
The function rep() istead produces replica of the object given as its 1st parameter:

```{r}
rep(0, 40)
rep(0, times = 40)
```
I wrote **object**, and I really meant it:
```{r}
rep(c(1, 2, 3), 40)

```
Another useful argument of `rep()` is `each`:
```{r}
rep(c(0, 1, 2), each = 10)

```

### Vectors
Comparison (logical) operators produce `TRUE` or `FALSE`. Of course they are vector-friendly:
```{r}
num_Vect <- c(0.5, 55, -10, 6)
num_Vect < 1
```
Logical operators are `>`, `<`, `>=`. `<=`, `==` and `!=`.
Boolean operators are instead: `|` (or), `&` (and), `!` (not).
For string you can use both single and doble quotes in R (but do not mix them!):
```{r}
my_char_double <- c("My", "name", "is")
my_char_double
my_char_single <- c('My', 'name', 'is')
my_char_single
my_char_double == my_char_single
```
String concatenating is of common use, but in R is actually ackward to perform:
```{r}
paste("Hello", "world!", sep = " ")
paste("Hello", "world!", collapse = " ")

paste(my_char_single, collapse = ' ')
```
`paste` works for vectors as well:
```{r}
paste(my_char_single, collapse = ' ')
```
This example shows how to **add an element to a vector**: you basically create a new vector by the concatenation of the old vector and a new vector formed of the new element:
```{r}
c(my_char_single, 'dave')
```
`paste` function starts to make sense for slightly more complicated jobs:
```{r}
paste(1:3, c("X", "Y", "Z"), sep=' ')
v <- paste(LETTERS, 1:4, sep = "-")
v
str(v)
```
In the latter example there are two things worth noting:

- 1st, the `1:4` vector gets recycled to mathc the length of the predefined `LETTERS` vector:

- 2nd, the result is a character vector, i.e. the `1:4` vector is coerced from integer to character (the type of the elements of `LETTERS`) and then processed by `paste`.

### Missing Values
A missing (in the **statistical** sense) value is denoted in R by `NA`. Operations on `NA` values yield other `NA` values.
```{r}
x <- c(44, NA, 5, NA)
x*3
```
Now let's create a vector that contains numbers and `NA`s at random:
```{r}
y <- rnorm(1000)
z <- rep(NA, 1000)
my_data <- sample(c(y, z), 100)
my_na <- is.na(my_data)
```
The `is.na` function can be used to test is an objectis an `NA`:
```{r}
my_na <- is.na(my_data)
my_na
str(my_na)
```

Unfortunately, `==` does not work the same way:
```{r}
my_data == NA
```
This is because `NA` is **not** a value of some type, it is simply a placeholder for a quantity that is not available. Thus, applyingthe application of any operator on that symbol produces `NA`. So, how many `NA`s did we actually get?
```{r}
num_na <- sum(my_na)
num_na
```

### Subsetting Vectors
Subsetting can be done with single indices, vectors of indices:
```{r}
x[10]
x[1:10]
```
can be done with vectors of booleans, meaning that when the boolean element is `TRUE` the element is kept, when the boolean is `FALSE`, the element is discarded. Of course we can use functions that return vectors of boolean.
`TRUE` (or `FALSE`) single value (vector of length 1) is recycled into a vector of `TRUE` (or `FALSE`) booleans of length equal to length(x):
```{r}
x[TRUE]
x[FALSE]

x[c(TRUE, FALSE)]
```
An useful application is as follows. Return a vector composed of the elements of `x`that are non-`NA`:
```{r}
y <- x[!is.na(x)]
```
The result of what follows is less useful, but interesting:
```{r}
y <- x[is.na(x)]
```
Lets' get back to the intersting `x` and perform another useful subsetting:
```{r}
y <- x[!is.na(x)]
y[y > 0]
```
This is not the same as:
```{r}
x[x> 0]
```
since the application of the `>` operator to `NA` values lead to `NA`. Lukily enough, we can do it in one shot:
```{r}
x[!is.na(x) & x > 0]
```

```{r}
x[c(1, 2, 3)]
x[-c(1, 2, 3)]
x[c(-1, -2, -3)]
```
R does not check if requested indices are out of the bounds of the vector. It is your job to grant that you use indices the right way.
R vectors can have their elements *named*:
```{r}
vect <- c(foo = 11, bar = 2, norf = NA)
vect
names(vect)
vect[1]
```
Names can be added to unnamed vectors to make them named:
```{r}
vect2 <- c(11, 2, NA)
names(vect2) <- c('foo', 'bar', 'norf')
```
But keep in mind that, named or unnamed, they are the same thing:
```{r}
identical(vect, vect2)
```
Names can be unsed to subset:

```{r}
vect["bar"]
vect[2]
vect[c("foo", "bar")]
```
Just in case you are wondering, you cannot use the `$` operator for vectors. And vectors does not have `dim()`.

### Matrices and Data Frames
Both serve the purpose to store data organized as a **table**. Matrices can only store data of a single type; data frames columns have to be of the same type, but it can vary across  columns.

Strange things happen when you give `dim` attribute to a vector:
```{r}
my_vector <- 1:20
my_vector
dim(my_vector)
length(my_vector)

dim(my_vector) <- c(4, 5)
dim(my_vector)
my_vector
attributes(my_vector)
attributes(my_vector)[1]
str(attributes(my_vector)[1])
attributes(my_vector)[[1]]
attributes(my_vector)[[1]][1]
attributes(my_vector)[[1]][2]
class(my_vector)
```
And see the magic:
```{r}
my_matrix <- my_vector
```
So we see that a matrix is simply a vector with a `dim`ension attribute. To confirm check this out:
```{r}
my_matrix2 <- matrix(1:20, 4, 5)
identical (my_matrix, my_matrix2)
```

And finally something that looks like a real application. If rows in the matrix `my_matrix` are observations of some people, we might want to name the rows to identify patients:
```{r}
patients <- c('Bill', 'Gina', 'Kelly', 'Sean')
cbind(patients, my_matrix)
```
Unfortunately, since matrix objects can store only objects of the same type, and character objects cannot be forced into meaningful numbers, the other objects are (implicitly) coerced into strings! Not quite what we wanted. We need to resorto to **data frames**:
```{r}
my_data <- data.frame(patients, my_matrix)
my_data
class(my_data)
str(my_data)
```
Now everything looks ok, I mean of the same type. No coercion took place, the `data.frame()` function simply assembled the new object from the original, unmodified components. We can name give meaningful names to the variables (columns) instead of the names `X1`, `X2`, ... assigned by `data.frame()`:
```{r}
cnames <- c('patient', 'age', 'weight', 'bp', 'rating', 'test')
colnames(my_data) <- cnames
my_data
```
