Peer-graded Assignment: Building an R Package
FARS Package Building R Packages
Introduction
The purpose of this assessment is for you to combine your skills of creating, writing, documenting, and testing an R package with releasing that package on GitHub. In this assessment you'll be taking the R files from Week 2's assessment about documentation and putting that file in an R package.

For this assessment you must

write a vignette to include in your package using knitr and R Markdown
write at least one test written using testthat
put your package on GitHub
set up the repository so that the package can be checked and built on Travis
Once your package has built on Travis and the build is passing with no errors, warnings, or notes you should 6. add your Travis badge to the README.md file of your package repository.
Functions Modifications
Unfortunatelly, I have made some modification to clear any erros, warnings and notes in all functions.

The most annoying problem is the no visible binding for global variable.

How to fix this problem
I have found in the Github an issues due to this problem:

Issue 451

Which led me to another issue in Stack Overflow.

How can I handle R CMD check “no visible binding for global variable” notes when my ggplot2 syntax is sensible?

Finally, I found the solution in dplyr website.

dplyr

If this function is in a package, using .data also prevents R CMD check from giving a NOTE about undefined global variables (provided that you’ve also imported rlang::.data with @importFrom rlang .data).

I realize that in the first link of Github someone said the above quotation.

Modifications
Whenever I saw a magrittr operator I must use the .data to point the oculted dataset. An example is the fars_read_years function, where I put .data$MONTH and .data$year.

fars_read_years <- function(years) {
        lapply(years, function(year) {
                file <- make_filename(year)
                tryCatch({
                        dat <- fars_read(file)
                        dplyr::mutate(dat, year = year) %>%
                                dplyr::select(.data$MONTH, .data$year)
                }, error = function(e) {
                        warning("invalid year: ", year)
                        return(NULL)
                })
        })
}