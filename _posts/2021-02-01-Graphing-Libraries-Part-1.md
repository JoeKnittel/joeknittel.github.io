---
title: "Graphing Libraries, Part 1 : Interactive Plots Using R and Python"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-02-01 07:02:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](\images\viz-1-top.png)

We concluded the last post with the construction of a choropleth map using R's most famous graphing library, ggplot2, in conjunction with some additional packages from the Urban Institute. It was a demonstration that, with just a few lines of code, we can create compelling visualizations from our data.

In this post, we'll take data visualization to the next level, as we explore some of my favorite R and Python packages that provide interactivity (i.e., the user can directly manipulate the visualizations).

We'll consider three libraries from R (plotly, Highcharter, dygraphs) and three from Python (Pygal, bokeh, Altair) and, for each, we'll do the following:

- Describe some use cases for the library
- Install the package
- Produce an example plot
- Discuss what the code does

Let's start with the R packages, then we'll turn our attention to those of Python.

## plotly

<a href = "https://plotly.com/r/">Plotly</a> is perhaps the most commonly used interactive data visualization library for R (and <a href = "https://plotly.com/python/">Python</a>!).

The package provides tools for generating basic interactive plots (scatterplots, line plots, bar charts, etc.), statistical charts (histograms, box plots, violin plots, etc.), scientific plots (contour plots, heatmaps, network graphs, etc.), financial charts (time series, candlestick charts, etc.), maps (choropleth maps, scatter plots on maps, etc.), and 3-D charts, as well as subplots and animations.

Needless to say, it's a very powerful library and it deserves a place in any R programmer's toolkit.

### Installation

```r
install.packages("plotly")
```

### Example Plot: Two-Dimensional Histogram Contour

```r

library(plotly)

cnt <- with(diamonds, table(cut, clarity))

fig <- plot_ly(diamonds, x = ~cut, y = ~clarity)
fig <- fig %>%
  add_trace(
    type='histogram2dcontour',
    contours = list(
      showlabels = T,
      labelfont = list(
        family = 'Raleway',
        color = 'white'
      )
    ),
    hoverlabel = list(
      bgcolor = 'white',
      bordercolor = 'black',
      font = list(
        family = 'Raleway',
        color = 'black'
      )
    )
)

fig
```

<iframe src="\interactive plots\plotly-example.html" width="700" height="600" frameBorder="0"></iframe>

In the above plot, we constructed a two-dimensional histogram contour for a dataset containing thousands of diamonds. The plot focuses on two key variables (cut and clarity), and looks to depict their joint distribution over the entire dataset.

A quick glance at this map might remind you of a contour map of a mountain, with a peak of 4500 at the point (Ideal, VS2). If you have good spatial awareness, you should be able to visualize the entire three-dimensional mountain, which corresponds, in structure, to the joint probability distribution.

We could represent this information with a table, but the contour plot, in my opinion, does a way better job of bringing the distribution to life.

Note that about half of the code is for styling the hover effect. The dirty work is done with just a few lines.

## Highcharter

The next library we'll look at is <a href = "https://jkunst.com/highcharter/">Highcharter</a>, which is a <a href = "https://en.wikipedia.org/wiki/Wrapper_function">wrapper</a> for <a href = "https://www.highcharts.com/">Highcharts.js</a>, a commonly-used JavaScript visualization library.  

The package can be used to create an assortment of line charts, scatterplots, bar charts, heatmaps, treemaps, etc., and it's really useful whenever you want to compare sub-groups of a dataset.

### Installation

```r
install.packages("highcharter")
```

### Example Plot: Sub-group Scatterplot

```r
library(highcharter)
remotes::install_github("allisonhorst/palmerpenguins")
data(penguins, package = "palmerpenguins")

hchart(penguins, "scatter", hcaes(x = flipper_length_mm, y = bill_length_mm,
                                  group = species))
```
<iframe src="\interactive plots\highcharter-example.html" width="700" height="600" frameBorder="0"></iframe>

The above plot was generated with just one line of code (the first few lines were just loading the package and getting some data from another package)!

Run your mouse over the plot to see it in action. Whenever you mouseover a point in a specific sub-group, only the points within its same group are highlighted, allowing us to observe each sub-group in further detail. There's even a very clean tooltip, displaying information for the individual point currently being highlighted.

It can't be understated how useful it is to be able to generate such a compelling plot with so little code.  

## dygraphs

Just like the previous package, the <a href = "http://rstudio.github.io/dygraphs/index.html">dygraphs package</a> is a wrapper for its respective homonymous <a href = "https://dygraphs.com/">JavaScript library</a>.

