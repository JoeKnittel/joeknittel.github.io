---
title: "Our First Jupyter Notebook, Deployed with Binder"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-21 06:02:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](\images\jupyter-binder.png)

Last time, we set up Python on our computer, tested out some very basic code, installed a useful package, and set up Jupyter Lab.

We'll continue where we left off and accomplish the following by the end of this post:

1. Install a few more useful packages (as a review, and for our future projects' sake)
2. Get a good understanding of the layout and most crucial features of Jupyter Lab
3. Compose our first Jupyter notebook, intertwining Python code with bits of markdown
4. Deploy our notebook in a self-contained execution environment called Binder, so everyone with whom we share our code experiences it in an identical fashion

## More Python Packages

Currently, we have two Python packages installed: NumPy and Jupyter Lab.

That's a good start. With NumPy, we can work with arrays, simulate random numbers, perform some basic statistical calculations, and even do linear algebra. And we can put all of those things into action in our new IDE, Jupyter Lab.

But to do some more useful analyses, we'll need a few more tools, so let's just follow the same package installation procedure as last time, then we'll jump back into Jupyter Lab.

### Pip to the Rescue once More

If you can recall from last time, we installed both of our packages in the same way, by opening up our terminal and typing:

`pip install [package-name]`

In almost all cases, this will be how we install new Python packages.

#### Pandas Package

The <a href = "https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html">pandas package</a> allows us to work with tabular data in what are called `dataframes`.

![](\images\df.png)

Looks a lot like a spreadsheet, or information in a database, or even a table on a website, right? Well, it basically is.

The pandas package lets us load up external data from any of these sources into a dataframe, and then manipulate and perform calculations on the data just like in Excel, for instance.

Sound good? Then give it an install: `pip install pandas`

#### Matplotlib Package

How about some charts? Everyone likes a good bar chart or scatterplot. Or, at least, I do. If not, maybe a histogram or a box plot will suit your fancy.

In any case, I'm sure we can agree that data analysis is far from complete without a graphing library.

<a href = "https://matplotlib.org/gallery/index.html">Matplotlib</a> serves this purpose and is well worth the quick install: `pip install matplotlib`

## Composing a Jupyter Notebook

Alright, those package should prove to be useful in the near future. Let's open up Jupyter Lab so we can have some fun with our new tools.

### Loading Jupyter Lab

Just a reminder, we need to change directory in our terminal to the base Juypter directory we constructed last time, then type `jupyter lab`

### Describing the UI

After a few seconds of loading, we should be back to the screen you saw at the end of the last post (it was somewhat of a cliffhanger, I know).

Let's talk about what you're seeing on the screen.

#### Launcher

When you first load up Jupyter Lab, most of the screen will be comprised of the `Launcher`, where you'll be able to do any of the following (among other things):

- **Create a Python notebook**: with this type of file, you'll be able to write blocks of code called `cells`, run the cells of code individually or all at once, and write comments, formatted in any style you can imagine
- **Open the Python Console**: this is just like when we ran the Python code `2+2` in the terminal; we can run one-off code statements from here
- **Open a Terminal**: here, we can do our `pip install ...` statements whenever we need a new Python library
- **Create a Markdown File**: markdown is a markup language used to quickly create formatted text (I'm using it right now to generate this blog post, and we'll discuss it in more detail in a later section)

Once you choose an option within the Launcher, your choice file format will take the center stage and the Launcher will disappear.

If ever you need to launch a new file, just click the blue "+" atop the file browser in the upper-left-hand corner of the screen, and it'll open the Launcher in a new window.

#### Left Sidebar

On the far left side of the screen, you'll see a number of icons. They control the display of the `Left Sidebar`.

Click the different icons to display the various fields.

If you click the currently selected icon and the Left Sidebar will collapse, letting the main work area take up most of the screen.

Options include:

- **File Browser**: The default view is the `File Browser`, which is pretty self-explanatory. The File Browser contains all of the directories and files within your base Jupyter path. Navigate it just like you would any other file browser.
- **Terminals and Kernels**: Here, you can control the terminal, which we've already discussed. You can also control the `kernel` running within a Jupyter notebook, which is just the notebook's code interpreter.
- **Table of Contents**: Get a quick overview of your file here. No surprises.
- **Extensions**: We briefly hinted at the extensibility of Jupyter Lab. This is where you can add new extensions.

#### Everything Else

There are a bunch of other elements of the UI, but most are either self-explanatory or not that crucial to our purposes.

In any event, if we come across something we haven't discussed previously, it'll be described in more detail when the time comes.

### What is a Jupyter Notebook, Anyway?

### An Introductory Exercise in Literate Programming



## Binder

## Summary

## Coming Up
