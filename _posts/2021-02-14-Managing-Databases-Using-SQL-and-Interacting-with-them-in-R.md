---
title: Managing Databases Using SQL and Interacting With Them in R
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-02-14 9:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](/images/sql+r.png)<!-- -->

In previous posts, we’ve discussed accessing and interacting with data stored in various formats (Excel spreadsheets, CSV files, data frames, JSON objects, etc.).  Now, we turn our attention to perhaps the most common data source: the database.

We’ll do the following:

1. Describe what databases are, consider why we might use a database rather than, say, a spreadsheet, and list some common database management systems  
2. Discuss database servers, how to set one up locally, and the credentials required for logging into the server
3. Introduce some common SQL commands, try a few out, and load up a database from a .sql file
4. Set up a DBI connection to a database in R
5. Query a database in R using SQL and dplyr commands

## Introduction to Databases

So, what is a database? Put simply, a database is just an organized collection of data that can be managed (i.e., defined, updated, controlled, and queried) using computer software called a DBMS (database management system).

### Why Use a Database?

Spreadsheets have served us well in the past; what makes a database a better option?

**Data Volume** – there’s a set row and column maximum within spreadsheet software, limiting their utility when working with massive datasets

**Speed** – spreadsheets load the entire dataset into the local machine’s memory, which, when working with a large dataset, can cause delays

**User Permissions** – databases, by requiring login credentials, allow individualized privileges (e.g., admin/read/write) to different people

**Updating** – many people can simultaneously manage a database

**Data Integrity** – the database’s model ensures that all data maintain a pre-specified form, ensuring consistency and validity

### Database Models

Databases have been around since the 1970’s, and, as you might imagine, their logical structures have evolved substantially since their inception.

In this post, we’ll focus on a database model called the relational database, wherein data are organized into tables (relations) comprised of rows (tuples) and columns (attributes).

Below is an example of an ER-diagram, which allows us to visualize such a data model:  

![](/images/er-diagram.png)<!-- -->

### Database Management Systems (DBMS)

As discussed previously, we need some kind of computer software that allows us to manage our database. To that end, there are <a href = "https://db-engines.com/en/ranking">hundreds of options</a>, but the top few (Oracle, MySQL, Microsoft SQL Server, PosgreSQL, and MongoDB) take the lion’s share of the market.

Most of these DBMS’s utilize a programming language called SQL (structured query language) to manage their underlying databases.

## Database Servers

The machine in which databases reside is called a database server. In order to optimize performance, this is often a dedicated machine (it exists specifically to host databases), loaded up with multi-core processors, lots of RAM, and data storage redundancies.

### Remote Server

In most cases, the database you’ll be interested in and its corresponding server will already be set up.

If that’s the case, you’ll just need the following in order to manage the database:
-	host address (server’s domain name or IP address);
-	appropriate credentials (username and password); and,
-	the database name

### Local Server

Alternatively, you could run a database server and host databases on your local machine.

In fact, as a self-contained running example in the next few sections, we’ll do just that by setting up MySQL locally.

### Working Example, Part 1: Setting up a Local Database Server

Throughout the rest of this post, we’ll be working with a free, easy-to-use DBMS called MySQL. We’ll run it locally, so we’ll have the ability to experiment with creating our own database.

First, If you’re running Windows, head over <a href = "https://dev.mysql.com/downloads/windows/installer/8.0.html">here</a> to download the MySQL Installer. This will set up all of the essential tools you might need in order to manage your server. Use default settings.  

If you’re on a different OS, then start <a href = "https://dev.mysql.com/downloads/mysql/">here</a> by downloading MySQL Server for your particular OS. You may find other MySQL tools (e.g., an ODBC connector) useful in the future, but MySQL Server is all we’ll need.  

*Note 1*: MySQL Server installation may take upwards of an hour to complete, will require several gigabytes of storage, and will likely download a number of dependencies (e.g., Visual Studio) along the way.

