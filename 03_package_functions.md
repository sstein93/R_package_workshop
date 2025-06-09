# Functions

## Learning objectives

-   Write an R function and add it to your package
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

