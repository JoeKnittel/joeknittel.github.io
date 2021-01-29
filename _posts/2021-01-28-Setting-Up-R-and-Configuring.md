---
title: Setting up R and Configuring it for Usage Within Jupyter Lab
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-28 07:01:02'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](\images/r-top.png)

The last few posts on this blog have been about data analysis using the Python programming language. We got our feet wet with installing various packages, writing literate programs, and sharing our work with others through the cloud. Now, we seek to extend this same set of skills/features to another programming language called R.

Objectives for this post include:

1. Introducing R as a valid alternative to Python
2. Installing the base R language
3. Setting up R Studio, the most commonly used IDE for R  
4. Installing a few R packages
5. Configuring an R kernel to work within Jupyter Lab
6. Taking R for a test drive

## A Brief Introduction to R

`R` is a freely available programming language that's been around since 1993 and has, for most of its existence, been most data science practitioners' preferred tool for exploring and displaying their data.

The language was constructed specifically for statistical computing, with many useful statistical features built directly into it.

Just like in Python, we'll be able to work with tabular data in R by importing CSV files, Excel files, etc. into a data structure called a data frame; this can be done by utilizing nothing more than R's base package, whereas in Python we needed a supplementary package (pandas).

Due to its heritage as the de facto language for data science, R has a massive collection of packages with which we'll look to experiment in upcoming posts.

## Installation

We'll be installing a few things in this section. First, we'll install the base R program, then we'll download a useful IDE called R Studio, we'll install a few packages, and finally set up the R kernel for usage within Jupyter Lab.

### R

Getting R installed is slightly trickier than some other programs, but not bad at all.

Start by going <a href = "https://cran.r-project.org/mirrors.html">here</a> and selecting a mirror that's near you.

From there, you'll see an oddly-organized page as shown below:

![](\images/os-choice.png)

At the top, you'll see some operating system choices, so select your operating system here.

- If you're running Windows, click *base* on the following page, then *Download R 4.x.y for Windows* on the next page

- For Linux, you'll need to choose your distribution on the following page, before proceeding to run the appropriate command needed to install

- On macOS, the link found under the section titled *Latest release* should serve you well

### R Studio

Just like in Python, once you've installed the base program, you'll have access to R within the terminal and a very basic GUI, which you can find by searching "R GUI" on your computer.

<img src = "\images\r-gui.png" width = 400>

This is nice to have, but we'd really like a more complete development environment, so let's set up what most R users use for their IDE: `R Studio`.

Installing R Studio is much more straightforward than the R language itself. Just click <a href = "https://rstudio.com/products/rstudio/download/#download">here</a> and find the version for your operating system.

Once R Studio is installed, give it a look. You should see something similar to the image below:

![](\images\r-studio.png)

R Studio provides all of the tools necessary for us to perform all of our analyses in R. We can execute one-off chunks of code in the console at the bottom-left section, edit scripts in the upper left section, view our variables and their respective structures in the upper-right section, and manage packages, plots, and our files in the bottom-right section.

### Packages

Similar to how Python operates, we can extend the functionality of the program by installing `packages`, which are collections of functions, sample data, and documentation.

Most packages are available on `CRAN` (the Comprehensive R Archive Network) and can be installed within R Studio by typing `install.packages("[package-name]")` or by using the GUI button in the bottom-right section of the IDE.

Let's use the technique described above to install a collection of rather useful packages called the <a href = "https://www.tidyverse.org/packages/">Tidyverse</a>.

![](\images/install-package.gif)

Note that once the packages were installed, I loaded them into the session by using the <a href = "https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/library">library</a> function with the code: `library(tidyverse)`. This allows us to utilize all of the functions contained within the Tidyverse collection of packages in our code.

In future posts, we'll explore other ways to install packages that cannot be found on CRAN.

### Installing IR Kernel

Let's get R configured to function within Jupyter Lab. To do so, follow the steps defined <a href = "https://irkernel.github.io/installation/">here</a>.

For most people, the following lines of code should do the job, but do check out the installation page for additional details:

```r
install.packages('IRkernel')
IRkernel::installspec()
```

## Test Drive

Let's see if we have, indeed, configured the R kernel for Jupyter Lab functionality, and, if so, we'll test out some R code using Tidyverse.

### Properly Configured?

Do you remember how to load up Jupyter Lab? If not, open your terminal and change the directory (cd) to wherever you've been storing your Jupyter files. Then, type `jupyter lab` and hit enter.

After a few seconds, your favorite IDE should have loaded. This time, though, the *Launcher* will have a few more options, as seen below:

<img src = "\images/updated-launcher.png" width = 400>

### A Quick Test: Jupyter Lab + R Kernel + Tidyverse

First, start a new notebook running the R kernel by clicking the newly available icon.

Let’s turn off warnings, since R in Jupyter Lab tends to spit out a lot
of unnecessary information while running cells.

``` r
options(warn=-1)
```

Next, we’ll load up the Tidyverse collection of packages:

``` r
library(tidyverse)
```