This package is not as versatile as the first two packages we've already discussed, but it's really useful for visualizing time series data.

### Installation

```r
install.packages("dygraphs")
```


### Example Plot: Time-series Plot with Prediction Interval

We'll need a bonus package (<a href = "https://www.r-graph-gallery.com/38-rcolorbrewers-palettes.html">RColorBrewer</a>) for this example, which will generate a color palette. You'll probably use this package in many other cases going forward, so give it an install:

```r
install.packages("RColorBrewer")
```

Now, our example:

```r
library(dygraphs)
hw <- HoltWinters(ldeaths)
predicted <- predict(hw, n.ahead = 72, prediction.interval = TRUE)

dygraph(predicted, main = "Predicted Lung Deaths (UK)") %>%
  dyAxis("x", drawGrid = FALSE) %>%
  dySeries(c("lwr", "fit", "upr"), label = "Deaths") %>%
  dyOptions(colors = RColorBrewer::brewer.pal(3, "Set1"))
```
<iframe src="\interactive plots\dygraphs-example.html" width="650" height="600" frameBorder="0"></iframe>

In the above plot, we took a time-series of lung-related deaths in the UK from 1974-1979, fit a Holt-Winters additive model (which allow us to capture seasonality in the data) to the data, then plotted the model's predictions for the next 72 months, including a prediction interval.

Give the plot a mouseover to see it in action. I'm rather fond of the particularly clean effect: all of the pertinent information that one might expect is displayed, with no bloat.

## Pygal

We'll now shift our attention to Python libraries, starting with <a href = "http://www.pygal.org/en/stable/index.html">Pygal</a>.

The package provides a fairly nice collection of built-in plots, including (but not limited to) bar charts, line charts, pie charts, and radar charts.

And when it comes to style, Pygal provides some customizability, offering a few options. This can't be said of a lot of other graphing libraries in R and Python, so it's good to have.

### Installation

```python
pip install pygal
```

^ reminder, we're in Python now!

### Example Plot: Normalized Stacked Bar Chart

```python
import pygal
line_chart = pygal.StackedBar()
line_chart.title = 'Browser usage evolution (in %)'
line_chart.x_labels = map(str, range(2002, 2013))
line_chart.add('Firefox', [None, None, 0, 16.6, 25, 31, 36.4, 45.5, 46.3, 42.8, 37.1])
line_chart.add('Chrome', [None,None,None, None, None, None, 0, 3.9, 10.8, 23.8, 35.3])
line_chart.add('IE', [85.8, 84.6, 84.7, 74.5, 66, 58.6, 54.7, 44.8, 36.2, 26.6, 20.1])
line_chart.add('Others', [14.2, 15.4, 15.3, 8.9, 9, 10.4, 8.9, 5.8, 6.7, 6.8, 7.5])
line_chart.render_in_browser()
```
<iframe src="\interactive plots\pygal-example.html" width="700" height="540" frameBorder="0"></iframe>

In the plot above, we see a normalized stacked bar chart of different browser usage over time (clearly not up-to-date, since <a href = "https://gs.statcounter.com/browser-market-share">Chrome has taken over</a> much of the market).

It's probably a plot type you're familiar with, but perhaps you've never seen such a chart with Pygal's style. I'm particularly fond of the package's unique mouseover effect and default color scheme, so whenever it's appropriate to go for something not so conventional, Pygal is worth consideration, in my opinion.

## bokeh

<a href = "https://docs.bokeh.org/en/latest/index.html">bokeh</a> is a very versatile and powerful graphing library for Python.

One of the coolest things about bokeh is that it allows one to create dashboards, by utilizing widgets (e.g., slider, dropdown menu, etc.). This provides the user with an extensive depth of data interactivity.

But not every plot is dashboard; using bokeh, we can also create almost any of the aforementioned interactive plots from other packages.

This one is a no-doubter to be included in any Python programmer's toolkit.

### Installation

```python
pip install bokeh
```

### Example Plot: Calendar Heatmap

