---
title: "Composing Our First Jupyter Notebook"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-23 06:02:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---


![](\images\jupyter_lab_logo.png)

Last time, we set up Python on our computer, tested out some very basic code, installed a useful package, and set up Jupyter Lab.

We'll continue where we left off and accomplish the following by the end of this post:

1. Install a few more useful packages (as a review, and for our future projects' sake)
2. Get a good understanding of the layout and most crucial features of Jupyter Lab
3. Compose our first Jupyter notebook, intertwining Python code with bits of markdown

## More Python Packages

Currently, we have two Python packages installed: NumPy and Jupyter Lab.

That's a good start. With NumPy, we can work with arrays, simulate random numbers, perform some basic statistical calculations, and even do linear algebra. And we can put all of those things into action in our new IDE, Jupyter Lab.

But to do some more useful analyses, we'll need a few more tools, so let's just follow the same package installation procedure as last time, then we'll jump back into Jupyter Lab.

### Pip to the Rescue, Once More

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

- **Create a Python notebook**: With this type of file, you'll be able to write blocks of code called `cells`, run the cells of code individually or all at once, and write comments, formatted in any style you can imagine.

- **Open the Python Console**: This is just like when we ran the Python code `2+2` in the terminal; we can run one-off code statements from here.

- **Open a Terminal**: Here, we can do our `pip install ...` statements whenever we need a new Python library.

- **Create a Markdown File**: Markdown is a markup language used to quickly create formatted text (I'm using it right now to generate this blog post, and we'll discuss it in more detail in a later section).

Once you choose an option within the Launcher, your choice file format will take the center stage in the `Main Work Area` and the Launcher will disappear.

If ever you need to launch a new file, just click the blue "+" atop the file browser in the upper-left-hand corner of the screen, and it'll open the Launcher in a new window.

#### Main Work Area

This is just the center region of the UI. Most of the time, it will take up the majority of the screen and will be where you will write your code and markdown.

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

So, what *is* a Jupyter notebook and what can we do with this thing?

In short, a Jupyter notebook is a virtual environment wherein `literate programming` resides.

The idea of literate programming harkens back to a <a href = "https://en.wikipedia.org/wiki/Turing_Award">Turing Award</a>-winning computer scientist named <a href = "https://en.wikipedia.org/wiki/Donald_Knuth">Donald Knuth</a>, who developed the typsetting system $\TeX$, which we'll use in our future notebooks. Knuth saw programs as literary works, stating:

>"Instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want a computer to do."

This exact idea has been brought to bear with Jupyter notebooks, as we intertwine cells of code, markdown text, and images to explain to our reader the intent of each element of our analysis.

Now that we know what a Jupyter notebook is, let's create one!

### An Introductory Exercise in Literate Programming

Our first Jupyter notebook will contain four components:

1. Python code to perform the analysis
2. A few markdown elements to describe our code and create section headings
3. Optional images, illustrating our analysis
4. Any additional styling elements (e.g., HTML and/or CSS), if desired

#### Launch a New Jupyter Notebook

First things first, let's launch a new notebook. To do so, go to the Launcher (it should already be front and center, if you're where we left off in the previous section; if not, click the blue "+" in the left sidebar, as described previously).

Since you only have one kernel at this point (the one for Python), you won't be presented with an option to choose a kernel, but, as you'll see in future posts, once you have multiple kernels installed, you'll have to tell Jupyter Lab which one to use before getting started.

#### Name our Notebook

You can save your notebook using the shortcut: `Ctrl+Shift+S` or by using the `Menu Bar` way up top: `File -> Save Notebook As...`. Go ahead and give your notebook a name, noticing the file extension, ".ipynb". All of our Jupyter notebooks will have this file extension.

#### Cells

Notice the tab containing our notebook is named with its new name, but also notice just below the `Tab Menu` at the top of the notebook the `Cell Menu` shown here:

![](\images\top-of-jupyter.png)

The icons in the Cell Menu are fairly self-explanatory. You can copy and paste cells like text in a Word document. You can run the code or markdown in a set of selected cells. You can reset the kernel if something goes majorly wrong during code execution.

But take particular notice of the dropdown menu at the right of the menu: this is where you tell the kernel a cell's type, so it knows how to process it.

Also, notice the one and only cell, highlighted in blue. This shows that the cell is currently selected. Code cells also have a set of square brackets $[ \ \ ]$ which will look like $[*]$ while processing and, once executed, it'll look like $[i]$, where $i$ represents the $i^{th}$ cell of code that's been executed.

#### Headings

Let's give our notebook a title heading using a markdown cell.

To do so, click the aforemention dropdown menu which currently says "Code", and change it to "Markdown".

Type "# My First Jupyter Notebook" (or something similar) into the highlighted cell (don't forget the #, and don't include the quotes).

Then, press `Shift+Enter` or click the `Run` icon in the Cell Menu.