In the following cell, we’ll read in some .csv data (using the
<a href = "https://readr.tidyverse.org/">readr package</a>) I’ve stored
locally on my site in case it’s deleted from its original source
(<https://support.spatialkey.com/spatialkey-sample-csv-data/>).

There’s not too much of a description of the dataset, but it appears to be data
pertaining to a collection of insurance policies in Florida from
2011-2012.

``` r
data = read_csv("http://joeknittel.github.io/sample_data.csv")
head(data) 
```

    ## # A tibble: 6 x 18
    ##   policyID statecode county eq_site_limit hu_site_limit fl_site_limit
    ##      <dbl> <chr>     <chr>          <dbl>         <dbl>         <dbl>
    ## 1   119736 FL        CLAY ~       498960        498960        498960
    ## 2   448094 FL        CLAY ~      1322376.      1322376.      1322376.
    ## 3   206893 FL        CLAY ~       190724.       190724.       190724.
    ## 4   333743 FL        CLAY ~            0         79521.            0
    ## 5   172534 FL        CLAY ~            0        254282.            0
    ## 6   785275 FL        CLAY ~            0        515036.            0
    ## # ... with 12 more variables: fr_site_limit <dbl>, tiv_2011 <dbl>,
    ## #   tiv_2012 <dbl>, eq_site_deductible <dbl>, hu_site_deductible <dbl>,
    ## #   fl_site_deductible <dbl>, fr_site_deductible <dbl>, point_latitude <dbl>,
    ## #   point_longitude <dbl>, line <chr>, construction <chr>,
    ## #   point_granularity <dbl>

Of particular interest are the *tiv\_2011* and *tiv\_2012* variables; they
represent the Total Insurable Value (TIV) of the policy from years 2011
and 2012, respectively.

Let’s see if we can observe any trends at the county level. Here,
we’ll employ the pipe operator `%>%` from the
<a href = "https://magrittr.tidyverse.org/">magrittr package</a> to sum
all TIV values for policies within each county, then add in a new
variable that computes the percent increase in TIV from 2011 to 2012.

``` r
grouped_data = data %>% group_by(county) %>%
               summarize(tiv2011 = sum(tiv_2011), tiv2012 = sum(tiv_2012),
                         change = (tiv2012-tiv2011)/tiv2011) %>%
               arrange(desc(change))
head(grouped_data, 10)
```

    ## # A tibble: 10 x 4
    ##    county              tiv2011    tiv2012 change
    ##    <chr>                 <dbl>      <dbl>  <dbl>
    ##  1 Orlando              36000      58857.  0.635
    ##  2 UNION COUNTY       3019258.   4324471.  0.432
    ##  3 DIXIE COUNTY      16040608.  22378058.  0.395
    ##  4 MADISON COUNTY    44965041.  61165726.  0.360
    ##  5 HIGHLANDS COUNTY 567025313. 756072585.  0.333
    ##  6 GILCHRIST COUNTY   9352936.  12270142.  0.312
    ##  7 OSCEOLA COUNTY      307029.    398351.  0.297
    ##  8 TAYLOR COUNTY     66138565.  85255667.  0.289
    ##  9 CALHOUN COUNTY    38235106.  48664158.  0.273
    ## 10 OKALOOSA COUNTY  658196224. 832855891.  0.265

It looks like there’s a special focus on policies in Orlando, Florida,
since Orlando is not a county and its percent change in TIV was higher
than that of any county in the state.

Let’s now get an idea of the distribution of percent increase in TIV
over all counties using a histogram from the
<a href = "https://ggplot2.tidyverse.org/">ggplot2 package</a>:

``` r
ggplot(data = grouped_data) + geom_histogram(mapping = aes(x = change))
```

![](/images/unnamed-chunk-5-1.png)<!-- -->

It appears as though most counties saw an increase in TIV, with an
average of around 20% increase.

``` r
mean(grouped_data$change)
```

    ## [1] 0.2016668

#### Bonus

As a demonstration of things to come, we’ll create a map of the percent
change in TIV. To do this, we’ll need a few additional packages (see
documentation
<a href = "https://urbaninstitute.github.io/urbnmapr/">here</a> and
<a href = "https://urbaninstitute.github.io/urbnthemes/articles/introducing-urbnthemes.html">here</a>)
from the <a href = "https://www.urban.org/aboutus">Urban Institute</a>,
so head over to your R console and run the lines:

``` r
install.packages("devtools")
devtools::install_github("UrbanInstitute/urbnmapr")           
install.packages("remotes")                                               
remotes::install_github("UrbanInstitute/urbnthemes")
```

Since these packages are not available on CRAN, we needed to use
package-specific instructions to install them.

Now, let's create our map:

``` r
library(urbnmapr)
library(urbnthemes)
names(grouped_data) = c("county_name","tiv2011","tiv2012","change")
grouped_data$county_name = tolower(grouped_data$county_name)
c = counties %>% filter(state_name == "Florida")
c$county_name = tolower(c$county_name)
m = grouped_data %>% full_join(c, by = "county_name")
p = m %>% ggplot(mapping = aes(long, lat, group = group, fill = change)) +
  geom_polygon(color = "#ffffff", size = .25) +
  scale_fill_gradientn(labels = scales::percent,
                       guide = guide_colorbar(title.position = "top")) +
  coord_map(projection = "albers", lat0 = 39, lat1 = 45) +
  labs(fill = "% change in TIV") +
  theme(legend.title = element_text(), legend.key.width = unit(.5, "in")) +
  theme_urbn_map()
p
```

We won’t get into the specifics of what’s going in the code above (see future posts
discussing data wrangling and visualization using R), but see if you can decipher
what actions the code is performing (you may need to review the tidyverse and urbanmapr
documentation).

Anyway, here's the result:

![](/images/unnamed-chunk-7-1.png)<!-- -->

It looks like there was a greater increase in TIV in
north-central and north-western Florida, with some data lacking in a few
counties on the east.

## Summary

We accomplished a lot in this post: we installed a really useful programming language called R and its most commonly used IDE (R Studio); we configured R to work within Jupyter Lab; we installed R packages from CRAN and other sources; and we began to explore the functions that can be found in the Tidyverse collection of packages.

## Coming Up

The chloropleth map we constructed in the last section was just to whet your appetite for data visualization. In the next post, we'll work with several really cool packages that will allow us to create interactive plots. It should be a fun one!