```python
from math import pi

import pandas as pd

from bokeh.io import show
from bokeh.models import BasicTicker, ColorBar, LinearColorMapper, PrintfTickFormatter
from bokeh.plotting import figure
from bokeh.sampledata.unemployment1948 import data

data['Year'] = data['Year'].astype(str)
data = data.set_index('Year')
data.drop('Annual', axis=1, inplace=True)
data.columns.name = 'Month'

years = list(data.index)
months = list(data.columns)

# reshape to 1D array or rates with a month and year for each row.
df = pd.DataFrame(data.stack(), columns=['rate']).reset_index()

# this is the colormap from the original NYTimes plot
colors = ["#75968f", "#a5bab7", "#c9d9d3", "#e2e2e2", "#dfccce",
          "#ddb7b1", "#cc7878", "#933b41", "#550b1d"]
mapper = LinearColorMapper(palette=colors, low=df.rate.min(), high=df.rate.max())

TOOLS = "hover,save,pan,box_zoom,reset,wheel_zoom"

p = figure(title="US Unemployment ({0} - {1})".format(years[0], years[-1]),
           x_range=years, y_range=list(reversed(months)),
           x_axis_location="above", plot_width=900, plot_height=400,
           tools=TOOLS, toolbar_location='below',
           tooltips=[('date', '@Month @Year'), ('rate', '@rate%')])

p.grid.grid_line_color = None
p.axis.axis_line_color = None
p.axis.major_tick_line_color = None
p.axis.major_label_text_font_size = "7px"
p.axis.major_label_standoff = 0
p.xaxis.major_label_orientation = pi / 3

p.rect(x="Year", y="Month", width=1, height=1,
       source=df,
       fill_color={'field': 'rate', 'transform': mapper},
       line_color=None)

color_bar = ColorBar(color_mapper=mapper, major_label_text_font_size="7px",
                     ticker=BasicTicker(desired_num_ticks=len(colors)),
                     formatter=PrintfTickFormatter(format="%d%%"),
                     label_standoff=6, border_line_color=None, location=(0, 0))
p.add_layout(color_bar, 'right')

show(p)      # show the plot

```
<iframe src="\interactive plots\bokeh-example.html" width="700" height="500" frameBorder="0"></iframe>

There's a lot going on code-wise in the above plot, but it's not difficult to see the utility of its result.

This is a calendar heatmap, wherein each rectangle corresponds to a specific month over the span of several decades. There are some clear elements that jump out at the viewer: specifically, the low unemployment rate around 1953 (post-WWII growth) and the high unemployment rates around 1982 (recession induced by tightened monetary policy) and around 2009 (the Great Recession). We also get a good view of intra-yearly patterns throughout the dataset.

That I chose to exhibit this plot is not particularly meaningful; there will be tons of opportunities for exploring the different types of plots bokeh is capable of producing (including those that contain widgets).

## Altair

Last but not least, we have <a href = "https://altair-viz.github.io/gallery/index.html">Altair</a>, which is one of the more commonly-used graphing libraries for Python.

The package provides tools for creating a vast array of static plots (e.g., bar charts, histograms, line charts, etc.), but it can also be used to create several types of interactive plots.

## Installation

```python
pip install altair vega_datasets
```

## Example Plot:

```python
import altair as alt
from vega_datasets import data

source = data.cars()

brush = alt.selection(type='interval')

points = alt.Chart(source).mark_point().encode(
    x='Horsepower:Q',
    y='Miles_per_Gallon:Q',
    color=alt.condition(brush, 'Origin:N', alt.value('lightgray'))
).add_selection(
    brush
)

bars = alt.Chart(source).mark_bar().encode(
    y='Origin:N',
    color='Origin:N',
    x='count(Origin):Q'
).transform_filter(
    brush
)

points & bars
```
<iframe src="\interactive plots\altair-example.html" width="600" height="484" frameBorder="0"></iframe>

Upon first glance, the plot above is pretty cool, as it contains both a scatterplot and histogram of the dataset, broken up by a class.

One can instantly see several patterns emerge, including the general distribution for each individual class, the overall trend of mpg vs. horsepower, and the percent of the market deriving from each class.

But click and drag to select a rectangular region of the scatterplot and things really come alive. We can now hone into a certain subset of the space to investigate patterns within just the region we care about. This is a rather powerful, and the plot has a top-notch user experience, to boot.

## Summary

Well, that was fun! We've now installed and investigated example use cases for a number of really cool data visualization libraries in R and Python.

These tools make it incredibly easy to produce compelling interactive plots, which elucidate essential patterns in the data that might not otherwise have been noticed.

One thing to consider is that many of these packages were first developed in JavaScript, as all major browsers have a built-in JavaScript engine. With that said, if we seek further customization beyond what is provided in the packages described in this post, we'll have to look into JavaScript libraries (e.g., <a href = "https://d3js.org/">D3.js</a>). This will certainly be a topic of future blog posts.

## Coming Up

Next time, we'll talk about databases, how to configure R to connect to them, and how to explore them using SQL commands.
