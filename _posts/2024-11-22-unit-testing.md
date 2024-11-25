---
author: tguillerme
layout: page
title:  "Unit testing"
teaser: "Test your code to make sure it's doing what it's doing"
categories:
    - session
tags:
    - worshop
    - coding
    - R
---

# Heard of unit testing?

Someone told to me when asking about unit testing:

> "You don't need to do unit testing. Just ensure your code is correct the first time you write it, like me. I believe my approach is called 'agile development'."

This illustrates really well the point of unit testing in an ironic way: it allows you to ensure your code is correct throughout it's life cycle and gives you a stronger quality check on your code.

I think when most people develop code, they draft some unit tests anyways.
I.e. most people type their code in some script, check if it does the correct expected output in the console and continue from there.
Well, in a way, I think that this is already the main step of unit testing: basically just checking that what you're typing is giving the expected results (e.g. if you change the colour in a plot, does the plot colours change?).
Unit testing is just a way to keep track of these changes and formalises a way to always run these tests.
Like most things in code development, there is a bit of friction at the start but it gets really easy and becomes natural the more you do it (i.e. there is a learning curve that you will go through through time).

# When you write a project, how do you know your code does what it does?

At the core of unit testing there is the idea of making sure you know your code does what it wants you to do.
I have no doubt that the person quoted before uses coding in a correct way and doesn't make more mistakes than others.
But how can they be sure that their code does what it says it does?

For example, if you have a very simple function adding to numbers together, it is pretty straightforward to be confident of its behaviour (e.g. the numbers add up).
But have you thought of what the behaviour of the function should be if you're adding a number to something else?
E.g. `1 + 1 = 2` but what about `1 + "a" = ?` `1 + matrix(1) = ?`.

## Unit testing globally

Globally this can be done by formally deciding what are the expected cases, usages and outputs of your code.
This can be done informally when developing your code (e.g. if you functionalise, make sure to also think what your function's outputs should, can and cannot be).
For example, if I have additions in my code, I want to be sure they do the correct thing:

```
## This should be correct
1 + 1 = 2
```

But it also allows you to think about some more corner cases, there is no wrong answer here.

```
## What do _I_ expect to happen here:
1 + NA = 1
1/0 = NA
...
```

You can easily integrate this in any of your scripts, either as commented out sections

```
## Do something fancy
...

## Make sure additions work
# 1 + 1 == 2
# is.na(1 + NA)
```

Or in a more formal way with logicals:

```
## Do something fancy
...

## Make sure additions work
test <- 1 + 1 == 2
if(test == FALSE) {
    stop("additions don't work!")
}

test <- is.na(1 + NA)
if(test == FALSE) {
    stop("additions don't work!")
}
```

## Unit testing in `R`

Or, a better option, in `R` is to use the formal package `testthat` which will automatically run tests after you've designed them.
Say we want to test this ultimately complex function:

```
mega.complicated <- function(a, b) {
    do.call(`+`, list(a = a, b = b))
}
```

We can create a test chunk using the `test_that` wrapping function that contains a series of expectations (e.g. `expect_equal`, `expect_true`, etc...)

```
## Loading the library
library(testthat)

## Running a test chunk
test_that("my function makes sense", {
    ## 1 + 1 is 2
    expect_equal(mega.complicated(1, 1), 2)
    ## 1 + NA is N
    expect_true(is.na(mega.complicated(1, NA)))
    })
```

If everything goes well, the chunk should just run silently (your additions work).
If something goes wrong, it should print an error message.

In a big project, like a package, you can formalise that into a specific `tests/testthat/` folder that contains all your different tests, usually in different files like `test-function1.R`, `test-function2.R`.
Here is [an example](https://github.com/TGuillerme/dispRity/tree/master/tests/testthat) from the `dispRity` package.
