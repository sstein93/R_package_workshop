# Packaging your code

## Learning Objectives

-   Explain the benefits of turning frequently used bits of code into package functions.

## What is a package?

An R package is a collection of functions that are bundled together in a way that lets them be easily reused and shared. Usually these functions are designed to work together to complete a specific task such as analyzing a particular kind of data. You are probably familiar with many packages already, for example `ggplot2` or `dplyr`.

For more details, look at Hadley Wickham's R packages book: <https://r-pkgs.org/introduction.html>

## Why write an R package?

Packages are the best way to distribute code and documentation. Even if you don't plan on sharing your code or package, it is still useful to have a place to store your commonly used functions. You may have heard the advice that you should turn code you frequently reuse into functions so you don't have to keep rewriting them. This also applied to functions! If you have functions you reuse in different projects, then putting them into a package will save you time in the long run since you won't have to copy and paste functions between projects or keep track of different versions of the same function.

## Packages for writing packages

Here are some helpful R packages that will make writing R packages easier:

-   `devtools` contains functions that help with checking, building, and installing packages.
-   `usethis` contains a range of templates and handy functions that used to be part of `devtools`
-   `roxygen2` contains functions to help with function documentation
-   `testthat` contains functions for writing unit tests
-   `knitr` helps to build vignettes

Please install these packages now.

```         
pkgs <- c("devtools", "usethis", "roxygen2", "testthat", "knitr")
install.packages(pkgs)
```

## Other requirements

You will need a recent version of R and RStudio. This was written using R 4.3.1. You can download R from <https://cloud.r-project.org/> and RStudio from <https://www.rstudio.com/products/rstudio/download/>.

------------------------------------------------------------------------

This is adapted from <https://combine-australia.github.io/r-pkg-dev/introduction.html>
