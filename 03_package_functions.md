# Functions

## Learning objectives

-   Write an R function and add it to your package
-   Checking your package to make sure it is correct
-   Writing roxygen compatible function documentation

## Adding a function

All functions should be the `R/` subdirectory of the R package. You can organize functions into files within your package however you'd like. However, it's generally a good idea to group similar functions into one file with a descriptive file name.

As an example, we are going to write a function that takes a vector of numerical values and returns summary statistics including the mean, median, standard deviation, min, and max:

We are going to put our function into an `.R` file called `summarize_data.R`. We can make this file manually, or use `usethis` to make the file under `R/`

```         
usethis::use_r("summarize_data")
```

You should see output like this:

```         
✔ Setting active project to '/path/to/dataexplore'
• Modify 'R/summarize_data.R'
```

Notice the green checkmark before setting the active project to the R package directory, and the red dot before the second statement, "modify 'R/summarize_data.R'". This is because you have to manually add the function to summarize_data.R!

Let's do that now. Add the following function to 'R/summarize_data.R':

```         
summarize_vector <- function(x){
  mean_val <- mean(x) 
  median_val <- median(x)
  sd_val <- sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

## Using the function

Now that we have a function we want to see if it works. Usually when we write a new function we load it by copying the code to the console or sourcing the R file. When we are developing a package we want to try and keep our environment empty so that we can be sure we are only working with objects inside the package. Instead we can load functions using `devtools::load_all()`.

You should see the output

```         
ℹ Loading dataexplore
```

This confirms that our package has been loaded into our environment!

Note: if you want to load this package into a different project directory, you can use `devtools::load_all("/path/to/dataexplore")` . This is similar to using `library(R_package_name)` to load an R package.

The function doesn't appear in the environment, just like all the functions in a package don't appear in the environment when we load it using library(). But if we try to use it the function should work.

```         
# generate a vector of random numbers from a normal distribution
x <- rnorm(100, 0, 10)
summarize_vector(x)
```

You should see this output:

```         
> x <- rnorm(100, 0, 10)
> summarize_vector(x)
$mean
[1] 0.1095912

$median
[1] 1.050597

$sd
[1] 11.01259

$min
[1] -30.79162

$max
[1] 26.48239
```

## Checking your package

There are multiple requirements for a package to be considered "correct". These can be hard to keep track of, so you can run `devtools::check()`, which automatically builds and checks your package against devtools's best practices.

Note: passing this check is essential if you want to upload your package to CRAN.

```         
devtools::check()
```

You should see output like this:

```         
> devtools::check()
══ Documenting ════════════════════════════════════════════════════════════════
ℹ Updating dataexplore documentation
ℹ Loading dataexplore

══ Building ═══════════════════════════════════════════════════════════════════
Setting env vars:
• CFLAGS    : -Wall -pedantic -fdiagnostics-color=always
• CXXFLAGS  : -Wall -pedantic -fdiagnostics-color=always
• CXX11FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX14FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX17FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX20FLAGS: -Wall -pedantic -fdiagnostics-color=always
── R CMD build ────────────────────────────────────────────────────────────────
✔  checking for file ‘/Users/shaynastein/Documents/gitrepos/dataexplore/DESCRIPTION’ ...
─  preparing ‘dataexplore’:
✔  checking DESCRIPTION meta-information ...
─  checking for LF line-endings in source and make files and shell scripts
─  checking for empty or unneeded directories
─  building ‘dataexplore_0.0.0.9000.tar.gz’
   
══ Checking ═══════════════════════════════════════════════════════════════════
Setting env vars:
• _R_CHECK_CRAN_INCOMING_REMOTE_               : FALSE
• _R_CHECK_CRAN_INCOMING_                      : FALSE
• _R_CHECK_FORCE_SUGGESTS_                     : FALSE
• _R_CHECK_PACKAGES_USED_IGNORE_UNUSED_IMPORTS_: FALSE
• NOT_CRAN                                     : true
── R CMD check ────────────────────────────────────────────────────────────────
─  using log directory ‘/private/var/folders/zx/70jh6_x90yl3g_mmrpm9hr280000gn/T/Rtmpw5ROqR/file1b96433348d9/dataexplore.Rcheck’
─  using R version 4.3.1 (2023-06-16)
─  using platform: aarch64-apple-darwin20 (64-bit)
─  R was compiled by
       Apple clang version 14.0.0 (clang-1400.0.29.202)
       GNU Fortran (GCC) 12.2.0