*Note 2*: Towards the end of the installation process, you will be asked to provide a root password. Record this information somewhere safe, as it will be used whenever you want to interact with the DBMS.

## SQL, the Language of Your Favorite DBMS

Once we have a database server, whether it be remote or local, we can use it to host databases and we can manage those databases with SQL commands.

Let’s put our newly-installed server to the test, and play around with some commonly-used SQL commands.

### Working Example, Part 2: Experimenting with SQL    

You may have installed an IDE (i.e., MySQL Workbench), but, for the sake of accessibility, we’ll write our SQL using the command line client (search “MySQL Command Line Client” on your computer; if you’re in Windows, it’s probably in the C:\Program Files\MySQL\MySQL Server 8.0\bin\ directory).

Log in with the root password you defined during the installation process and press enter. Afterward, type the SQL code `SHOW DATABASES;` to see what’s stored on your server.

![](\images\show-databases.gif)

Alright, let’s set up a very basic database. We’ll call our database **test** and it’ll contain just one table called **people**, which will have two attributes: a **PersonID** and a **LastName**.

We create our database with the code: `CREATE DATABASE test;`

Then, access our database with: `USE test;`

Creating a new table is slightly trickier: `CREATE TABLE people (PersonID int, LastName varchar(255));`

*Note*: Observe the structure of the last statement; attributes are defined by name (e.g., **PersonID**) followed by their data type (e.g., varchar(255), a string), separated by commas.

![](\images\create-table.gif)

Actually, before we proceed, let’s include a **FirstName** attribute in our people table as well.

We can do that with the following code: `ALTER TABLE people ADD FirstName varchar(255);`

Then, view the updated structure of our table using: `DESCRIBE people;`

![](\images\table-structure.gif)

Okay, let’s add in a few data entries.

![](\images\insert-data.gif)

Cool! Now, let’s see what’s in our table.

We can grab all entries in the table with the following code: `SELECT * FROM people;`

Or, we can refine our search to contain only entries that satisfy certain conditions. For instance, this code only selects entries whose LastName attribute has a second character of 'a': `SELECT * FROM people WHERE LastName LIKE ‘_a%’;`

![](\images\select-from-table.gif)

We’ve looked at some of the most common SQL commands, but there are <a href = "https://www.w3schools.com/sql/default.asp">plenty more</a>, and they can get really complicated.

We’ll look at one more SQL command before we proceed to the next section: `SOURCE`. Since our database is rather simplistic, and as a good practice exercise, let’s load up an entire database from a .sql file into our server.

Go to <a href = "https://www.mysqltutorial.org/mysql-sample-database.aspx">this page</a>; you’ll find a link to a sample database containing information from a business that sells scale models of classic cars. Download the .zip file and unzip the file (mysqlsampledatabase.sql) into an easily-accessible directory (I placed it in C:\temp).

Then, run the SQL command: `SOURCE [path-to-sample-database]` (Note: no semi-colon; see video).

![](\images\source.gif)

If you did everything correctly, when you run the SQL command: `SHOW DATABASES;`, you should see a new entry called classicmodels. Feel free to examine the content of the database using the commands we’ve discussed already and/or those found in the link above; we’ll delve deeper into the database’s structure and query it within R in later sections.

## Connecting to a Database in R

Now that we have a database to work with, let’s access its data using R.

### Install Packages

**DBI** – database interface package  

**RMySQL** – MySQL driver for R

**rstudioapi** (optional) – creates a secure password prompt   

Each of the above R packages is available on CRAN, so they can be installed with the standard code:

``` r
install.packages("DBI")
install.packages("RMySQL")
install.packages("rstudioapi")
```

### Working Example, Part 3: Database Connection in R

Now that we have the appropriate packages, let's set up a connection to our sample database from earlier:

``` r
library(DBI)
con = dbConnect(RMySQL::MySQL(),
                dbname = "classicmodels",
                host = "localhost",
                port = 3306,
                user = "root",
                password =  rstudioapi::askForPassword("Database Password"))
```

