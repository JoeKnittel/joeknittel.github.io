---
title: Getting Started with Python and Jupyter Lab
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-21 06:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](/images/python-jupyter.png)

In this post, we'll do the following:

1. Get started with the programming language called Python, and discuss some of its uses and advantages compared to Excel
2. Install our first Python package
3. Write a bit of Python code
4. Set up a really cool IDE called Jupyter Lab

## Why Python?

We talked about how hundreds of millions of people worldwide use Microsoft Excel to perform various forms of data analysis. Why do we need another program?

It turns out that in many cases Excel works just fine, but here are a few reasons to add Python to one's data analytical arsenal:

- **Automation**: we can automatically perform certain tasks that would have taken us much longer to do in Excel   

- **Visualizations**: with Python, we have better control of how we display charts and tools to create more elaborate plots

- **Big datasets**: scrolling through thousands (millions?) of rows of data in Excel vs. programmatically interacting with our data in Python

- **Free**: as we'll see below, Python is completely free to use

- **Easy to learn**: some programming languages have a steep learning curve, with complex syntax; that's not at all the case with Python, as we'll see later on  

- **Very popular**: by some estimates, there are around 10 million Python developers worldwide

- **Extensible**: since Python is such a popular programming language, there's also a large group of contributors worldwide who are constantly creating new tools to extend the functionality of the program

What do we have to lose? Let's get started with Python!

## Setting up Python

You can download the latest stable version of Python at: https://www.python.org/downloads/

Find the link for your particular operating system, and follow all of the default prompts.

### A Quick Test

Once you have everything installed, head over to your terminal (on Windows, press the `⊞ Win` key and type "cmd", then select "Command Prompt").

You should see something like:

![](\images\cmd.png)

Type "python" and press enter, which should result in (if it doesn't, either you didn't install Python at all, or the program has not been added to your PATH; a quick <a class = "post" href = "https://www.google.com/search?sxsrf=ALeKk02k37KuZe-GXFmpBlwWNXtuQXMgAQ%3A1611245768430&source=hp&ei=yKgJYPiFF4iL5wL4mKv4Dg&q=adding+python+to+path&oq=adding+python+&gs_lcp=CgZwc3ktYWIQAxgAMgUIABDJAzICCAAyAggAMgIIADICCAAyAggAMgIIADICCAAyAggAMgIIADoICAAQsQMQgwE6CAguELEDEIMBOgsILhCxAxDHARCjAjoFCC4QsQM6EQguELEDEMcBEKMCEMkDEJMCOg4ILhCxAxCDARDHARCjAjoFCAAQsQM6AgguOgsILhCxAxDJAxCTAjoFCC4QkwI6DQguELEDEMcBEKMCEAo6CwgAELEDEIMBEMkDUOANWK0dYOgkaABwAHgAgAFxiAGsCpIBBDExLjOYAQCgAQGqAQdnd3Mtd2l6&sclient=psy-ab">google search</a> should help here):

![](\images\python.png)

Finally, just to be sure everything is function properly, let's type `2+2` and see if Python knows its basic math:

![](\images\2+2.png)

<b><font color = "green">Success!</font></b>

### Installing a Package

When you installed Python (if you followed the default prompts, as suggested), you also downloaded the Package Installer for Python, or `pip`, for short.

pip will allow us to very easily extend the functionality of Python, by adding `packages`. Python packages most commonly contain a `library` of Python files called `modules`, and each one serves a different purpose (e.g., use the NumPy package to perform standard statistical calculations)

Statistical calculations? That sounds useful. Let's install the NumPy package and see an example of what it can do.

#### Using pip to Install NumPy

If you're still in the terminal from before, Python is still running, so we need to exit (by typing `exit()` and pressing enter). Next, we'll install NumPy by typing: `pip install numpy`.

Here's approximately what you should be seeing on your screen:

![](\images\numpy.png)

When you press enter, you should see the installation process occur and, if all goes according to plan, you'll encounter no errors.

Let's head back into Python (type `python` and press enter, as we did earlier), then import our new package and give it a nickname ("np") for ease of use going forward.

![](\images\import.png)

Finally, let's use NumPy to generate an array of $100$ sample points $[x_i], \ i \in \\{ 1,2,\dots,100 \\} $ from the standard normal distribution: $X \sim \mathcal{N}(\mu = 0, \sigma^2 = 1)$.

We do so by typing: `np.random.normal(0,1,100)`. And, the result should be something like:

![](\images\array.png)

<font color = "green"> <b>Awesome!</b> </font> 

It may not seem like much, but we're well on our way to doing some pretty cool stuff.

At this point, we need to get set up with an `IDE` (integrated development environment), which is just a program that will make it easy for us to write more elaborate code, instead of just one-off statements in the terminal.

Python comes with an IDE built in, called IDLE. It works (just search for "python" on your computer), but it's highly lacking compared to some of the more modern environments.

Going forward, we'll be using Jupyter Lab as our IDE. Here's a peak at the `UI` (user interface):

![](\images\jupyter-lab.png)

Pretty cool, right! Let's set it up.

## Jupyter Lab

### Installation

#### Base IDE

Installing our new IDE is a breeze: just type `pip install jupyterlab` from the terminal, as we had done when installing the NumPy package.

#### Extensions

Jupyter Lab provides the ability to add extensions (just like you might add a browser extension to Google Chrome). These additional features are pretty useful in a lot of situations, but you can't use them unless you have <a class = "post" href = "https://nodejs.org/en/about/">node.js</a> installed.

If you want to work with Jupyter Lab extensions (including widgets, machine learning applications, sql features, latex enhancements, and an improved suite of visualization libraries) in the future, download node.js at: <a class = "post" href =  "https://nodejs.org/en/download/">https://nodejs.org/en/download/</a> (use the default options when installing)

After installing node.js (which might take a few minutes), we'll be ready to give Jupyter Lab a spin.

### Creating a Jupyter Lab Directory

One last thing before we proceed: let's create a base working directory for our Jupyter Lab.

Find a place on your computer's file system where you normaly keep data analysis stuff and create a new directory for Juypter Lab (e.g., C:\Code\Jupyter).

In the next step, we'll run Jupyter Lab from this directory and it'll provide the base environment for all of our `Juypter notebooks` (i.e., collections of code interspersed with markdown elements).

## Loading Jupyter Lab

Running Juypter Lab requires just two steps (don't forget to exit out of Python first, if you're still in it from before, then proceed):

1. Open the terminal and change directory ("cd") to your new Jupyter directory from the previous step (e.g., `cd C:\Code\Jupyter`)

2. Type `jupyter lab`

That's it! Your default web browser should open and be directed to a local web server running at the address: http://localhost:8888/lab.

You should see something similar to the UI pictured in the previous section.

## Summary

We have now:

- Installed Python
- Learned how to and practiced installing additional Python packages using pip
- Installed an IDE called Jupyter Lab
- Learned how to launch the Jupyter Lab environment

## Coming Up

The next post will be very much an extension of this post: We'll describe the components of our new Jupyter Lab working environment, then get right into creating our first notebook. We'll then deploy the notebook in a self-contained execution environment. Tune in next time to find out what that means and why it's useful.
