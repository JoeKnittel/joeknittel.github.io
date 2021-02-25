---
title: "Data Analysis With MS Excel, Part I: Some Useful Functions"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-18 04:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](/images/excel logo.png)

Microsoft Excel is ubiquitous. The program is <a class = "post" href = "https://www.google.com/search?sxsrf=ALeKk00ODJ7TqYafhlPlw5BPYjlpyOTlrQ%3A1611010725599&source=hp&ei=pRIGYLKPIoHr_Qa35Y3QCw&q=how+many+people+use+excel&oq=how+many+people+use+excel&gs_lcp=CgZwc3ktYWIQAzoOCC4QsQMQxwEQowIQkwI6CAgAELEDEIMBOgUIABCxAzoLCC4QsQMQxwEQowI6AgguOgIIADoLCAAQsQMQgwEQyQM6CAguELEDEIMBOgUILhCxAzoFCAAQyQNQKFjDHmDcH2gAcAB4AIABfIgBzw-SAQQyMS40mAEAoAEBqgEHZ3dzLXdpeg&sclient=psy-ab&ved=0ahUKEwjy9ZrQyqbuAhWBdd8KHbdyA7oQ4dUDCAk&uact=5">used by 750 million people worldwide</a> in just about any field imaginable to do everything from creating a quick chart to complex data analysis. You can even <a class = "post" href ="https://www.instructables.com/Tetris-on-Excel/">play tetris</a> with Excel! It's a wonderful program, and I've been enjoying experimenting with its different uses since the mid-'90s.

In this first of what will likely be a truly compelling series of posts (\*sarcasm\*) on Excel, we'll do the following:

1. Describe what a function is
2. Talk about how functions are used in Excel
3. Learn about some of the more common and/or particularly useful Excel functions
4. Discuss how to access Microsoft's reference documentation for Excel

## What is a Function

A function can be thought of as an abstract machine that takes a bunch of things as input and outputs a number.

For instance, $f(x) = x^2$ is a function. The input of the "machine" is $x$ and the output is $x^2$ (i.e., whatever the input number is, multiplied by itself).

One can go into considerably more detail regarding the formalities of the construction, but that's really all that needs to be known for our purposes.

### How Functions are Used in Excel

How does this relate to Microsoft Excel? Well, it's the lifeblood of Excel! You see, the program contains data stored in labeled **cells**, and we can use those data as the input to Excel's many functions in order to get insight into the nature of the data.

### A Very Basic Example

![](\images\sum.gif)

The video above demonstrates the usage of the *SUM* function. The values of cells <i><font color = "CornflowerBlue">B2</font></i> and <i><font color = "red">B3</font></i> were used as input (we call these inputs to a function **arguments**) to generate the value of cell *B4*. This is done by selecting the output cell (in this case, *B4*) and setting its value to `=SUM(B2,B3)`. The equal sign tells Excel that the selected cell is not just raw data--it depends on the values of other cells according to some function.

Mathematically, *SUM* $(x_1, x_2, \dots, x_n) = \sum_{i=1}^n x_i$. Hence, *B4* = *SUM*(<font color = "CornflowerBlue"><i>B2</i></font>,<font color = "red"><i>B3</i></font>) = 3 + 7 = *10*.

## Some Useful Functions:

Now that we have an understanding of a basic function like *SUM*, let's consider some other popular functions, but ones with a little more depth.

### COUNTIFS

The *COUNTIFS* function allows us to count the number of cells within multiple ranges that satisfy all of the ranges' respective criteria.

That sounds really confusing (probably moreso due to my definition's lack of perspicuity than due to the function's complexity), but below shows it's really rather intuitive:

![](\images\countifs.gif)

I think the video makes it clear to see that, within the data on the left, there is one cell whose *Type* is *A* and whose *Val* $\ge$ *10*. Let's explore what was typed into *G3*:

<code>G3 = COUNTIFS(<font color = "CornflowerBlue">$B$3:$B$7</font>,<font color = "red">E3</font>,<font color = "plum">$C$3:$C$7</font>,<font color = "LimeGreen">F3</font>)</code>

*COUNTIFS* has four **arguments** in this case (note that there can be more arguments, but it will always be an even number, because each range has its corresponding criterion): a range, its criterion, another range, and its criterion.

What's a **range**? It's just a collection of adjacent cells (e.g., <font color = "CornflowerBlue"><i>B3:B7</i></font> is a range of cells from <font color = "CornflowerBlue"><i>B3</i></font> to <font color = "CornflowerBlue"><i>B7</i></font>).

What's with the all the dollar signs? The dollar signs signify an **absolute reference**, meaning the range is locked in place when we copy the formula to other cells.

*Pro-Tip: you can auto-populate the dollar signs for absolute reference by typing the F4 key immediately after selecting the range.*

And the **criterion**? It's just a condition that can be either *TRUE* or *FALSE*. The first criterion (second argument) just says we're looking for cells whose *Type* is *A*.

The pattern repeats itself for the third and fourth arguments such that the formula specifies we're looking for cells whose *Val* $\ge$ *10*.

Putting everything together, we can see that there is only one row that satisfies both of these criteria; hence, the value of cell *G3* is *1*.

The coolest part about functions in Excel is that once we've typed in our reference cells as arguments, we can just click on the bottom right-hand corner of the cell and drag the formula to other adjacent cells.