─  running under: macOS Ventura 13.1
─  using session charset: UTF-8
─  using options ‘--no-manual --as-cran’
✔  checking for file ‘dataexplore/DESCRIPTION’
─  this is package ‘dataexplore’ version ‘0.0.0.9000’
─  package encoding: UTF-8
✔  checking package namespace information ...
✔  checking package dependencies ...
✔  checking if this is a source package ...
✔  checking if there is a namespace
✔  checking for executable files ...
✔  checking for hidden files and directories
✔  checking for portable file names
✔  checking for sufficient/correct file permissions
✔  checking serialization versions
✔  checking whether package ‘dataexplore’ can be installed (616ms)
✔  checking installed package size
✔  checking package directory
✔  checking for future file timestamps ...
✔  checking DESCRIPTION meta-information ...
✔  checking top-level files ...
✔  checking for left-over files
✔  checking index information
✔  checking package subdirectories ...
✔  checking R files for non-ASCII characters ...
✔  checking R files for syntax errors ...
✔  checking whether the package can be loaded ...
✔  checking whether the package can be loaded with stated dependencies ...
✔  checking whether the package can be unloaded cleanly ...
✔  checking whether the namespace can be loaded with stated dependencies ...
✔  checking whether the namespace can be unloaded cleanly ...
✔  checking loading without being on the library search path ...
✔  checking dependencies in R code ...
✔  checking S3 generic/method consistency ...
✔  checking replacement functions ...
✔  checking foreign function calls ...
N  checking R code for possible problems (1.3s)
   summarize_vector: no visible global function definition for ‘median’
   summarize_vector: no visible global function definition for ‘sd’
   Undefined global functions or variables:
     median sd
   Consider adding
     importFrom("stats", "median", "sd")
   to your NAMESPACE file.
✔  checking Rd files ...
✔  checking Rd metadata ...
✔  checking Rd line widths ...
✔  checking Rd cross-references ...
✔  checking for missing documentation entries ...
✔  checking for code/documentation mismatches ...
✔  checking Rd \usage sections ...
✔  checking Rd contents ...
✔  checking for unstated dependencies in examples ...
✔  checking examples ...
✔  checking for non-standard things in the check directory
✔  checking for detritus in the temp directory
   
   See
     ‘/private/var/folders/zx/70jh6_x90yl3g_mmrpm9hr280000gn/T/Rtmpw5ROqR/file1b96433348d9/dataexplore.Rcheck/00check.log’
   for details.
   
   
── R CMD check results ──────────────────────────────────────────────── dataexplore 0.0.0.9000 ────
Duration: 5.3s

❯ checking R code for possible problems ... NOTE
  summarize_vector: no visible global function definition for ‘median’
  summarize_vector: no visible global function definition for ‘sd’
  Undefined global functions or variables:
    median sd
  Consider adding
    importFrom("stats", "median", "sd")
  to your NAMESPACE file.

