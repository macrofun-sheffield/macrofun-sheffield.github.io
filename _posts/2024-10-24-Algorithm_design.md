---
author: tguillerme
layout: page
title:  "Algorithm designs"
teaser: "Just functionalise your code"
categories:
    - session
tags:
    - worshop
    - coding
    - R
---

# Functionalise

That's it.
Just do that.

### But was is functionalising?

The idea is to divide the tasks of your aglorithm into standardised and transposable units.
Basically making the smaller possible functions that do a specific thing: i.e. small reusable bits of code that do one thing.

This sounds a bit daunting at first but can save a lot of time in the future (e.g. for big task or repetitive tasks).
But mainly I think it makes the code much more easier to understand and makes you think about what the code is doing (and why) before actually doing it.

It also allows you to better understand the structure of objects in R which is inherently what you are playing with when functionalising your code: you are going to modify objects in different ways (e.g. data manipulations) in order to get something out of it for publication (plot, table, p-value, etc.).

### Which objects?

Objects in R are pretty much everything in R that you manipulate write and transform.
In it's essence, `R` is a smart interface for the `C` programming language.
In `C` you can pretty much everything you want and need scaling up from bitwise operations (AND, OR, XOR, etc.) on bites (sets of bits).
I find it sometimes useful to go back to this very basic structure to understand that what you are doing in `R` is not much more different but `R` thankfully does all the heavy lifting for you.

For example, you want to do this simple operation:

```{r}
a <- 1
b <- 42
c <- a + b
```

Basically, `R` is going to assign the integer value `1` to an element you call `a`.
By the way the arrow is the assignment function: try doing writing \`<-\`(z, 2) in `R` and see what it does.
It's basically the function to attribute the integer value `2` to the element called `z`.
Now they are called `a` and `1` etc on you interface but basically they are just pointers to memory addresses in your machine: the integer `1` is stored in binary somewhere on your RAM (`0000000000000001`) which has an address (some hexadecimal stuff like this: `r lobstr::obj_addr(a)`) and the address is conveniently called `a` in your computer.
Same goes for `b` and if you do `a + b` (or \`+\`(a, b) - same thing) it's adding what these two addresses are pointing at together:


```
   0000000000000001
 & 0000000000101010
-------------------
   0000000000101011
```


And attributing these results to a spot in the RAM that you can track (here this address for `c` is `r lobstr::obj_addr(c)`).
And this basically scaling up to everything you do.

I find that keeping this in mind (at some level) can help you understand what is actually happening in your code beyond the actually statsitics you are doing: i.e. what are you asking your computer to do.

For example `"list"` objects are usually nested lists of addresses pointing to specific data that can be modified depending on what you need (this will make sense when we'll talk about `for` loops ;)).


![]({{ site.baseurl }}/Lists_from_Wickham.png)

Screenshot from Hadley Wickham's best book: [Advanced R](https://adv-r.hadley.nz/index.html). Basically a lot of useful things are in this book!

# Functionalising your scripts:

So how do you functionalise stuff knowing all that?

I wrote this for a project in 2011.
Note that the comments are in french but pretty easy to understand in english.

```{r, eval = FALSE}
dat2<-datat[,c(1,5:8,11,16,17)]
for (i in 2:8)
{a<-which(is.na(dat2[,i]))
if (length(a) >= 1){
dat2<-dat2[-which(is.na(dat2[,i])),]}}
dat2b<-scale(dat2[,2:8])
#décomposition de la matrice de variance covariance en vecteur et valeur propres
s2b<-svd(cov(dat2b))
barplot(s2b$d)
#examen des valeurs propres
barplot(s2b$d/sum(s2b$d))
s2b$d/sum(s2b$d)
cumsum(s2b$d/sum(s2b$d))
#position des individus sur les deux premi√®re composantes principale
proj2<-dat2b%*%s2b$u
plot(proj2[,1:2], pch=21, bg=(1:16)[ as.factor( as.character(dat2[,1]) ) ])

dat3<-datat[,c(1,5:8,11,13,16,17,18)]
for (i in 2:10)
{a<-which(is.na(dat3[,i]))
if (length(a) >= 1){
dat3<-dat3[-which(is.na(dat3[,i])),]}}
dat3b<-scale(dat3[,2:10])
#décomposition de la matrice de variance covariance en vecteur et valeur propres
s3b<-svd(cov(dat3b))
barplot(s3b$d)
#examen des valeurs propres
barplot(s3b$d/sum(s3b$d))
s3b$d/sum(s3b$d)
cumsum(s3b$d/sum(s3b$d))
#position des individus sur les deux premi√®re composantes principale
proj3<-dat3b%*%s3b$u
plot(proj3[,1:2], pch=21, bg=(1:12)[ as.factor( as.character(dat3[,1]) ) ])

