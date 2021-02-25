---
title: Creating a Data-Driven App With Shiny and Deploying It Online
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-02-24 10:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](/images/shiny.png)<!-- -->

Long gone are the days in which only computer science experts have the capability to develop software. Over the past few decades, technology has made huge strides, and the results can be seen not just in the way we live our everyday lives, but also in the way we solve problems.

This post aims to demonstrate that, with the right set of tools, even building and deploying a data-driven web app is not nearly as challenging as one might expect. Our tools of choice will be R Studio and the Shiny package.

We'll accomplish the following objectives throughout the course of the post:

1. Describe the structure of a Shiny app
2. Conceptualize our own app for interacting with and visualizing the famous Gapminder dataset
3. Develop code for the app's user interface and server logic
4. Test our app on a local server
5. Deploy the app online, with shinyapps.io

## What is Shiny?

[Shiny](https://shiny.rstudio.com/) is an R package that allows users to quickly and seamlessly build data-driven web apps.

Users interact with Shiny apps via [widgets](https://shiny.rstudio.com/gallery/widget-gallery.html), which adjust various output components in the app's reactive environment.

The package has also been developed to allow for considerable customization, with support for CSS themes and various JavaScript actions.  

## Structure of a Shiny App

Shiny apps can be structured in a number of ways, but they always have three major components: a user interface, server logic, and references to add-on packages used by the app.  

In this section, we'll describe the most common way these components are defined, as well as some alternative structures.

### Most Common Structure

The best way to structure a shiny app (in my humble opinion) is to break each of the three main components into its own R script file. Doing so makes it easier to keep your code organized, which is especially important if your app has many widgets and/or complex logic defining the widgets' behavior.

Below, we describe the three files in this configuration.

*Note: if using this configuration, the file names must be exactly as follows: ui.R, server.R, global.R*

#### ui.R

This file describes the user interface of the Shiny app.

Shiny apps are composed of widgets whose states can be modified by a user.

Here we will define, at the very least, which widgets will be displayed within the app and their respective locations on the screen.

For many of the widgets, we will also be defining:

- visual elements (e.g. width, height, colors, etc.)
- initial conditions (e.g., the starting value of a slider when the app loads)
- user-selectable states (e.g., minimum value, maximum value, list of choices, on/off)
- optional labels

However, these characteristics are frequently a function of user input; if this is the case, arguments defining the widgets' look and behavior will be the output of some server logic, and hence will be found in the server.R file.

#### server.R

This file describes the server logic defining how reactive plots (and, potentially, other widgets) are to be rendered on-screen based on the current widget states.

The main idea here is that the user defines the values of variables by choosing widget states, and these variable values are then used to output certain reactive elements, which will change if ever the user adjusts the widgets to a different state.

#### global.R

This file is very simple. It's just a list of library calls to various R packages whose functions can be accessed anywhere within ui.R or server.R.

### Alternative Structures

The above 3-file structure is not the only way to build a Shiny app. Here are two alternative setups:

#### ui.R + server.R

We can forgo the global.R file altogether and just add library calls for UI-specific and server-specific elements at the top of their respective files.

#### app.R

It's possible to build a Shiny app with just one file: app.R

To do so, the file should be composed as:

```r
[global.R]

ui <-

   [ui.R]

server <- function(input, output) {

  [server.R]

}

shinyApp(ui, server)
```
Where **[global.R]**, **[ui.R]**, and **[server.R]** would be the code found within each respective file, given one were to have structured the app with the standard 3-file structure.

## Conceptualizing Our App

Alright, we now have an idea of what Shiny is and the file structure of a Shiny app.

Let's now begin to conceptualize our own app.

### Purpose

We'll be building an app to visualize the relationship between a country's GDP per capita and its people's mean life expectancy, using data from R's [gapminder package](https://cran.r-project.org/web/packages/gapminder/index.html).

We'd like our app to allow the user to look at specific subsets of the data (e.g., display only countries from a specific continent, and/or display only countries whose population is less than a specific number, and/or only display data from a specific year, etc.).

Users should be able to modify their data selections by adjusting the states of an assortment of different widgets.

Also, to maximize the app's immersiveness, we'd like to include an interactive plot, with a tooltip summarizing individual data points, and each point colored based on its continents, with a radius based on its population.

### Structure

We'll use the 3-file (i.e., global.R, ui.R, server.R) setup for our app.

### Sketch

Building a Shiny app without a sketch is akin to an architect building a structure without a blueprint.

Obviously, it's not exactly the same thing, but it's highly advisable to, at the very least, put together a rough sketch of how you want your app to look and feel. Doing so will make developing the code in ui.R and server.R significantly easier.

Here's approximately how I hope our finished app will look:

![](/images/sketch.png)<!-- -->

### Description

The app, as depicted in the sketch, can be described as follows:

<ol>
  <li>At the top of the app, we have a navigation bar, with a title and two tabs: <b>Analysis</b> and <b>About</b></li>
  <br><li>The following will be displayed on the <b>Analysis</b> page:</li><br>
  <ul>
    <li>A sidebar panel, which houses the following widgets:</li><br>
    <ul>
      <li>an upload button to upload a dataset</li>
      <li>a dropdown menu to select the continent</li>
      <li>a slider for selecting a range of GDP per capita values to be displayed</li>
      <li>a checkbox that determines whether to apply a log transformation to GDP per capita</li>
      <li>a slider for selecting a range of populations to display</li>
      <li>a slider to select the year</li>
    </ul>
    <li>A main panel, which displays a reactive, interactive plot, rendered based on the states of the sidebar widgets</li>
  </ul>
  <li>On the <b>About</b> page, we'll just dispay some markdown-formatted text, which describes the nature of the app</li>
</ol>

## Building the App

Alright, we've brainstormed the design and functionality of our app. Now, let's build this thing!

### ui.R

Defining the UI is rather straightforward and follows the description in the previous section almost to a tee.

#### Navigation Bar

We'll start with the navigation bar, which we'll reference in the server logic with an ID; we'll give the navbar a title, and we'll choose which tab is selected upon loading the app:

```r
# the navbar is the top bar with the pages of our app
navbarPage(id = "nav", title = "Global Life Expectancy vs. GDP per Capita",
           selected = "about",

           [code for the rest of the UI components]

)
```
<i>Notice that we have not fully defined our navbarPage; the following function calls will be nested within the navbarPage and the final line of our ui.R file will be a closing parenthesis pertaining to this component.</i>

#### Analysis Page

Let's now define the **Analysis** page:

```r
    # the first page is the main page, where data is to be uploaded and the plot is
    # to be rendered
    tabPanel(value = "analysis", title = "Analysis",

      fluidPage(

            # the main page is broken up into a sidebar panel and main panel
            sidebarLayout(

                # the sidebar panel houses all of our widgets which control
                # the resultant plot
                sidebarPanel(

                      **[ALL OF OUR WIDGETS!]**

                ),

                mainPanel(

                      **[OUR PLOT!]**
                )
            )
        )
    ),
```

So, when the "Analysis" tab is selected in the navigation bar, we'll display a fluidPage (i.e., a reactive page) with a sidebar panel containing our widgets and a main panel containing our plot.

**Widgets**

Within the sidebarPanel, we need to define our widgets.

The first widget is an input button; it's composition is not user dependent, so we can define it entirely within ui.R:

```r
# upload an .rds data file
fileInput("file1", "Upload Data:",
          multiple = FALSE,
          accept = c(".rds")),
```

With this construction, the user will see the label "Upload Data:" on a browse button, which, when pressed will allow them to choose one local .rds file containing the dataset; we can then reference the inputted file in the server logic as **input$file1**.

*Note: our app will be rather contrived in that many of its components will be defined in such a way to only handle data from a specific .rds file, which we'll link within the app*.

We'd like the other widgets to be defined/displayed only once the data file is inputted, so in our ui.R, we'll just call them **uiOutput** and give them each an ID for reference in the server logic:

```r
# dropdown menu with the continents
uiOutput("continentDropdown"),

# slider to control gdpPercap variable
uiOutput("gdpSlider"),

# checkbox to choose whether to log-transform the gdpPercap variable
uiOutput("logCheckbox"),

# slider to control the population range that will be represented in the plot
uiOutput("popSlider"),

# slider to select the year
uiOutput("yearSlider")
```

**Plot**

Our plot, as we said earlier, is the only thing in the mainPanel.

Here, we can define the dimensions of the plot, and, to make it a cool interactive plot, we'll be using Plotly with a *plotlyOutput* function call:

```r
# the main panel houses the plot generated based on the states
# of the widgets in the sidebar
mainPanel(

    # the plot is the only item in the main panel
    plotlyOutput("mainPlot", width = "80%", height = "600px")

)
```

#### About Page

Here's the code for the **About** page. Not too much to say other than that we could have alternatively used a .html file with an *includeHTML* function call, or even hard-coded in the text and formatting of the page without an external file:

```r
# the second page of the navbar is an about page that describes the app
tabPanel(value = "about", title = "About",

  fluidPage(

        # the about page is generated from a markdown file within the app's directory
        includeMarkdown("about.md")
    )
)
```

#### UI Summary

That's everything; our UI is fully defined! Click [here]("https://github.com/JoeKnittel/ShinyDemo/blob/main/Code/ui.R") to see the entire file.

### server.R

Alright, now we need to define the server logic for rendering our app components.

We start with the *shinyServer* function:

```r
# define server logic to render ui elements and the resultant plot
shinyServer(function(input, output) {
.
.
.
```

#### Reactive Functions

Let's now define some reactive functions that we'll use to render our widgets.

**getFile**

First, let's define a *getFile* function to store the data from the upload widget into an R data frame:

```r
# function to access inputted data, if present, as store it in a data frame
    getFile <- reactive({

        # only proceed once a data file is uploaded
        req(input$file1)

        # attempt to load the data into a data frame
        tryCatch(
            df <- readRDS(input$file1$datapath),
            error = function(e) {
                # return a safeError if a parsing error occurs
                stop(safeError(e))
            }
        )

        # return the data as a data frame
        df
    })
```

*Note: We do a bit of error checking to make sure nothing funny was uploaded.*

**getData**

Now, let's define a function for grabbing a filtered subset of the dataset, based on user-defined widget states:

```r
# function to reduce data to just that which is defined by widget states
getData <- reactive({

    # only proceed once a data file is uploaded and the dropdown widget is loaded
    req(input$file1)
    req(input$continent)

    # attempt to load the data into a data frame
    tryCatch(
        df <- readRDS(input$file1$datapath),
        error = function(e) {
            # return a safeError if a parsing error occurs
            stop(safeError(e))
        }
    )

    # filter the data based on continent dropdown and year slider
    if(input$continent != "All"){
        df = df %>% filter(continent == input$continent)
    }

    df = df %>% filter(year == input$year)

    # return the filtered data frame  
    df
})

```
<i>Note: we've used <b>magrittr</b> pipes and <b>dplyr</b> function calls, so we'll need to make sure we load up the <b>tidyverse</b> package in our global.R file.</i>

#### Render Widgets

In our ui.R, we stated that a number of our widgets would be a function of user input; we constructed placeholder UI elements and gave them IDs.

Now, we can proceed to define their remaining characteristics.

**Continents Dropdown Menu**

Let's start with the continents dropdown menu:

```r
# dropdown menu to select the limit the data to a specific continent   
output$continentDropdown <- renderUI({
    df <- getFile()
    selectInput("continent", "Choose Continent:",
                choices = c("All", levels(unique(df$continent))))
})
```
continentDropdown was the ID we defined for the dropdown in ui.R, and we use the renderUI function to define it.

We store the uploaded data in a data frame using our reactive function, then use it to define the possible choice for the dropdown menu, which is created using the shiny package's *selectInput* function.

**GDP per Capita Range Slider**

Next, we have our GDP per capita slider:

```r
# slider to adjust the range of GDP per capita values,
  based on other widget selections
output$gdpSlider <- renderUI({
    df <- getData()
    values = round(df$gdpPercap)
    if(input$logGDP == TRUE){
        values = round(log10(values),4)
    }
    sliderInput("gdpRange", "Select GDP per Capita Range:", min = min(values),
                max = max(values), value = c(min(values),max(values)))
})
```
We load up our dataset in a data frame, then define the slider's possible values, based on whether the user has selected to log-transform the data.

Then, we do some rounding, and create the slider with the *sliderInput* function, setting maximum and minimum values, and defining the value as a range using a vector: `c(min(values),max(values))`.

*Note: If we were to define the value as a single number, the slider would be just a regular slider (see year slider below).*

**Population Range Slider**

The population range slider is very similar to the GDP per capita slider:

```r
# slider to adjust the range of population values, based on other widget selections
output$popSlider <- renderUI({
    df <- getData()
    values = round(df$pop)
    sliderInput("popRange", "Select Population Range:", min = min(values),
                max = max(values), value = c(min(values),max(values)))
})
```

**Year Slider**

The year slider is also rather similar:

```r
# slider to select the year from which data will be selected for plotting
output$yearSlider <- renderUI({
    req(input$file1)
    sliderInput("year", "Select Year:", min = 1952, max = 2007, step = 5,
                value = 1952, sep = "", animate = TRUE)
})
```
*Note: by setting `animate = TRUE`, a play button will be displayed near the slider that will automatically cycle through the slider's value, with no additional user input.*

**Log-Transform Checkbox**

The checkbox widget is rather straightforward:

```r
# checkbox to choose whether to log-transform gdp per capita
output$logCheckbox <- renderUI({
    req(input$file1)
    checkboxInput("logGDP", "Log-transform GDP per Capita?", value = FALSE)
})
```
#### Render Plot

Here's the code we will use to render our Plotly plot:

```r
# render the plotly plot that will be displayed in the main panel on the analysis page
   output$mainPlot <- renderPlotly({

       # make sure some data was uploaded, then generate a data frame from it
       req(input$file1)
       df <- getData()

       # be sure that the gdp slider element is rendered before proceeding
       validate(
           need(input$gdpRange != "", "Loading...")
       )

       # perform some alterations to the data frame based on widget states before
       # plotting the data  
       log = ""
       if(input$logGDP == TRUE) {
         log = "log-"
         df$gdpPercap = log10(df$gdpPercap)
       }
       df = df %>% filter(between(gdpPercap, input$gdpRange[1], input$gdpRange[2]))
       df = df %>% filter(between(pop, input$popRange[1], input$popRange[2]))

       # just a sanity check to make sure there's data to be plotted
       if(nrow(df) == 0) {plot(1,1, pch = 19, col = "red")}

       # if there's some data to plot, plot it
       else{

         # generate a plotly scatter plot from the data
         p = plot_ly(df, x = ~gdpPercap, y = ~lifeExp, type = 'scatter',
                     mode = 'markers', size = ~pop, color = ~continent,
                     sizes = c(10, 50),
                     marker = list(opacity = 0.5, sizemode = 'diameter'),
                     hoverinfo = 'text',
                     fill = ~'',
                     text = ~paste("Country: ", country, "<br>", log,
                                   "GDP per Capita: ", round(gdpPercap,3),
                                   "<br>Life Expectancy: ", lifeExp, sep = ""))

         # adjust the format of the plot
         fig = p %>% layout(title = paste("Life Expectancy vs. ", log,
                                          "GDP per Capita in ", input$year,
                                          sep = ""),
                            xaxis = list(title = paste(log, "GDP per Capita",
                                                       sep = ""),
                                         showgrid = FALSE, zeroline = FALSE),
                            yaxis = list(title = "Life Expectancy",
                                         range = c(25, 85),
                                         showgrid = FALSE, zeroline = FALSE),
                            showlegend = TRUE)

         # return the formatted plotly plot
         fig
       }
   })
```

It seems kind of ominous, but much of the code for the plot is similar to what we've done rendering the UI components: we make sure the data is has been uploaded and that all of the UI components have been rendered first, then do a bit of filtering based on the current widget states.

The vast majority of the second half of this code defines the style of the Plotly plot: we want to have a scatter plot of Life Expectancy vs. GDP per capita, with points whose sizes and colors are defined by their respective countries' populations and continents; we define the message to be displayed when a user hovers their mouse above a given point; and, finally, we format the axes of the plot.

#### Server Summary

We've now defined all of our server logic. To see it all in one place, click [here](https://github.com/JoeKnittel/ShinyDemo/blob/main/Code/server.R).

### global.R

We used three packages within our ui.R and server.R code: **shiny**, **tidyverse**, and **plotly**.

Hence, our global.R will be:

```r
# load up packages that can be accessed by ui.R and/or server.R
library(shiny)
library(tidyverse)
library(plotly)
```

Nothing surprising here.

### about.md

If you recall from above, there's one thing we still have not defined: the **About** page.

In our ui.R file, we stated that when the "About" tab was selected in the navigation bar, we would display the contents of a markdown file called "about.md".

This markdown file will describe to a user the purpose of the app and how to use it, as well as some information about the code used to create the app, and even a link to the page you're currently reading (meta, right?).

Check out the app's about.md file [here](https://raw.githubusercontent.com/JoeKnittel/ShinyDemo/main/Code/about.md).  

## Testing Locally

With all of the necessary code written, let's see our app in action!

### Run App

First, make sure you have any of the ui.R, server.R, or global.R files open in R Studio, then click on the "Run App" button as shown below:

![](/images/run-app-locally.gif)<!-- -->

Doing so will run the app locally (mine ran on port 5609, but yours will likely be different).

Hopefully, your app will load and you'll be able to interact with it. That's not to say everything has been coded properly: you may encounter an error or two along the way, which will probably present in red text within the app.

If this happens, you'll need to review the error messages within the R Studio logs and make the appropriate changes to the code, before proceeding.

### View App in Webpage

When running your app in the previous step, you may have noticed the "Open in Browser" button at the top.

It's a good idea to click this button to see how your app displays within a browser. After all, when you deploy your app, it'll be viewed within a standard web browser.

Test out your app in different browsers by copying the address (e.g., http://127.0.0.1:5609) into Google Chrome or Firefox or whichever browser you think might be used by your intended users to make sure everyone's experience will be as expected.

## Publishing

Once you've tested out your app locally and everything looks good, you're ready to publish your app!

Well, not exactly.

### Update Packages

Remember that when your app is deployed to a server, it'll be generated using (presumably) the most up-to-date versions of the R packages you referenced in your code (in the global.R file).

With that said, even if your app runs properly on your local machine, you may encounter issues deploying your app if your R packages are not updated.

Therefore, to make sure deployment goes smoothly, it's essential that you update all of the R packages you used in your code and any additional dependencies before proceeding to the next step.

### Create a shinyapps.io Account

The best way for a beginner to deploy their Shiny app online is through [shinyapps.io](https://www.shinyapps.io/).

It's free and easy, so this is what we'll use for our app.

Just go to the site and set up an account.

### rsconnect Package

Next, you'll need the **rsconnect** R package. It's on CRAN, so you know what to do: `install.packages("rsconnect")`

Once installed, load the package into your R Studio session: `library(rsconnect)`

### Set Working Directory

Your working directory is probably already your app's directory, but if it's not, run the following code: `setwd(path-to-app)`

### Generate a Token

On shinyapps.io, go to the Admin page and select **Account** $\rightarrow$ **Tokens** (or just click [here](https://www.shinyapps.io/admin/#/tokens)), and then select **Add Token**.

Click **Show**

Click **Copy to clipboard**

See the .gif below, which demonstrates this:

![](/images/token.gif)<!-- -->

### Run the rsconnect Code

Paste what you just copied from shinyapps.io into R Studio and run the code.

It should look something like:

```r
# configure the shinyapps.io account for publishing via r studio
rsconnect::setAccountInfo(name='[your-shinyapps.io-account]',
                          token='[your-token]',
                          secret='[your-secret-code]')
```

### Deploy App

Finally, deploy the app to shinyapps.io with the code: `deployApp()`

This will take some time, but once everything's done, the online app will load in your web browser.

<b><font color = "green">Congratulations, you've just published your first Shiny app!</font></b><br>

Here's the app we just built, live on the web: <a href = "https://joe-knittel.shinyapps.io/life-expectancy-vs-gdp/">https://joe-knittel.shinyapps.io/life-expectancy-vs-gdp/</a>

### Updating the App

If, for whatever reason, you need to update your app in the future, just do the following:

- save your updated code in your ui.R, server.R, and/or global.R files
- make sure your working directory is the app directory using `setwd`
- load up the **rsconnect** package with the code: `library(rsconnect)`
- run: `deployApp()`
- type `Y` in the prompt to update your already-published app on the server

## Other Shiny Apps

You've now gotten a glimpse at what Shiny can do, but there's so much we haven't covered in this introductory tutorial of sorts.

For instance, why not use Shiny to perform reactive statistical calculations based on widget states?

Or, how about allowing a user to upload datasets with different file extensions (e.g., .csv, .tsv, etc.)?

What about displaying $\LaTeX$ equations within the app?

As an example, I've done all three of these in an app that fits a distribution to discrete random variable.

Check out [my other Shiny app](https://joe-knittel.shinyapps.io/fitting-a-discrete-distribution/).

And there are countless other examples in the [Shiny Gallery](https://shiny.rstudio.com/gallery/).

## Summary

I told you this post would be a fun and useful one!

Hopefully, you've learned:

- what the Shiny package is
- why we might use Shiny
- the structure of a Shiny app
- how to develop the UI and server logic for a Shiny app
- how to deploy a Shiny app online

## Coming Up

The last several posts on this blog have been centered around building out a generalized data science toolkit which we can use to import, manipulate, visualize, and interact with data.

With this firm technological foundation in place, we can now begin to use these tools, in conjunction with specialized knowledge regarding insurance and finance, to perform rigorous analyses of various actuarial phenomena.

Specific details have yet to be defined, but stayed tuned for more posts in the not-too-distant future!
