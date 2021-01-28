---
title: Setting up R and Configuring it for Usage Within Jupyter Lab
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-25 07:01:02'
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

.
.
.

## Summary

.
.
.


## Coming Up

.
.
.