*Note 1*: You will be prompted to input the DBMS root password when you run this code.

*Note 2*: If accessing a different database via a remote server, you’ll need appropriate credentials, as described earlier.

*Note 3*: If you’re connecting to an entirely different DBMS, you’ll need a different driver (e.g., PosgreSQL requires the RPosgreSQL package and RMySQL::MySQL() would be replaced with RPostgres::Postgres() in the dbConnect call)

## Querying a Database in R

Let’s now begin a preliminary analysis of the data in our database.

### Working Example, Part 4: Querying the Database

In R, we can query the database using SQL with the *dbGetQuery* function from the DBI package.

For instance, we can get a data frame consisting of the first five entries in the **customers** table with the following code:

``` r
# runs SQL command, and pipes the result to a data frame
dbGetQuery(con, "SELECT * FROM customers LIMIT 5")
```

    ##   customerNumber               customerName contactLastName contactFirstName
    ## 1            103          Atelier graphique         Schmitt          Carine
    ## 2            112         Signal Gift Stores            King             Jean
    ## 3            114 Australian Collectors, Co.        Ferguson            Peter
    ## 4            119          La Rochelle Gifts         Labrune          Janine
    ## 5            121         Baane Mini Imports      Bergulfsen           Jonas
    ##          phone                 addressLine1 addressLine2      city    state
    ## 1   40.32.2555               54, rue Royale         <NA>    Nantes     <NA>
    ## 2   7025551838              8489 Strong St.         <NA> Las Vegas       NV
    ## 3 03 9520 4555            636 St Kilda Road      Level 3 Melbourne Victoria
    ## 4   40.67.8555 67, rue des Cinquante Otages         <NA>    Nantes     <NA>
    ## 5   07-98 9555       Erling Skakkes gate 78         <NA>   Stavern     <NA>
    ##   postalCode   country salesRepEmployeeNumber creditLimit
    ## 1      44000    France                   1370       21000
    ## 2      83030       USA                   1166       71800
    ## 3       3004 Australia                   1611      117300
    ## 4      44000    France                   1370      118200
    ## 5       4110    Norway                   1504       81700

Notice that we must specify the database connection in the first argument, then follow it with a SQL command.

Oftentimes, it’s much more time-efficient to query our database using dplyr (recall this Tidyverse package from previous posts).

We can generate the same result as above (except, as a tibble, rather than a data frame) using the following code:

``` r
# runs SQL command via dplyr, then pipes the result to a tibble
tbl(con, "customers") %>% head(5) %>% collect()
```

    ## # A tibble: 5 x 13
    ##   customerNumber customerName contactLastName contactFirstName phone
    ##            <int> <chr>        <chr>           <chr>            <chr>
    ## 1            103 Atelier gra~ Schmitt         "Carine "        40.3~
    ## 2            112 Signal Gift~ King            "Jean"           7025~
    ## 3            114 Australian ~ Ferguson        "Peter"          03 9~
    ## 4            119 La Rochelle~ Labrune         "Janine "        40.6~
    ## 5            121 Baane Mini ~ Bergulfsen      "Jonas "         07-9~
    ## # ... with 8 more variables: addressLine1 <chr>, addressLine2 <chr>,
    ## #   city <chr>, state <chr>, postalCode <chr>, country <chr>,
    ## #   salesRepEmployeeNumber <int>, creditLimit <dbl>

Let’s look deeper. What else can we learn from our database?

How about a histogram of the company’s order amounts:

``` r
payments = tbl(con, "payments") %>% collect()
ggplot(payments, aes(x = amount)) + geom_histogram()
```

![](/images/unnamed-chunk-5-1.png)<!-- -->

Wow! There’s a $120k+ purchase in there. Could that be real? Seems like
a whole lotta scale models of vintage cars… Let’s investigate:

