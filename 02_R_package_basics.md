# Writing your first R package

## Learning objectives

-   Create an R package using `usethis`
-   Editing the DESCRIPTION file
-   Adding your first function to the package
-   Writing roxygen compatible function headers for documentation

## Creating an R package

First, open RStudio. If you currently have an R project open, close it now by clicking *File \> Close Project*.

Then, decide on a name for your package. Package names can only consist of letters, numbers, and dots (.), and must start with a letter. While all of these are allowed, it can be easier to remember if the package name consists only of lowercase letters. It can be tricky to change the name of a package later, so it's worth spending time thinking about.

The example code in this workshop will make a package named \`exploredata\`, which you can use as well if you want to follow along.

We will use the \`usethis::create_package()\` function to create the template for our package. All it needs is the path to the directory where we want to create the package. For example:

```         
usethis::create_package("~/path/to/dataexplore")
```

You will see some information printed to the console, like this:

```         
> usethis::create_package("~/Documents/gitrepos/dataexplore")
✔ Setting active project to '/Users/USER/Documents/gitrepos/dataexplore'
✔ Creating 'R/'
✔ Writing 'DESCRIPTION'
Package: dataexplore
Title: What the Package Does (One Line, Title Case)
Version: 0.0.0.9000
Authors@R (parsed):
    * First Last <first.last@example.com> [aut, cre] (YOUR-ORCID-ID)
Description: What the package does (one paragraph).
License: `use_mit_license()`, `use_gpl3_license()` or friends to
    pick a license
Encoding: UTF-8
Roxygen: list(markdown = TRUE)
RoxygenNote: 7.2.3
✔ Writing 'NAMESPACE'
✔ Writing 'dataexplore.Rproj'
✔ Adding '^dataexplore\\.Rproj$' to '.Rbuildignore'
✔ Adding '.Rproj.user' to '.gitignore'
✔ Adding '^\\.Rproj\\.user$' to '.Rbuildignore'
✔ Opening '/Users/USER/Documents/gitrepos/dataexplore/' in new RStudio session
✔ Setting active project to '<no active project>'
```

You will see something similar everytime you run a `usethis` command. Green ticks indicate a step has been completed correctly. A red dot means that `usethis` can't complete something and you might need to follow instructions to do it manually.

A new RStudio session with your R package should open, and you should see something like this

![](images/Screenshot%202025-06-07%20at%209.05.24%20PM.png)

-   `DESCRIPTION`: the metadata file for your package. It contains information about the package, including required imports and dependencies. We will fill this in as we develop the package.

-   `NAMESPACE`: This file describes the functions in our package. You shouldn't need to edit this file manually if you develop your package using `roxygen2` and `devtools`.

-   `R/`: This is the directory that contains all of our R code

Other files that `usethis` creates but aren't necessary for an R package:

-   `.gitignore` Useful for specifying files for git to ignore if you use git for version control (recommended)

-   `.Rbuildignore` Similar to the concept of .gitignore, this file is used to mark files that are in the directory but aren't part of the package and shouldn't be included when it's built. \`usethis\` will edit this for you, so you shouldn't need to edit it.

-   `dataexplore.Rproj`: The RStudio project file, which you don't need to worry about.

## Filling in the DESCRIPTION

The `DESCRIPTION` file contains all the package metadata, including what the package is called, what version it is, a description, who the authors are, what other packages it depends on etc. Open the `DESCRIPTION` file and you should see something like this:

```         
Package: dataexplore
Title: What the Package Does (One Line, Title Case)
Version: 0.0.0.9000
Authors@R: 
    person("First", "Last", , "first.last@example.com", role = c("aut", "cre"),
           comment = c(ORCID = "YOUR-ORCID-ID"))
Description: What the package does (one paragraph).
License: `use_mit_license()`, `use_gpl3_license()` or friends to pick a
    license
Encoding: UTF-8
Roxygen: list(markdown = TRUE)
RoxygenNote: 7.2.3
```

### Title and description

The package name is correctly set to the name we specified when creating the package, but we should update the title and description.

The title should be a single line in "Title Case" that explains what your package is. It should be plain text (no markup) and NOT end in a period.

The description is a paragraph that can go into more detail.

This information will be on the CRAN page for the package if you upload to CRAN. Check out some published R packages for some examples!

-   `roxygen2`: <https://cran.r-project.org/web/packages/roxygen2/index.html>

-   `devtools`: <https://cran.r-project.org/web/packages/devtools/index.html>

-   `usethis`: <https://cran.r-project.org/web/packages/usethis/index.html>

For example, we could write:

```         
Package: dataexplore
Title: Data Exploration Tools
Version: 0.0.0.9000
Authors@R: 
    person("First", "Last", email = "first.last@example.com", role = c("aut", "cre"),
           comment = c(ORCID = "YOUR-ORCID-ID"))
Description: This package contains useful functions for exploratory data analysis, including calculating summary statistics and plotting data distributions.
License: `use_mit_license()`, `use_gpl3_license()` or friends to pick a
    license
Encoding: UTF-8
Roxygen: list(markdown = TRUE)
RoxygenNote: 7.2.3
```

### Author

Use the `Authors@R` field to identify the package's author, and whom to contact if something goes wrong. This field is unusual because it contains executable R code rather than plain text.

```         
Authors@R: person("Count", "Dracula", email = "dracula@countingbats.com",
  role = c("aut", "cre"))
```

```         
person("Count", "Dracula", email = "dracula@countingbats.com", 
  role = c("aut", "cre"))
#> [1] "Count Dracula <dracula@countingbats.com> [aut, cre]"
```

This command says that Count Dracula is both the maintainer (`cre`) and author (`aut`) and that their email address is `dracula@countingbats.com`.

The `person()` required inputs are:

-   The name, specified by the first two arguments, `given` and `family`

-   The `email` address, which is only an absolute requirement for the maintainer.

-   One or more three letter codes specifying the `role`. These are the most important roles to know about:

    -   `cre`: the creator or maintainer, the person you should bother if you have problems. Despite being short for "creator", this is the correct role to use for the current maintainer, even if they are not the initial creator of the package.

    -   `aut`: authors

    -   `ctb`: contributors

    -   `cph`: copyright holder. This is used to list additional copyright holders who are not authors, typically companies, like an employer of one or more of the authors.

    -   `fnd`: funders.

### License

The last thing we will update now is the software license. The describes how our code can be used and without one people must assume that it can\'t be used at all! It is good to be as open and free as you can with your license to make sure your code is as useful to the community as possible. For this example we will use the MIT license which basically says the code can be used for any purpose and doesn\'t come with any warranties.

Now the DESCRIPTION file might look like:

```         
Package: dataexplore
Title: Data Exploration Tools
Version: 0.0.0.9000
Authors@R: 
    person("Count", "Dracula", email = "dracula@countingbats.com", role = c("aut", "cre"),
           comment = c(ORCID = "1234"))
Description: This package contains useful functions for exploratory data analysis, including calculating summary statistics and plotting data distributions.
License: `use_mit_license()`
Encoding: UTF-8
Roxygen: list(markdown = TRUE)
RoxygenNote: 7.2.3
```