dat4<-datat[,c(1,5:8,11)]
for (i in 2:6)
{a<-which(is.na(dat4[,i]))
if (length(a) >= 1){
dat4<-dat4[-which(is.na(dat4[,i])),]}}
dat4b<-scale(dat4[,2:6])
#décomposition de la matrice de variance covariance en vecteur et valeur propres
s4b<-svd(cov(dat4b))
barplot(s4b$d)
#examen des valeurs propres
barplot(s4b$d/sum(s4b$d))
s4b$d/sum(s4b$d)
cumsum(s4b$d/sum(s4b$d))
#position des individus sur les deux premi√®re composantes principale
proj4<-dat4b%*%s4b$u
plot(proj4[,1:2], pch=21, bg=(1:20)[ as.factor( as.character(dat4[,1]) ) ])
```

First thing to notice is that each chunk are pretty similar in their (ugly) structure so it seems to bits that do the same things three different times.
We can zoom on one bit an make it more readable (I think past me didn't had a keyboard with a space bar)


```{r, eval = FALSE}
dat2 <- datat[, c(1, 5:8, 11, 16, 17)]

for (i in 2:8) {
    
    a <- which(is.na(dat2[, i]))
    
    if (length(a) >= 1) {
        dat2 <- dat2[-which(is.na(dat2[, i])), ]
    }
}

dat2b <- scale(dat2[,2:8])

#décomposition de la matrice de variance covariance en vecteur et valeur propres
s2b <- svd(cov(dat2b))
barplot(s2b$d)

#examen des valeurs propres
barplot(s2b$d/sum(s2b$d))
s2b$d/sum(s2b$d)
cumsum(s2b$d/sum(s2b$d))

#position des individus sur les deux premi√®re composantes principale
proj2 <- dat2b%*%s2b$u
plot(proj2[,1:2], pch = 21, bg = (1:16)[as.factor(as.character(dat2[,1]))])
```

We can then divide into distinguishable chunks:

### 1. subsetting some columns from some matrix or data.frame

```{r, eval = FALSE}
## Subsetting some columns from a matrix/data.frame
dat2 <- datat[, c(1, 5:8, 11, 16, 17)]
```

OK. Let's rewrite that as a function then (easy one):

```{r, eval = FALSE}
## Subsetting columns from the data
subset.data <- function(data, columns) {
    return(data[, columns])
}
```

### 2. looping through the number of columns (using apply!)

Excluding the first one column for some reason.
In the loop it seems we are first looking for NAs in a column and removing the corresponding rows if there are some NAs.

```{r, eval = FALSE}
## Looping through all columns, excluding the first one
for (i in 2:8) {
    
    ## Looking for NAs and storing them into a
    a <- which(is.na(dat2[, i]))
    
    ## If there is an NA remove the whole row from the data
    if (length(a) >= 1) {
        dat2 <- dat2[-which(is.na(dat2[, i])), ]
    }
}
```

> Note that the code is looking through each column every time and is checking which column has NAs twice! We can probably optimise that eh!

This is what the loop is actually doing:

```{r, eval = FALSE}
## Detecting columns with NAs
detecting.rows.with.na.by.column <- function(data) {
    ## Creating an empty vector that will contain the NAs
    row_has_na <- integer()
    for (one_column in 1:ncol(data)) {
        ## Incrementing the list of columns
        ##(it will not be filled if the column has no NA)
        row_has_na <- c(row_has_na , which(is.na(data[, one_column])))
    }
    ## Return all the rows with NAs
    return(unique(row_has_na))
}

## Removing rows if they have an NAs
## We actually have done the "if they have NAs" bit above
## so we can just do remove rows
remove.rows <- function(data, rows) {
    return(data[-rows, ])
} 
```


> `loop` tip 1: note that `for` loops are RAM intensive because they often have to create a copy of your data in each iteration, not being able to guess where you are going with it. You can speed that up by specifying where you're going by creating an empty object to fill at the start of the loop (here `column_has_na <- integer()` will specify we're gonna store integers so it avoids the loop copying the RAM for the whole data each time - it'll just keep track of the integers).

> `loop` tip 2: avoid useless index names like `i` or `j`. Writing down what is happening makes it much easier for humans to read (e.g. here `data[, one_column]` makes it clear we're selecting one column, compared to `data[, i]`, especially if you enjoy deeply nested loops!). 

### For a loop through data.frames or matrices: use `apply`. Just do it.

Actually, if you think of it, this function is just checking whether columns have NA in a dataset and returning their numbers.
That means collecting a vector that is of any length (0 if no NA) and corresponding to the rows that have at least one NA.
You are applying a function "find NAs in a column" to each column of the data...
Uh oh! `apply`!

```{r, eval = FALSE}
## Rewriting the function using a simple apply
detecting.rows.with.na.by.column <- function(data) {
                  ## The input matrix/data.frame
    row_has_na <- apply(X = data,
                  ## Whether to look at rows (1) or columns (2)
                        MARGIN = 2,
                  ## What to do to each column
                        FUN = function(data) which(is.na(data))))
    ## return just the row unique row numbers
    return(unique(c(row_has_na)))
}
```

Once you'll get used with `apply` you'll probably just write it as a one liner:

```{r, eval = FALSE}
detecting.rows.with.na.by.column <- function(data) {
    ## return just the row unique row numbers
    return(unique(c(apply(data, 2, function(data) which(is.na(data))))))
}
```

So we can rewrite the whole loop using these functions:

```{r, eval = FALSE}
## Remove rows that correspond to columns with NAs
remove.rows.for.na.columns <- function(data) {
    return(remove.rows(data, detecting.rows.with.na.by.column(data)))
}
```

```{r, eval = FALSE}
## That's a cleaner loop!
data[, -1] <- remove.rows.for.na.columns(data[, -1])

