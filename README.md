
<!-- README.md is generated from README.Rmd. Please edit that file -->
phsmethods
==========

[![Build Status](https://travis-ci.com/Health-SocialCare-Scotland/phsmethods.svg?branch=master)](https://travis-ci.com/Health-SocialCare-Scotland/phsmethods) [![codecov](https://codecov.io/gh/Health-SocialCare-Scotland/phsmethods/branch/master/graph/badge.svg)](https://codecov.io/gh/Health-SocialCare-Scotland/phsmethods)

`phsmethods` contains functions for commonly undertaken analytical tasks in [Public Health Scotland (PHS)](https://publichealthreform.scot/public-health-scotland):

-   `file_size()` returns the names and sizes of files in a directory
-   `fin_year()` assigns a date to a financial year in the format `YYYY/YY`
-   `postcode()` formats improperly recorded postcodes
-   `qtr()`, `qtr_end()`, `qtr_next()` and `qtr_prev()` assign a date to a quarter

`phsmethods` can be used on both the [server](http://spsssrv02.csa.scot.nhs.uk:8787/) and desktop versions of RStudio.

Installation
------------

To install `phsmethods`, the package `remotes` is required, and can be installed with `install.packages("remotes")`.

You can then install `phsmethods` on RStudio server from GitHub with:

``` r
remotes::install_github("Health-SocialCare-Scotland/phsmethods")
```

Network security settings may prevent `remotes::install_github()` from working on RStudio desktop. If this is the case, `phsmethods` can be installed by downloading the [zip of the repository](https://github.com/Health-SocialCare-Scotland/phsmethods/archive/master.zip) and running the following code (replacing the section marked `<>`, including the arrows themselves):

``` r
remotes::install_local("<FILEPATH OF ZIPPED FILE>/phsmethods-master.zip",
                       upgrade = "never")
```

Using phsmethods
----------------

Load `phsmethods` using `library()`:

``` r
library(phsmethods)
```

To access the help file for any of `phsmethods`' functions, type `?function_name` into the RStudio console after loading the package:

``` r
?fin_year
?postcode
```

### file\_size

``` r
# Names and sizes of all files in the tests/testthat/files folder
file_size(testthat::test_path("files"))
#> # A tibble: 8 x 2
#>   name             size       
#>   <chr>            <chr>      
#> 1 airquality.xls   Excel 26 KB
#> 2 bod.xlsx         Excel 5 KB 
#> 3 iris.csv         CSV 4 KB   
#> 4 mtcars.sav       SPSS 4 KB  
#> 5 plant-growth.rds RDS 316 B  
#> 6 puromycin.txt    Text 442 B 
#> 7 stackloss.fst    FST 897 B  
#> 8 swiss.tsv        TSV 1 KB

# Names and sizes of Excel files only in the tests/testthat/files folder
file_size(testthat::test_path("files"), pattern = ".xlsx?$")
#> # A tibble: 2 x 2
#>   name           size       
#>   <chr>          <chr>      
#> 1 airquality.xls Excel 26 KB
#> 2 bod.xlsx       Excel 5 KB
```

### fin\_year

``` r
a <- lubridate::dmy(c(21012017, 04042017, 17112017))
fin_year(a)
#> [1] "2016/17" "2017/18" "2017/18"
```

### postcode

``` r
# The default is pc7 format
postcode("G26QE")
#> [1] "G2  6QE"

# But pc8 format can also be applied
postcode(c("KA89NB", "PA152TY"), format = "pc8")
#> [1] "KA8 9NB"  "PA15 2TY"

# postcode accounts for irregular spacing and lower case letters
library(dplyr)
b <- tibble(pc = c("G 4 2 9 B A", "g207al", "Dg98bS", "DD37J    y"))
b %>% mutate(pc = postcode(pc))
#> # A tibble: 4 x 1
#>   pc     
#>   <chr>  
#> 1 G42 9BA
#> 2 G20 7AL
#> 3 DG9 8BS
#> 4 DD3 7JY
```

### qtr, qtr\_end, qtr\_next and qtr\_prev

``` r
c <- lubridate::dmy(c(26032012, 04052012, 23092012))

# qtr returns the current quarter and year
# The default is long format
qtr(c)
#> [1] "January to March 2012"  "April to June 2012"     "July to September 2012"

# But short format can also be applied
qtr(c, format = "short")
#> [1] "Jan-Mar 2012" "Apr-Jun 2012" "Jul-Sep 2012"


# qtr_end returns the last month in the quarter
qtr_end(c)
#> [1] "March 2012"     "June 2012"      "September 2012"
qtr_end(c, format = "short")
#> [1] "Mar 2012" "Jun 2012" "Sep 2012"


# qtr_next returns the next quarter
qtr_next(c)
#> [1] "April to June 2012"       "July to September 2012"  
#> [3] "October to December 2012"
qtr_next(c, format = "short")
#> [1] "Apr-Jun 2012" "Jul-Sep 2012" "Oct-Dec 2012"


# qtr_prev returns the previous quarter
qtr_prev(c)
#> [1] "October to December 2011" "January to March 2012"   
#> [3] "April to June 2012"
qtr_prev(c, format = "short")
#> [1] "Oct-Dec 2011" "Jan-Mar 2012" "Apr-Jun 2012"
```

Contributing to phsmethods
--------------------------

At present, the maintainers of this package are [Jack Hannah](https://github.com/jackhannah95), [David Caldwell](https://github.com/davidc92) and [Lucinda Lawrie](https://github.com/lucindalawrie).

This package is intended to be in continuous development and contributions may be made by anyone within PHS. If you would like to contribute a function, or propose an improvement to an existing function, please first create an [issue](https://github.com/Health-SocialCare-Scotland/phsmethods/issues) on GitHub and assign **all** of the package maintainers to it. This is to ensure that no duplication of effort occurs in the case of multiple people having the same idea. The package maintainers will discuss the issue and get back to you as soon as possible.

When contributing, please create a [branch](https://github.com/Health-SocialCare-Scotland/phsmethods/branches) in this repository and carry out all work on it. Please ensure you have linked RStudio to your GitHub account using `usethis::edit_git_config()` prior to making your contribution. When you are ready for a review, please create a [pull request](https://github.com/Health-SocialCare-Scotland/phsmethods/pulls) and assign **all** of the package maintainers as reviewers. One or more of them will conduct a review, provide feedback and, if necessary, request changes prior to merging your branch.

Please be mindful of information governance when contributing to this package. No data files (aside from publically available and downloadable datasets or unless explicitly approved), server connection details, passwords or person identifiable or otherwise confidential information should be included anywhere within this package or any other repository (whether public or private) used within PHS. This includes within code and code commentary. For more information on security when using git and GitHub, and on using git and GitHub for version control more generally, please see the [Transforming Publishing Programme](https://www.isdscotland.org/Products-and-Services/Transforming-Publishing-Programme/)'s [Git guide](https://nhs-nss-transforming-publications.github.io/git-guide/) and [GitHub guidance](https://github.com/NHS-NSS-transforming-publications/GitHub-guidance).

Please feel free to add yourself to the 'Authors' section of the `Description` file when contributing. As a rule of thumb, please assign your role as author (`"aut"`) when writing an exported function, and as contributor (`"ctb"`) when editing an existing function and/or writing a non-exported function.

`phsmethods` will, as much as possible, adhere to the [tidyverse style guide](https://style.tidyverse.org/) and the [rOpenSci package development guide](https://devguide.ropensci.org/). The most pertinent points to take from these are:

-   All function names should be in lower case, with words separated by an underscore
-   Put a space after a comma, never before
-   Put a space before and after infix operators such as `<-`, `==` and `+`
-   Limit code to 80 characters per line
-   Function documentation should be generated using [`roxygen2`](https://github.com/r-lib/roxygen2)
-   All functions should be tested using [`testthat`](https://github.com/r-lib/testthat)
-   The package should always pass `devtools::check()`

It's not necessary to have experience with GitHub or of building an R package to contribute to `phsmethods`; as long as you can write an R function, the package maintainers can assist with error handling, writing documentation, testing and other aspects of package development. It is advised, however, to consult Hadley Wickham's [R Packages](https://r-pkgs.org/) book prior to making a contribution. It may also be useful to consult the [documentation](https://github.com/Health-SocialCare-Scotland/phsmethods/tree/master/R) and [tests](https://github.com/Health-SocialCare-Scotland/phsmethods/tree/master/tests/testthat) of existing functions within this package as a point of reference.

Please note that this README may fail to 'Knit' at times as a result of network security settings. This will likely be due to the badges for continuous integration and test coverage at the top of the document. If you are editing the `README.Rmd` document and are unable to successfully get it to 'Knit', please contact the package maintainers for assistance.
