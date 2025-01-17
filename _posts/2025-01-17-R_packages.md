---
author: tguillerme
layout: page
title:  "R packages"
teaser: "Wrapping your code into R packages makes reproducibility easier, especially for future you!"
categories:
    - session
tags:
    - workshop
    - R
    - packages
---


# Making R packages

It's super easy!

download this R markdown file [here](https://github.com/macrofun-sheffield/macrofun-sheffield.github.io/blob/main/_posts/2025-01-17-R_packages.md)

## Why packaging your R code?

 * Writing your code for sharing with other users
 * It's easy (really)
 * It makes your coding more tidy (one package per project)
 * It makes your code more portable (more reuse/citations)
 * It makes it easier to reuse your code for future projects (it has proper documentation!)
 * It can be the start of a real package (you never know where you project will expand)

## What you'll need

 * One function (or more)
 * One folder (or more)
 * One idea (or more)
 * `devtools` and `roxygen2` packages

```
## Load them packages!
library("devtools")
library("roxygen2")
```

# Cooking instructions

Make sure you're in the right directory and of you go!
Most of the tutorial part on making R packages was taken from the [Not so standard deviation](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/) blog.
Check it out, it's explained way better than this note!

## Initialise the package

R packages have a typical format (folders with fixed names) and typical files (`DESCRIPTION` and `NAMESPACE`).
You can do all that by hand but eh, why bother, just run the following and `devtools` deals with everything for you!

```
## Create a package with an inspiring name (keep it short!)
create("fakeR")
```

### The `DESCRIPTION` file

The `create()` generates a file called `DESCRIPTION`, this is the most important file in your folder since it is this one that defines the existence of your package.
Luckily, `devtools` generates it all for you so you can just edit the text in there to write down the info on your package (what it does, who made it, etc...).

## Get yourself a nice function

Let's write a complex function:

```
## A really complex function
average <- function(vector) {
    return(sum(vector)/length(vector))
}
```

And save it in a specific file in `R/` (for example `average.R`).

## Document your function

A great thing about packaging your code is to document it in a consistent way.
This way, whenever you go back to it after a couple of years, you can simply go `?average` to remember what the hell where you thinking back then...
The documentation is made super easy by the `roxygen` package: it recognises function manual tags as `#'` and some standard manual formats (e.g. `@title` will format the text as a title, etc...).
Note that only lines starting with `#'` are interpreted by `roxygen`; you can still comment your code normally using `#`.
One other important thing is the `@export` tag that will export the function (we'll see that later)

```
#' @title Average
#'
#' @description Does some complex math to get an average value
#'
#' @param vector A numeric vector
#' 
#' @return A numeric value
#' 
#' @examples
#' average(rnorm(10))
#' 
#' @author Thomas Guillerme
#' @export

## A really complex function
average <- function(vector) {
    return(sum(vector)/length(vector))
}
```

## Compile the package

Within your package folder, you can first create your documentation:

```
setwd("fakeR/")
document()
```

And then compile and install the whole package:

```
install()
```

Check it out:

```
library(fakeR)
## Wow!
average(c(1,2,3))
## WOWOWOWOW!
```

### The `NAMESPACE` file

The `document()` generates a file called `NAMESPACE`, this is a crucial file for the package since it contains the functions that will be imported in the R environment when you will load the package.
Thankfully, `roxygen` and `devtools` take care of it for you so if you feel uncomfortable with this file, let it just live it's life and it should never bother you.
Alternatively you can write it/edit it yourself but you'll have to be careful and thorough!

The importance of this file will make more sense after introducing the internal functions.

## Internal functions

These are functions that shouldn't be accessed by the user but are still loaded in the R environment.
Classically, these are support functions for your main functions (that can be called by the user).
For example, we could mofidy the `average.R` function to add some sanitising function for checking if the input is in the correct format.

We can create a `check.vector` function that we will store in a `sanitising.R` file:

```
## Check if an argument is a numeric vector
check.vector <- function(vector) {
    if(class(vector) != "numeric") {
        stop("Your vector is not numeric!")
    }
}
```

Note that we don't need a `roxygen` type manual and no `export` since this won't be seen by the user.

We can then edit the `average.R` function to include this function (again users don't need to access `check.vector`):

```
#' @title Average
#'
#' @description Does some complex math to get an average value
#'
#' @param vector A numeric vector
#' 
#' @return A numeric value
#' 
#' @examples
#' average(rnorm(10))
#' 
#' @author Thomas Guillerme
#' @export

## A really complex function
average <- function(vector) {
    ## Checking the vector
    check.vector(vector)
    ## Calculating the average
    return(sum(vector)/length(vector))
}
```

And you can then recompile all the yoke:

```
document()
install()
```

And check it:

```
library(fakeR)
average(c(1,2,3))
average(c("a", "b", "c"))
## This one is not happy...
```


## Dependencies

Of course, you're not going to reinvent the wheel each time!
You can use functions from other packages to make life easier.
So for that, there's a little trick: the idea being that you could load a lot of libraries each time but that might give your R session a big old RAM footprint!

The trick is to call functions by using their package's name.
For example `t.test` becomes `stats::t.test` (because it's in the `stats` package).
These dependencies also need to be exported to the `NAMESPACE` but we can take advantage of `roxygen` awesomeness and just use the `#' @importFrom` tags (see at the bottom of the file).


```
#' @title Super significant t.test
#'
#' @description Always make sure your results are significant
#'
#' @param X A distribution 
#' @param Y Another distribution
#' 
#' @return A significant t-test!
#' 
#' @examples
#' super.t.test(rnorm(50), rnorm(50))
#' 
#' @author Thomas Guillerme
#' @export
super.t.test <- function(X, Y) {

    ## Run the test
    test_results <- stats::t.test(X, Y)

    ## Check if the p-value is non-significant
    if(test_results$p.value > 0.05) {
        ## Cheat!
        test_results$p.value <- stats::runif(1, max = 0.05)
    }
    
    return(test_results)
}

#' @importFrom stats t.test
NULL

#' @importFrom stats runif
NULL
```

Recompile and check:

```
document()
install()
```

```
library(fakeR)

## Running the t.test
super.t.test(rnorm(50), rnorm(50))
## Hehehe!
```

## Sharing

Finally, you can upload your package to GitHub (and initiate proper version control).
This is super handy so you can load the package from everywhere using:

```
install_github("my_github/fakeR")
```

> Of course, you're not me so you might want to change the repository and user name...

That's it!
Of course, many rules can be bend to fit your specific needs.
The Google will be there to assist you for specific rule bending!

## Unit test

Well actually, that's not really it...
You can always go one step further and add a test suit to your package to make sure it works at its best every time.
This is more advanced computing stuff (yet easy) so I'll let you browse the [wikipedia](https://en.wikipedia.org/wiki/Unit_testing) page on it for more info.
In really brief, it tests your code every time you run it.
This helps, when you have a lot of interdependent code: for example if you change a bit of a function that's used by other functions, it makes sure the other functions' behavior is not affected.

You can initiate the testing using `test` function from `devtools` but you need to first install the testing package: `testthat`

```
install.packages("testthat")
## Initiate the testing
test()
```

This creates a folder call `test` containing another folder called `testthat`, that's where we're gonna write the tests...
Again, I'm not going to go in the philosophy of unit-testing (plenty of resources out there on the interwebs describing it better than me) but here's what we want to do in essence: we want to make sure that our function, when feeding specific inputs give back expected outputs.

For example, we can create our test file for our average function as follow (and saving it in a file called `test-average.R` in `test/testthat/`):

```
## Describing the context (so it prints on screen what it is testing)
context("average")

## Testing the behaviour
test_that("average works fine", {
    ## If the input is not numeric it should print an error
    expect_error(average(c("a", "b", "c")))
    ## If the input is numeric the output should be of class numeric as well
    expect_is(average(c(1,2,3)), "numeric")
    ## And this should be a single value
    expect_equal(length(average(c(1,2,3))), 1)
    ## That is equal to 2
    expect_equal(average(c(1,2,3)), 2)
})
```

This way, if we modify some aspect of the function or its dependencies, we can see if it breaks the test suit (bad!) or if it passes (good!).

To run the tests, simply use the `test` function:

```
test()
```

# Thoughts

## Things you need to think about (in my opinion)

 * Be consistent in your coding: make sure you come up with a method for naming variables and functions.
    For example functions can be always camel case (`myFunction`) or separated with dots (`my.function`) and variables always without spaces (`myvariable`) or separated with underscores (`my_variables`)
 * Give sensible names to things! In the age of auto-fill, there's no excuse for functions or variable called `x`, `f` or `my_fun`.
    And be careful of `i`, `data` or `test` they can mean anything.
    I use function/variable names like `get.p.values`/`p_values_matrix` and verbose iteration variables (`for (row in 1:nrow(matrix)) {print(matrix[row,])}`).
 * First functionality, then optimisation: make your code do what you want it to do before making it doing well.
 * Unit test, unit test, unit test. Always, as much as possible.

## Unit testing digression

Everybody has his own technique but personally I think that test driven development is the best.
The idea is to follow this pipeline:    

 1. Write down what your code should do (as a function)

```
## This function should return a matrix with as many rows as the input
## and values between 0 and 1.
```

 2. Write the test

```
## This function should return a matrix with as many rows as the input
## and values between 0 and 1.
test_that("my function does what it should", {
    expect_is(output, "matrix")
    expect_equal(nrow(output), length(input))
    expect_false(any(output > 1))
    expect_false(any(output < 0))
})
```
 
 3. And only then write the code

```
## My function
my.function <- function(input) {
    return(matrix(data = sample(c(0,1), replace = TRUE), nrow = length(input)))
}

## This function should return a matrix with as many rows as the input
## and values between 0 and 1.
output <- my.function(input)
test_that("my function does what it should", {
    expect_is(output, "matrix")
    expect_equal(nrow(output), length(input))
    expect_false(any(output > 1))
    expect_false(any(output < 0))
})
```

It makes you think about your code in an architectural way rather than in a code-as-you go way and often results in way more efficient/neat code (at least for me).
Also it helps you writing functions that are always tested!

## Lazy compiling

Personally I like making my life easier by creating a wrapping function that does all that for me (typically this function lives out of the package like in the [`.Rprofile` file](https://csgillespie.github.io/efficientR/3-3-r-startup.html#rprofile)). Something like:

```
refresh.fakeR <- function() 
{
    library(devtools)
    ## Setting the right path
    setwd("~/my_folder/")
    ## Installing and loading
    install("fakeR") ; library(fakeR)
    ## Get within the package
    cd("fakeR/")
    ## Test and document
    test() ; document()
}
```

## The `CRAN`

If you're writing your package for other users, you might be interested in submitting it to the `CRAN`.
Although `CRAN` packages are not especially better or worth that other ones, they have the advantage of being currated.
This means when there is changes to `R` or to your dependencies, if your package is broken, it will advertise you _before_ the updates and tell you to update your package or it'll be removed from `CRAN`.
This can sound annoying but it's a good way to keep your package alive and useful for other users!

To submit your package to the `CRAN` you need to adhere to their testing compliances.
They are also a bit strict and can seem annoying but again, it helps making sure your package works on every machine and is still working, even years in the future.

To run the `CRAN` tests, you need to compile it via your terminal using the following:

```
## Compiling the code as tar ball
R CMD build --resave-data fakeR

## Check the package
R CMD check --as-cran fakeR_*.tar.gz
```

If it passes with the following text at the end:

```
Status: 0 ERROR, 0 WARNING, 0 NOTEs
```

You can submit it to the `CRAN` via: https://cran.r-project.org/submit.html.
Your package will the be checked by a robot and, if then double checked by a human (one of the people actually developing `R`!) and you'll receive a list of precise emails telling you what's wrong or not.
And if you've done your developing neatly, it'll be officially published on the CRAN!
It might look daunting but it's cumulative: for small packages it's actually pretty easy but the more complex it becomes, the more complex the checks become!