```

### 3. scaling the data

This one is easy as it is just scaling and there's already a function called scale.
Note here that we can ignore the subseting of the data since we can just make sure the whole input ignores the first column.

```{r, eval = FALSE}
## Scaling the data
scale(data)
```

### 4. doing some fancy maths

This chunk looks scary and fancy, what's happening here?

```{r, eval = FALSE}
#décomposition de la matrice de variance covariance en vecteur et valeur propres
s2b <- svd(cov(dat2b))
barplot(s2b$d)

#examen des valeurs propres
barplot(s2b$d/sum(s2b$d))
s2b$d/sum(s2b$d)
cumsum(s2b$d/sum(s2b$d))

#position des individus sur les deux premi√®re composantes principale
proj2 <- dat2b%*%s2b$u
```

Joking it's actually just doing a PCA (by hand, not sure why) and extracting the variance per dimensions


```{r, eval = FALSE}
## Doing the PCA
pca <- prcomp(data)
```

> Note that here we can also skip the scaling step by just using the option `scale = TRUE` in `prcomp`.

```{r, eval = FALSE}
## Get the cumulative variance per dimensions
pca.var.dim <- function(pca) {
    variances <- apply(pca$x, 2, var)
    ## Relative variances
    return(variances/sum(variances))
}
```

### 5. plotting the results

And the last bit is about plotting the results.
We can skip this for now.

```{r, eval = FALSE}
plot(proj2[,1:2], pch = 21, bg = (1:16)[as.factor(as.character(dat2[,1]))])
```

## Combining all these functions into a script

We can now merge all these functions in a way that makes sense:

```{r, eval = FALSE}
## Select some columns
new_data <- subset.data(data, columns)

## Remove rows with nas
new_data <- remove.rows.for.na.columns(new_data)

## Running a pca
pca <- prcomp(new_data, scale = TRUE)

## And then some plotting:
barplot(pca.var.dim(pca))
plot(pca$x[,c(1,2)], pch = 21)
```

But remember, we want to do that multiple times to different datasets!
Let's just finally functionalise the whole pipeline:

```{r, eval = FALSE}
## Run a PCA with no NAs on specific columns
run.pca.with.no.na <- function(columns, data) {
    ## Select columns
    data <- subset.data(data, columns)
    ## Remove NAs
    data <- remove.rows.for.na.columns(data)
    ## Run a pca on the scaled data
    return(prcomp(data, scale = TRUE))
}
```

> Note here I've put `columns` as a first argument (and not data). This doesn't matter much in general but it's a good practice to put your variable first. We'll see why in just a second

And just rewrite our whole script like this:

```{r, eval = FALSE}
## First set
columns <- c(5:8,11,16,17)
first_pca <- run.pca.with.no.na(columns, data)

## Second set
columns <- c(5:8,11,13,16,17,18)
second_pca <- run.pca.with.no.na(columns, data)

## Third set
columns <- c(5:8,11)
third_pca <- run.pca.with.no.na(columns, data)
```

Note here we could just run this in a loop if we create a list of columns:

```{r, eval = FALSE}
## List of columns
list_of_columns <- list(first_set  = c(5:8,11,16,17),
                        second_set = c(5:8,11,13,16,17,18),
                        third_set  = c(5:8,11))

## Placeholder for the pcas
list_of_pcas <- list()

## Looping through each pca
for(one_set in 1:length(list_of_columns)) {
    ## Doing one pca
    list_of_pcas[[one_set]] <- run.pca.with.no.na(list_of_columns[[one_set]], data)
}
```

Oh but wait...

#### For any other kind of loop use `lapply`. Just do it.

Again, once you have the logic, you can easily forget about `for` loops, we've seen how to use `apply` for matrices.
Well there's an even easier function for lists: `lapply`:

```{r, eval = FALSE} 
## Running the whole script in one line:
list_of_pcas <- lapply(list_of_columns, run.pca.with.no.na, data)
```

And this is all just much nicer to read compare to the rainbow barf that I wrote in 2011!


# Functionalising from the start:

Design code as functions and objects from start: describe what your functions are supposed to do and what kind of objects you will need.
This will make everything smooth and efficient from scratch!

Here's a fun project for those that want to try it: https://github.com/TGuillerme/TaxoMatch3000