``` r
tbl(con, "payments") %>% arrange(desc(amount)) %>% head(1)
```

    ## # Source:     lazy query [?? x 4]
    ## # Database:   mysql 8.0.23 [root@localhost:/classicmodels]
    ## # Ordered by: desc(amount)
    ##   customerNumber checkNumber paymentDate  amount
    ##            <int> <chr>       <chr>         <dbl>
    ## 1            141 JE105477    2005-03-18  120167.

Apparently, customer \#141 made a $120,167 purchase in March of 2005.
Who could possibly be buying so many scale models of cars?

``` r
tbl(con, "customers") %>% filter(customerNumber == 141) %>%
select(customerNumber, customerName)
```

    ## # Source:   lazy query [?? x 2]
    ## # Database: mysql 8.0.23 [root@localhost:/classicmodels]
    ##   customerNumber customerName          
    ##            <int> <chr>                 
    ## 1            141 Euro+ Shopping Channel

That makes sense, a shopping channel made the big purchase.

And, as it turns out, the payment didn’t even correspond to one specific
order, as the most Euro+ ever spent was around $60k:

``` r
tbl(con, "orders") %>% filter(customerNumber == 141) %>%
left_join(tbl(con, "orderdetails")) %>% group_by(orderNumber) %>%
summarize(orderTotal = sum(quantityOrdered * priceEach)) %>%
arrange(desc(orderTotal))
```

    ## # Source:     lazy query [?? x 2]
    ## # Database:   mysql 8.0.23 [root@localhost:/classicmodels]
    ## # Ordered by: desc(orderTotal)
    ##    orderNumber orderTotal
    ##          <int>      <dbl>
    ##  1       10212     59831.
    ##  2       10262     47065.
    ##  3       10386     46969.
    ##  4       10412     46895.
    ##  5       10350     46493.
    ##  6       10153     44940.
    ##  7       10358     44185.
    ##  8       10104     40206.
    ##  9       10203     40063.
    ## 10       10383     36852.
    ## # ... with more rows

Finally, let’s see Euro+’s top 5 favorite purchases from the
retailer:

``` r
tbl(con, "orders") %>% filter(customerNumber == 141) %>%
left_join(tbl(con, "orderdetails")) %>%
left_join(tbl(con, "products"),by = "productCode") %>%
select(productName, quantityOrdered, priceEach) %>%
group_by(productName) %>%
summarize(numPurchased = sum(quantityOrdered),
          totalPricePaid = sum(quantityOrdered *priceEach)) %>%
arrange(desc(totalPricePaid)) %>% head(5)
```

    ## # Source:     lazy query [?? x 3]
    ## # Database:   mysql 8.0.23 [root@localhost:/classicmodels]
    ## # Ordered by: desc(totalPricePaid)
    ##   productName                    numPurchased totalPricePaid
    ##   <chr>                                 <dbl>          <dbl>
    ## 1 1992 Ferrari 360 Spider red             308         46992.
    ## 2 1956 Porsche 356A Coupe                 161         21048.
    ## 3 1957 Chevy Pickup                       183         18716.
    ## 4 1998 Chrysler Plymouth Prowler          125         18050.
    ## 5 1903 Ford Model A                       140         17789.

I’m no car buff, but I guess people like the 1992 Ferrari 360 Spider, in
red.

## Summary

We now know what a database is and how to manage one with a DBMS using SQL commands.

Even more important, we can access databases through R using the DBI package in conjunction with our favorite database driver and the appropriate credentials.

Finally, we can manipulate and analyze the data from our database using SQL commands or even dplyr with pipes, as we’ve done in previous posts.

## Coming Up
In our next post, we will develop a basic data-driven app using the Shiny package in R.

We’ll talk about the core components of any Shiny app, build out a UI consisting of a bunch of widgets for interactivity, and define the functions that’ll be executed upon such interactions.

It might be the most fun and useful blog entry yet!
