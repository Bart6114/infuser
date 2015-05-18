[![Travis-CI Build
Status](https://travis-ci.org/zachmayer/infuser.svg?branch=master)](https://travis-ci.org/zachmayer/infuser)
[![Coverage
Status](https://img.shields.io/coveralls/zachmayer/infuser.svg)](https://coveralls.io/r/zachmayer/infuser?branch=master)
[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/infuser)](http://cran.r-project.org/web/packages/infuser)
[![Downloads](http://cranlogs.r-pkg.org/badges/infuser)](http://cran.rstudio.com/package=infuser)

`infuser` is a simple and very basic templating engine for R. It
replaces parameters within templates with specified values. Templates
can be either contained in a string or in a file.

### Installation

    install.packages("infuser")

### Usage

Let's have a look at an example string.

    my_sql<-"SELECT * FROM Customers
    WHERE Year = {{year}}
    AND Month = {{month|3}};"

Here the variable parameters are enclosed by `{{` and `}}` characters.
See `?infuse` to use your own specification.

From now on, we suppose the character string `my_sql` is our template.
To show the parameters requested by the template you can run the
following.

    library(infuser)
    variables_requested(my_sql, verbose = TRUE)

    ## Variables requested by template:
    ## >> year
    ## >> month (default = 3)

To fill in the template simply provide the requested parameters.

    infused_sql<-
    infuse(my_sql, year=2016, month=8)

    cat(infused_sql)

    ## SELECT * FROM Customers
    ## WHERE Year = 2016
    ## AND Month = 8;

If a default value is available in the template, it will be used if the
parameter is not specified.

    infused_sql<-
    infuse(my_sql, year=2016)

    cat(infused_sql)

    ## SELECT * FROM Customers
    ## WHERE Year = 2016
    ## AND Month = 3;

Just like we're using a string here, a text file can be used. An example
text file can be found in the package as follows:

    example_file<-
      system.file("extdata", "sql1.sql", package="infuser")

    example_file

    ## [1] "/home/bart/R/x86_64-pc-linux-gnu-library/3.1/library/infuser/extdata/sql1.sql"

Again, we can check which parameters are requested by the template.

    variables_requested(example_file, verbose = TRUE)

    ## Variables requested by template:
    ## >> month (default = 3)
    ## >> year

And provide their values.

    infused_template<-
      infuse(example_file, year = 2016, month = 12)

    cat(infused_template)

    ## SELECT LAT_N, CITY, TEMP_F
    ## FROM STATS, STATION
    ## WHERE MONTH = 12
    ## AND YEAR = 2016
    ## AND STATS.ID = STATION.ID
    ## ORDER BY TEMP_F;

### Issues / questions

Simply create a new issue at this GitHub repository.