0 errors ✔ | 0 warnings ✔ | 1 note ✖
```

You can see all the different types of checks that `devtools` has run. The section at the end where it tells you how many errors, warnings and notes there are:

-   Errors happen when you code has broken and failed one of the checks. If errors are not fixed your package will not work correctly.

-   Warnings are slightly less serious but should also be addressed. Your package will probably work without fixing thise but it is highly advised that you do.

-   Notes are advice rather than problems. It can be up to you whether or not to address them but there is usually a good reason to.

We have a note about undefined global functions, and it suggests we specify that the package imports the functions `median` and `sd` from the stats package. This is particularly important if your function relies on a package that is not automatically installed and loaded at the beginning of an R session, and we will cover imports and dependencies in a later section.

## Documentation

Documenting functions is crucial for writing shareable and interpretable code. It will help you remember what your code does in the future, and it will help other people use your code. R package documentation is very easy to do using the package `roxygen2`. If we include special comments at the start of each function, roxygen will translate that into the complicated syntax that R uses for package documentation.

### Writing documentation

To insert roxygen style documentation skeleton at the top of your function, press *Ctrl+Alt+Shift+R* (Windows) or *Cmd+Opt+Shift+R* (Mac). The function should now look like this:

```         
#' Title
#'
#' @param x 
#'
#' @return
#' @export
#'
#' @examples
summarize_vector <- function(x){
  mean_val <- mean(x)
  median_val <- median(x)
  sd_val <- sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

Notice that all of the Roxygen comments start with `#'` . This first line is the title of the function, followed by a blank line where you can add a sentence or paragraph giving a more detailed description of the function:

```         
#' Summarize Vector
#'
#' Given a vector 'x', calculate summary statistics mean, median, 
#' standard deviation, min, and max.
#'
#' @param x 
#'
#' @return
#' @export
#'
#' @examples
summarize_vector <- function(x){
  mean_val <- mean(x)
  median_val <- median(x)
  sd_val <- sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

The next section describes the function parameters, marked by the '\@param' field. All we need to do is provide a description:

```         
#' Summarize Vector
#'
#' Given a vector 'x', calculate summary statistics mean, median, 
#' standard deviation, min, and max.
#'
#' @param x A vector of numeric values
#'
#' @return
#' @export
#'
#' @examples
summarize_vector <- function(x){
  mean_val <- mean(x)
  median_val <- median(x)
  sd_val <- sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

The `@return` field specifies what the function returns, in this case a list containing the summary statistics

```         
#' Summarize Vector
#'
#' Given a vector 'x', calculate summary statistics mean, median, 
#' standard deviation, min, and max.
#'
#' @param x A vector of numeric values
#'
#' @return A list containing the summary statistics mean, median, 
#' standard deviation, min, and max
#'
#' @export
#'
#' @examples
summarize_vector <- function(x){
  mean_val <- mean(x)
  median_val <- median(x)
  sd_val <- sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

`@export` is on the next line. This field doesn't add documentation to the help pages, instead it modifies the NAMESPACE file. Including `\@export` tells Roxygen that this is a function that should be available to the package user and so it should be added to the NAMESPACE file. If we wanted this to be an internal function that isn't meant to be used by a user, we would leave out `@export`. Internal functions can be useful to handle small processes that package functions rely on but an outside user wouldn't call separately.

The last field is `@examples`, which is where we can include examples showing how to use the function. These will be included in the help file and will be run using `example("function")`.

```         
#' Summarize Vector
#'
#' Given a vector 'x', calculate summary statistics mean, median, 
#' standard deviation, min, and max.
#'
#' @param x A vector of numeric values
#'
#' @return A list containing the summary statistics mean, median, 
#' standard deviation, min, and max
#'
#' @export
#'
#' @examples
#' # summarize a vector of random normals
#' x <- rnorm(100, 0, 10)
#' summarize_vector(x)
#'
summarize_vector <- function(x){
  mean_val <- mean(x)
  median_val <- median(x)
  sd_val <- sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

### Building documentation

Now we can build the function documentation using `devtools`:

```         
devtools::document()
```

You should see this output:

```         
> devtools::document()
ℹ Updating dataexplore documentation
ℹ Loading dataexplore
Writing NAMESPACE
Writing summarize_vector.Rd
```

You can see in the output that devtools has done two things:

First, it has updated the NAMESPACE file. Your NAMESPACE file should look like this:

```         
# Generated by roxygen2: do not edit by hand

export(summarize_vector)
```

This tells us that the `summarize_vector` function is exported by the package.

The second thing `devtools` has done is create a file called `summarize_vector.Rd` in the `man` directory. `.Rd` stands for "R Documentation", and this is the help file for the function. It should look like this:

```         
% Generated by roxygen2: do not edit by hand
% Please edit documentation in R/summarize_data.R
\name{summarize_vector}
\alias{summarize_vector}
\title{Summarize Vector}
\usage{
summarize_vector(x)
}
\arguments{
\item{x}{A vector of numeric values}
}
\value{
A list containing the summary statistics mean, median,
standard deviation, min, and max
}
\description{
Given a vector 'x', calculate summary statistics mean, median,
standard deviation, min, and max.
}
\examples{
# summarize a vector of random normals
x <- rnorm(100, 0, 10)
summarize_vector(x)

}
```

Now we can load our package again:

```         
devtools::load_all()
```

You should see this output:

```         
> devtools::load_all()
ℹ Loading dataexplore
```

Now, you can do `?summarize_vector` to get the help page for the `summarize_vector` function, as you would any other R function from a package:

![](images/Screenshot%202025-06-12%20at%207.36.41%20PM.png)

## Dependencies

Remember when we ran `devtools::check()` we got a warning about there being no visible global function for `sd` and `median`. We can add those function dependencies now using `usethis`:

```         
> usethis::use_package("stats")
✔ Adding 'stats' to Imports field in DESCRIPTION
• Refer to functions with `stats::fun()`
```

This updated our DESCRIPTION file to have `stats` in the Imports field:

```         
Package: dataexplore
Title: What the Package Does (One Line, Title Case)
Version: 0.0.0.9000
Authors@R: 
    person("First", "Last", , "first.last@example.com", role = c("aut", "cre"),
           comment = c(ORCID = "YOUR-ORCID-ID"))
Description: What the package does (one paragraph).
License: MIT + file LICENSE
Encoding: UTF-8
Roxygen: list(markdown = TRUE)
RoxygenNote: 7.2.3
Imports: 
    stats
```

This tells us that our package uses functions from the `stats` package. Let's run `devtools::check()` one more time:

```         
> devtools::check()
══ Documenting ════════════════════════════════════════════════════════════════════════════════════
ℹ Updating dataexplore documentation
ℹ Loading dataexplore

══ Building ═══════════════════════════════════════════════════════════════════════════════════════
Setting env vars:
• CFLAGS    : -Wall -pedantic -fdiagnostics-color=always
• CXXFLAGS  : -Wall -pedantic -fdiagnostics-color=always
• CXX11FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX14FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX17FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX20FLAGS: -Wall -pedantic -fdiagnostics-color=always
── R CMD build ────────────────────────────────────────────────────────────────────────────────────
✔  checking for file ‘/Users/shaynastein/Documents/gitrepos/dataexplore/DESCRIPTION’ ...
─  preparing ‘dataexplore’:
✔  checking DESCRIPTION meta-information ...
─  checking for LF line-endings in source and make files and shell scripts
─  checking for empty or unneeded directories
─  building ‘dataexplore_0.0.0.9000.tar.gz’
   
══ Checking ═══════════════════════════════════════════════════════════════════════════════════════
Setting env vars:
• _R_CHECK_CRAN_INCOMING_REMOTE_               : FALSE
• _R_CHECK_CRAN_INCOMING_                      : FALSE
• _R_CHECK_FORCE_SUGGESTS_                     : FALSE
• _R_CHECK_PACKAGES_USED_IGNORE_UNUSED_IMPORTS_: FALSE
• NOT_CRAN                                     : true
── R CMD check ────────────────────────────────────────────────────────────────────────────────────
─  using log directory ‘/private/var/folders/zx/70jh6_x90yl3g_mmrpm9hr280000gn/T/Rtmpw5ROqR/file1b96cf2daf4/dataexplore.Rcheck’
─  using R version 4.3.1 (2023-06-16)
─  using platform: aarch64-apple-darwin20 (64-bit)
─  R was compiled by
       Apple clang version 14.0.0 (clang-1400.0.29.202)
       GNU Fortran (GCC) 12.2.0
─  running under: macOS Ventura 13.1
─  using session charset: UTF-8
─  using options ‘--no-manual --as-cran’
✔  checking for file ‘dataexplore/DESCRIPTION’
─  this is package ‘dataexplore’ version ‘0.0.0.9000’
─  package encoding: UTF-8
✔  checking package namespace information ...
✔  checking package dependencies (460ms)
✔  checking if this is a source package ...
✔  checking if there is a namespace
✔  checking for executable files ...
✔  checking for hidden files and directories ...
✔  checking for portable file names
✔  checking for sufficient/correct file permissions
✔  checking serialization versions
✔  checking whether package ‘dataexplore’ can be installed (590ms)
✔  checking installed package size
✔  checking package directory
✔  checking for future file timestamps (585ms)
✔  checking DESCRIPTION meta-information ...
✔  checking top-level files ...
✔  checking for left-over files
✔  checking index information
✔  checking package subdirectories ...
✔  checking R files for non-ASCII characters ...
✔  checking R files for syntax errors ...
✔  checking whether the package can be loaded ...
✔  checking whether the package can be loaded with stated dependencies ...
✔  checking whether the package can be unloaded cleanly ...
✔  checking whether the namespace can be loaded with stated dependencies ...
✔  checking whether the namespace can be unloaded cleanly ...
✔  checking loading without being on the library search path ...
N  checking dependencies in R code ...
   Namespace in Imports field not imported from: ‘stats’
     All declared Imports should be used.
✔  checking S3 generic/method consistency ...
✔  checking replacement functions ...
✔  checking foreign function calls ...
N  checking R code for possible problems (1.2s)
   summarize_vector: no visible global function definition for ‘median’
   summarize_vector: no visible global function definition for ‘sd’
   Undefined global functions or variables:
     median sd
   Consider adding
     importFrom("stats", "median", "sd")
   to your NAMESPACE file.
✔  checking Rd files ...
✔  checking Rd metadata ...
✔  checking Rd line widths ...
✔  checking Rd cross-references ...
✔  checking for missing documentation entries ...
✔  checking for code/documentation mismatches ...
✔  checking Rd \usage sections ...
✔  checking Rd contents ...
✔  checking for unstated dependencies in examples ...
✔  checking examples ...
✔  checking for non-standard things in the check directory
✔  checking for detritus in the temp directory
   
   See
     ‘/private/var/folders/zx/70jh6_x90yl3g_mmrpm9hr280000gn/T/Rtmpw5ROqR/file1b96cf2daf4/dataexplore.Rcheck/00check.log’
   for details.
   
   
── R CMD check results ──────────────────────────────────────────────── dataexplore 0.0.0.9000 ────
Duration: 5.5s

❯ checking dependencies in R code ... NOTE
  Namespace in Imports field not imported from: ‘stats’
    All declared Imports should be used.

❯ checking R code for possible problems ... NOTE
  summarize_vector: no visible global function definition for ‘median’
  summarize_vector: no visible global function definition for ‘sd’
  Undefined global functions or variables:
    median sd
  Consider adding
    importFrom("stats", "median", "sd")
  to your NAMESPACE file.

0 errors ✔ | 0 warnings ✔ | 2 notes ✖
```

We are still getting 2 notes:

-   The first is that there is a namespace `stats` that we are not importing form. This means that we are importing a package with our function but not using it in any of our functions.

-   The second is the same note from before saying that there is no visible global function or variable for `median` or `sd` .

We are getting these because we forgot to follow the instructions to refer to functions with `stats::fun()` from `usethis::stats()`:

```         
> usethis::use_package("stats")
✔ Adding 'stats' to Imports field in DESCRIPTION
• Refer to functions with `stats::fun()`
```

Let's namespace our functions accordingly:

```         
#' Summarize Vector
#'
#' Given a vector 'x', calculate summary statistics mean, median,
#' standard deviation, min, and max.
#'
#' @param x A vector of numeric values
#'
#' @return A list containing the summary statistics mean, median,
#' standard deviation, min, and max
#'
#' @export
#'
#' @examples
#' # summarize a vector of random normals
#' x <- rnorm(100, 0, 10)
#' summarize_vector(x)
#'
summarize_vector <- function(x){
  mean_val <- mean(x)
  median_val <- stats::median(x)
  sd_val <- stats::sd(x)
  min_val <- min(x)
  max_val <- max(x)
  return(list(mean = mean_val, median = median_val, sd = sd_val, min = min_val, max = max_val))
}
```

Let's run `devtools::check()` one more time:

```         
> devtools::check()
══ Documenting ════════════════════════════════════════════════════════════════════════════════════
ℹ Updating dataexplore documentation
ℹ Loading dataexplore

══ Building ═══════════════════════════════════════════════════════════════════════════════════════
Setting env vars:
• CFLAGS    : -Wall -pedantic -fdiagnostics-color=always
• CXXFLAGS  : -Wall -pedantic -fdiagnostics-color=always
• CXX11FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX14FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX17FLAGS: -Wall -pedantic -fdiagnostics-color=always
• CXX20FLAGS: -Wall -pedantic -fdiagnostics-color=always
── R CMD build ────────────────────────────────────────────────────────────────────────────────────
✔  checking for file ‘/Users/shaynastein/Documents/gitrepos/dataexplore/DESCRIPTION’ ...
─  preparing ‘dataexplore’:
✔  checking DESCRIPTION meta-information ...
─  checking for LF line-endings in source and make files and shell scripts
─  checking for empty or unneeded directories
─  building ‘dataexplore_0.0.0.9000.tar.gz’
   
══ Checking ═══════════════════════════════════════════════════════════════════════════════════════
Setting env vars:
• _R_CHECK_CRAN_INCOMING_REMOTE_               : FALSE
• _R_CHECK_CRAN_INCOMING_                      : FALSE
• _R_CHECK_FORCE_SUGGESTS_                     : FALSE
• _R_CHECK_PACKAGES_USED_IGNORE_UNUSED_IMPORTS_: FALSE
• NOT_CRAN                                     : true
── R CMD check ────────────────────────────────────────────────────────────────────────────────────
─  using log directory ‘/private/var/folders/zx/70jh6_x90yl3g_mmrpm9hr280000gn/T/Rtmpw5ROqR/file1b96303ab7e8/dataexplore.Rcheck’
─  using R version 4.3.1 (2023-06-16)
─  using platform: aarch64-apple-darwin20 (64-bit)
─  R was compiled by
       Apple clang version 14.0.0 (clang-1400.0.29.202)
       GNU Fortran (GCC) 12.2.0
─  running under: macOS Ventura 13.1
─  using session charset: UTF-8
─  using options ‘--no-manual --as-cran’
✔  checking for file ‘dataexplore/DESCRIPTION’
─  this is package ‘dataexplore’ version ‘0.0.0.9000’
─  package encoding: UTF-8
✔  checking package namespace information ...
✔  checking package dependencies (400ms)
✔  checking if this is a source package ...
✔  checking if there is a namespace
✔  checking for executable files ...
✔  checking for hidden files and directories
✔  checking for portable file names
✔  checking for sufficient/correct file permissions
✔  checking serialization versions
✔  checking whether package ‘dataexplore’ can be installed (602ms)
✔  checking installed package size ...
✔  checking package directory
✔  checking for future file timestamps (376ms)
✔  checking DESCRIPTION meta-information ...
✔  checking top-level files ...
✔  checking for left-over files
✔  checking index information
✔  checking package subdirectories ...
✔  checking R files for non-ASCII characters ...
✔  checking R files for syntax errors ...
✔  checking whether the package can be loaded ...
✔  checking whether the package can be loaded with stated dependencies ...
✔  checking whether the package can be unloaded cleanly ...
✔  checking whether the namespace can be loaded with stated dependencies ...
✔  checking whether the namespace can be unloaded cleanly ...
✔  checking loading without being on the library search path ...
✔  checking dependencies in R code ...
✔  checking S3 generic/method consistency ...
✔  checking replacement functions ...
✔  checking foreign function calls ...
✔  checking R code for possible problems (1.1s)
✔  checking Rd files ...
✔  checking Rd metadata ...
✔  checking Rd line widths ...
✔  checking Rd cross-references ...
✔  checking for missing documentation entries ...
✔  checking for code/documentation mismatches ...
✔  checking Rd \usage sections ...
✔  checking Rd contents ...
✔  checking for unstated dependencies in examples ...
✔  checking examples ...
✔  checking for non-standard things in the check directory
✔  checking for detritus in the temp directory
   
   
── R CMD check results ──────────────────────────────────────────────── dataexplore 0.0.0.9000 ────
Duration: 5.1s

0 errors ✔ | 0 warnings ✔ | 0 notes ✔
```

We passed without any warnings or notes :)
