---
title: "Binder : Enabling Reproducible Data Analysis by Leveraging the Cloud"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-25 07:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](\images\binder-logo.png)

Last time, we got an introductory taste of what it's like to do work with data in Jupyter Lab. We described the UI and created our first notebook in the spirit of literate programming, utilizing both Python code and markdown cells.

We ended, however, on kind of a downer, describing some challenges to reproducing our work with our current method of dissemination (sending our .ipynb files directly to other people).

With that said, there was still hope: we introduced the concept of `Binder`, a seemingly miraculous solution to our problem that uses cloud computing to create a self-contained execution environment identical to our own.

In this post, we'll do the following:

1. Give a high-level overview of Binder
2. Describe the necessary steps one must take in order to utilize the service
3. Deploy our notebook in the cloud and share it with others

## Overview of Binder

As we've described earlier, Binder solves the issue of reproducibility by leveraging cloud resources. So, how does this all work?

Have a look at the diagram below, and then we'll try to dig into what's going on.

![](\images\binderhub.png)

Here's the overall idea, from the perspective of a data science practitioner, as simply as I can express it:

1. Write some code in a Jupyter notebook and create a $\text{requirements.txt}$ file saying which packages are needed to run the notebook, then place these files in a GitHub repository.

2. Direct the `BinderHub` to your repository (and, specifically, the notebook), so a `container` can be built and stored in a registry for future processing.

3. BinderHub generates a link which can then be shared with whomever.

4. When someone clicks the link you sent them, `JupyterHub` provides them with access to a temporary instance of the Jupyter notebook (with all dependencies installed), run not on their local machine, but by the pooled resources (compute and memory) of a collection of machines in the cloud (a `Kubernetes cluster`).

Got all that? Well, it doesn't matter too much whether you have a well-defined conception of *everything* (e.g., what a BinderHub, JupyterHub or container are) that's going on behind the scenes. I've provided this overview moreso for the sake of completeness than out of necessity to we, the users of the Binder service.

All you really need to know is described in the next section.

## All You Really Need to Know

There was a lot going on in the last section, so let's consider just the elements of technology that we need to worry about.

The following are required in order to use Binder:

1. GitHub account
2. Repository containing the following:
   - Jupyter notebook
   - $\text{requirements.txt}$ file, which lists the code's required packages

Once we've set up a GitHub repository, we just need to direct BinderHub to our code, and it'll generate a container and a link for us to share with others.

If someone happens to visit the link, they'll have access to our code in a self-contained execution environment identical to the one we used to do our analysis.

Okay, that sounds much simpler. Let's get started!

## Building our Binder

### GitHub

The first thing we need in order to build our Binder is to have a `GitHub` account.

If you're unaware of what <a href = "https://en.wikipedia.org/wiki/GitHub">GitHub</a> is, it's a free software development tool that assists in collaborative version control (helping people keep track of past changes and further update software), among other features (e.g., this blog is hosted on GitHub Pages).

#### Create Account

Setting up an account with GitHub shouldn't take more than a minute or two; just head over to <a href = "https://github.com/join">https://github.com/join</a>, fill out the form, and you'll be ready to go.

#### Create Repository

Alright, I'm assuming you now have a GitHub account. Let's now proceed to create a repository for our project.

To be clear, a `repository` is just a fancy name for a directory: it's a place to store various files.

You can create a new repository by, first, clicking on the <i>Repositories</i> tab in your profile, then clicking the <i>New</i> button in the upper right-hand part of the screen. This can also be done directly by clicking <a href = "https://github.com/new">here</a> (assuming you're logged into your GitHub account).

There are a few options for how you wish to configure your your repository, but all that's necessary for our purposes is that the repo be set to <i>Public</i>.

See the image below for how I've set up my example repo:

![](\images/new-repo.png)

Once everything's set to your liking, go ahead and click <i>Create repository</i>.

### Requirements.txt File

We now have a GitHub repo and a Jupyter notebook, but what's this $\text{requirements.txt}$ you keep seeing?

The $\text{requirements.txt}$ file is one of <a href = "https://mybinder.readthedocs.io/en/latest/using/config_files.html">many possible configuration files</a> the BinderHub uses to guide it in its creation of containerized image of our notebook.

The file is just a list of the Python packages we utilize in our code, with a specific version number for each package.

If you can recall, we used three packages in our notebook: pandas, numpy, and matplotlib.

Now, we just need version numbers.

#### Which Package Version?

To see what version of your Python packages are installed, go to your terminal and type: `pip list` and find the lines pertaining to our packages.

Here's approximately what you'll see:

```text
.
.
.
matplotlib             3.3.3
.
.
.
numpy                  1.19.5
.
.
.
pandas                 1.2.0
.
.
.
```

Perfect, let's proceed.

#### Creating Requirements.txt

We have our version numbers, so let's create $\text{requirements.txt}$.

Open your favorite text editor and type the following three lines (where the version numbers correspond to those on your machine, not mine):

```text
matplotlib==3.3.3
numpy==1.19.5
pandas==1.2.0
```

Now, save the file as $\text{requirements.txt}$ in your Jupyter Lab directory, alongside your notebook.

*Alternative Configuration*

We could have used `>=` instead of `==` for the package version numbers. The way we've set up the file currently requires that the `pods` (or, machines in the cloud responsible for serving up our notebook to others) be configured to have <i>exactly</i> the same setup as ours, rather than having the most up-to-date version of our packages that are available at the time of processing.

### Upload Files to GitHub Repository

Uploading files to your repo can be done in a number of ways. We'll stick to the easiest (using the GUI on the GitHub website), as described below:

$\text{GitHub Profile} \rightarrow \text{Repositories} \rightarrow \text{[your-repository]} \rightarrow \text{Add file} \rightarrow \text{Upload files}$

Once there, just drop in the files, as shown below:

![](\images/upload.gif)


### Building a Container

With all that's necessary to generate a Binder, let's head over to <a href = "https://mybinder.org/">mybinder.org</a> and build a container for our code.

Once on the Binder site, fill out the form as below:

![](\images\binder-site.png)

1. Type in the URL to your GitHub repository
2. Type in the filename of your Jupyter notebook
3. Copy the link to your Binder
4. Copy the Binder badge for a clickable button link Binder (optional)

Once you've filled out the form correctly (and saved the link(s) for future use), click the <i>launch</i> button. It'll take a few moments to build a container, but just let it do its thing.

After the build is complete, you will be directed to temporary instance of your notebook, served through the cloud.

## Accessing the Binder

Send out the link (or Binder badge) from the previous section to whomever, and they'll have access to your analysis in a computing environment identical to the one you had when creating it.

Here's a link to my Binder: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/JoeKnittel/BinderDemo/HEAD?filepath=analysis.ipynb)

## Some Things to Keep in Mind

- Remember, there's a lot going on in the background to allow all of this to work properly, so don't be surprised if it takes awhile for your container to be built (previous step) or for the temporary notebook server instance to load for other people.

- If ever you need to update the notebook, just <a href = "https://git-scm.com/docs/git-push">push</a> the updated .ipynb (and $\text{requirements.txt}$ file, if it needs to be changed) to your repo and a new container will be built, allowing viewers access to the updated notebook.

- Fortunately, all of this cloud computation is done for free! With that said, one must realize there are limitations to the memory and compute power available to run our code. If we need to, for instance, train a 100-layer convolutional neural network within our notebook, we might need to seek other options.

## Coming Up

We've now cultivated the basic know-how to perform reproducible data analysis using Python (with the help of Jupyter Lab and Binder).

In the next post, we'll continue to build out our data science toolkit by setting up a program I like using even more than Python--it's called R.