You should see cell's miniscule text miraculously transform into something rather majestic:

![](\images\heading.png)

That one little hashtag (#) is what told the notebook to generate an HTML <$\text{h1}$> tag, which is rendered as a large heading in a web document.

This is just one of the <a href = "https://www.markdownguide.org/basic-syntax/">many ways</a> we can quickly and easily transform our plain text into formatted text using markdown cells.

#### Writing Code

Let's load up some data into a dataframe using pandas and a sample dataset found <a href = "https://people.sc.fsu.edu/~jburkardt/data/csv/csv.html">online</a>:

`import pandas as pd`

`data = pd.read_csv("https://people.sc.fsu.edu/~jburkardt/data/csv/hurricanes.csv")`

The first line allows us to use all of the modules in the pandas library (which, as you may recall, is used for working with tabular data) and the second loads the data from the .csv file into a dataframe called $\text{data}$, using the read_csv function from the pandas library.

You can put these two lines of code in one cell or two, depending on your preference. Either way, you should get the same result when you run the code.

To make sure the dataframe was generated correctly, try displaying the dataframe by running the code `data`. You should see something like this so far:

![](\images\dataframe.png)

The data contained in our .csv file represent the amount of tropical storms and hurricanes arising in the Atlantic Ocean, organized by month, and from 2005-2015. We also see an "Average" column, which represents the monthly average of the data for each month over the represented years.

Let's calculate the total amount of storms in each year with this code:

```python
import numpy as np
df = data.iloc[:,np.r_[2:len(data.columns)]]
df.columns = ['2005', '2006', '2007', '2008', '2009', '2010',
              '2011', '2012', '2013', '2014', '2015']
yearly_totals = data.sum(axis=0)
```

Here, we imported the numpy package, used it to select only the year columns of our dataset (which we renamed to $\text{df}$ for ease of usage), renamed the column names, then summed over the 0-axis (columns) to get our totals.

If you type `yearly_totals` into a cell below it and run it, you can see the column sums.

Now, let's generate a quick bar plot:

```python
from matplotlib import pyplot as plt
plt.title('# Hurricanes and Tropical Storms in the Atlantic')
yearly_totals.plot.bar()
```

You should see something like this after running the cell:

![](\images\bar.png)

#### Additional Formatting

At this point, let's conclude our analysis. Obviously, we can look into some other useful statistics and generate some additional cool plots, but this post was really just about learning the basics.

Before we finish up, though, let's make some formatting alterations. First, let's change our title heading to something more meaningful. I went with "Atlantic Hurricanes from 2005-2015".

Then, let's reorganize our code a bit. In my code, I placed the numpy and matplotlib import statements in the top cell, alongside the pandas import statement.

To add further embellishment, we can even add images to our notebook. I decided to place <a href = "https://www.cummins.com/sites/default/files/styles/newsroom_hero_image/public/images/newsroom_article/Cummins%20-%202020%20Hurricane%20Season%20-%20What%20to%20Expect%20-%20Web.jpg?itok=OIDc9N_N">this image</a> right below our title heading to show what our data represent.

You can add an image to your notebook with a markdown cell containing the following text:

`![](path-to-image-file)`

The path can be to a file stored locally on your computer's hard drive or to an online image.

#### Revised Notebook

Here's how mine turned out:

![](\images\first-jupyter-notebook.gif)

Looks pretty good for a first notebook.

For extra credit, try adding some HTML tags to your markdown cells to provide even more elaborate formatting.

Other than that, our first notebook is complete. Let's see how we can share our analysis with other people.

## Sharing a Jupyter Notebook

### Send the .ipynb File Directly

We currently have a fully-functional Jupyter notebook that uses a few Python packages and manipulates, calculates statistics, and generates a plot from data stored in a .csv file online.

Our analysis (i.e., our notebook, the .ipynb file) can now be shared with anyone by just sending them the .ipynb file (or the entire contents of the directory in which it resides, just in case you reference local images). <a href = "https://downgit.github.io/#/home?url=https://github.com/JoeKnittel/joeknittel.github.io/tree/main/notebooks/jupyter-notebook-demo">Here</a>'s a link to my directory, containing the Jupyter notebook and the locally-referenced image.

## Coming Up

While it's fantastic to be able to just send a .ipynb file to someone to let them see your analysis, there are a few slight problems: The person to whom you send your analysis must have Python and Jupyter installed on their computer in order to execute the code in the file's cells. Additionally, any packages you import in your notebook must also be installed on your co-worker's/co-collaborator's computer. This severely limits the reproducibility of the analysis.

To address this issue of reproducibility, our next post will be dedicated to hosting our Jupyter notebooks for free online in a self-contained execution environment (i.e., a docker image), using a service called Binder. In doing so, our code will be accessible and--crucially--executable to anyone on the planet who has internet access.