That's what we did in the video: by dragging the formula down, the criteria arguments change, but the ranges stay locked in place, hence the formula for cell *G4* becomes:

<code>G4 = COUNTIFS(<font color = "CornflowerBlue">$B$3:$B$7</font>,<font color = "red">E<b>4</b></font>,<font color = "plum">$C$3:$C$7</font>,<font color = "LimeGreen">F<b>4</b></font>)</code>

Notice the very slight change? The formula is almost exactly the same, but the calculation is entirely different: In this case, we're considering the number of *Type B* cells whose *Val* < *25*. That happens to be *2*, in this case, as we see in the video.

I'm sure you can think of a number of ways the function can be used to analyze data. The possibilities are really endless.

### SUMIFS

Let's move on to a similar function called *SUMIFS*. Take a look at the video below and see if you can determine what's going on:

![](\images\sumifs.gif)

Hopefully, it's fairly obvious what this function is doing: it's summing a range of cells, but only if several criteria are satisfied. Let's take a look under the hood of our calculation for cell *H3*:

<code>H3 = SUMIFS(<font color = "CornflowerBlue">$D$3:$D$7</font>,<font color = "red">$B3:$B7</font>,<font color = "plum">F3</font>,<font color = "LimeGreen">$C$3:$C$7</font>,<font color = "HotPink">G3</font>)</code>

So, our arguments are: a range of cells over which we will be summing, the first criterion's range, the first criterion, the second criterion's range, the second criterion.

Observing our function definition, the value of *H3* is just the sum of all cells within the range <font color = "CornflowerBlue"><i>D3:D7</i></font> that have values of *A* and *X* (our two criteria) immediately to the left. Unfortunately, there are no rows that satisfy both of these criteria, so our function outputs *0* for *H3*'s value.

Not too bad, huh?

And since we used absolute references for our ranges, we can just copy our formula down to the remaining rows in the list on the right. You should take a moment to compare the calculated values to what you would expect them to be (e.g., notice that there are two rows with criteria *C* and *Y*, so we must sum over both of those rows).

### VLOOKUP

The last function we'll look at in this post is called *VLOOKUP*. It's considerably different from the other functions we've discussed so far, but its utility is just as significant.

*VLOOKUP* finds a corresponding **value** for a given **key** in a **table** of values, located in a specified **column number**.

Our arguments are: a key (i.e., a cell) which focuses our search to a specific row in the data, a table of values (i.e., a range), a column number that tells us where where to look for the value in the table, and a boolean value (i.e., *TRUE* or *FALSE*) representing the type of match (approximate or exact, respectively).

Let's take a look at our function machine in action:

![](\images\vlookup.gif)

Can you see what's going on?

Let's consider the first calculation (i.e., the column whose header is *VAL2 (EXACT)*). Our formula for the value of *H3* is:

<code>H3 = VLOOKUP(<font color = "CornflowerBlue">G3</font>,<font color = "red">$B$3:$E$7</font>,3,FALSE)</code>

The formula is basically saying the following:

1. Look at the cells within the range <font color = "red"><i>B3:E7</i></font> and focus your attention on the row with the value *"A"* (since <font color = "CornflowerBlue"><i>G3</i></font> = *"A"*) in the first column of the range (i.e., *Column B*). This row happens to be *Row 3*.

2. Next, go to column number *3* in the table (i.e., *Column D*, since we start in *Column B*) and find the value of the cell.

Since the intersection of *Row 3* and *Column D* is the cell *D3*, our function's value is its value: *2*.

Okay, that seems to make sense, and the function's value of *5* for *TYPE B* makes sense, but what about the function's value for *TYPE C* and *TYPE D*?

This is where things get a little messy. Since the key *"C"* is not present in the first column of the table, the function can't compute a value for *VAL2 (EXACT)*; that's why the result is *#N/A*. And notice the result for *TYPE D*? It's *8* because that's the value corresponding to the first row (*Row 5*) whose value matches the key in column number *3*. Just something to keep in mind if your data contain not exactly one row for each key value.

Let's briefly describe the second *VLOOKUP* calculation (with the last arugment = *TRUE*). Everything's the same, except if no rows contain the key, the function selects the row whose value in the first column of the data is the most similar to the key. This happens in the case of *TYPE C*: the function just considers the key to be *"B"* rather than *"C"*, so it focuses on *Row 4* in its search. Consequently, it finds a value of *5* for *VAL2 (APPROX)*. And in the case where the key appears in more than one row in the table, the function now selects the last instance (as opposed to the first instance when the last argument was *FALSE*). This results in a *VAL2 (APPROX)* of *14*.

## Other Excel Functions

Excel contains countless functions, all with their specific merit in different scenarios, but no one knows what every single Excel function does.

Luckily, having a firm understanding of how functions work, access to <a class = "post" href = "https://support.microsoft.com/en-us/">reference documentation</a>, some patience to experiment, and a bit of ingenuity goes a long way in becoming acquainted with a new function's usage.

## Coming Up

- In the next entry of this series on Excel, we'll talk about some other commonly used features like conditional formatting and pivot tables. Look out for that post in the coming weeks/months.

- The next blog post, though, will be about programming in Excel using a powerful language called VBA--it's the tool used to make the Tetris game described above, and we'll use it to do something just as cool: interact with an open Web API to automate actions within Excel.
