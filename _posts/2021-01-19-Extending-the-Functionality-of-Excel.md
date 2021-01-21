---
title: "Extending the Functionality of Excel : Working with VBA and Web APIs"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-01-19 05:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](/images/vba api.png)

Last time, we considered some very contrived examples wherein we applied a few of Excel's built-in functions to data contained in a few cells. It was a good way to get a taste of how Excel is used, but it certainly doesn't tell the whole story.

In this post, we look to extend our reach within Micorosft Excel by doing the following:

1. Describing the various types of data sources and how to import them into Excel
2. Using VBA, the programming language of Excel, to define our own functions and subroutines
3. Connecting to and interacting with a website's database programmatically through an interface called a Web API

## Data are Everywhere

When performing real-world analyses in Excel, we often won't be typing our functions' input data directly into cells. Data can also be found in:

- Other Excel worksheets within the same workbook

- External sources:
  - Data files
  - Webpages
  - Databases
  - etc.

### Using Data in Other Worksheets

We didn't talk about it last time, but the grid of cells in which we tested our functions is called a `worksheet`. When we open up a new MS Excel file, it contains just one worksheet, but we can add more very easily (see below):

![](\images\sheet2.gif)

The file that houses this collection of worksheets is called a `workbook`, and it allows us to keep our data organized. We can even reference a range of cells (or just one cell) in other worksheets as our function arguments by typing $\text{[worksheet-name]![range]}$ instead of just $\text{[range]}$ (see example below):

![](\images\sheet1ref.gif)

As you can see, we were able to use the `SUM` function on the worksheet named $\text{Sheet2}$ with arguments coming from cells $\text{B25:B26}$ on the worksheet named $\text{Sheet1}$ using by typing $\text{"=SUM(Sheet1!B25:B26)"}$.

### Accessing Data from External Sources

#### Using the Query Tool

If your desired data reside in a readily-accessible external source, Excel makes the process of setting up a connection and importing the data seamless by clicking: $\text{Data} \rightarrow \text{New Query}$:

![](\images\external_data.gif)

Using this approach, we can import data from:

- Excel files (.xls, .xlsx, .xlsm)
- CSV (comma-separated value) files
- Individual webpages
- Databases

The process of importing data from other Excel files is simple and from CSV files and webpages is pretty easy, so we're going to skip over those.

Connecting to and interacting directly with databases in Excel is more challenging, but important, so we're going to save that for its own future post.

#### Using a Web API

In some cases, the data we want to interact with are not so easily accessible.

**Example: A Tool to Fetch a Word's Definition**

For instance, say we want a cell in our worksheet to display the cell to its immediate left's most common definition.

![](\images\definition.png)

We can set up a connection with a webpage that has the word's definition (e.g., <a href = "https://www.merriam-webster.com/dictionary/test">https://www.merriam-webster.com/dictionary/test</a> has information for the word "test") and then do a bit of <a href = "https://en.wikipedia.org/wiki/Data_wrangling">data wrangling</a> to display the definition. But that's just one page; we need this to work for all possible words! Seems like we're out of options.

Well, not so fast... Let's consider the problem from a different perspective:

- The definitions of all words (or, at least, the vast majority of them) can be found within different pages hosted on Merriam Webster's website
- All of those data are contained in a database somewhere
- If we can access that database, we can solve our problem

The question that remains is: "Can we access the site's database?" It turns out the answer is yes, if the site provides us with a `Web API` (see diagram below):

<img src = "\images\web-api.png" width = "100%">

In the diagram, we see that the key link between the site's database and us is the API. The process goes as follows:

1. We send an API request to the web server via the internet
2. The web browser directs the request to the database in the form of a query
3. The results of the query are collected into a nice little package and the web server directs it back to us
4. We get our requested data in the form of an API response (most commonly with <a href = "https://en.wikipedia.org/wiki/JSON">JSON</a> structure)     

Since <a href = "https://dictionaryapi.com/products/api-collegiate-dictionary">Merriam-Webster does provide a Web API</a>, we'll be able to proceed, and we'll do so using a programming language called `VBA`.

## Interacting with a Web API Using VBA

VBA (Visual Basic for Applications) is a programming language that allows us to create our own user-defined functions (`UDFs`) and `subroutines`, in order to extend the utility of our workbooks.

This is exactly what we need to set up a connection with a Web API, get our data, and interact with the data in all kinds of fancy ways.

### Setup

To get started, we first need to enable the $\text{Developer}$ tab within Excel. If yours is not already enabled, <a href = "https://support.microsoft.com/en-us/office/show-the-developer-tab-e1192344-5e56-4d45-931b-e5fd9bea2d45#:~:text=The%20Developer%20tab%20isn't,select%20the%20Developer%20check%20box.">this guide</a> should help you out.

Now, that that's set up, let's go to the Visual Basic editor:

![](\images\vba.gif)

We're ready to write some code! Let's continue with our Merriam-Webster example from above. Where we left off, we found out that we would be able to access Merriam-Webster's database via their Web API, but how do we go about doing so?

### Sending an API Request

First, we need a function to send the API request:

{% highlight vb %}

'sends an api request with the desired url, and outputs json data
Function getData(myUrl As String) As String

    Dim winHttpReq As Object
    Set winHttpReq = CreateObject("Microsoft.XMLHTTP")

    winHttpReq.Open "GET", myUrl, False
    winHttpReq.Send

    getData = winHttpReq.ResponseText

End Function

{% endhighlight %}

Don't worry too much about the details, but this function's argument is a URL. So what URL do we use?

The <a href = "https://dictionaryapi.com/products/api-collegiate-dictionary" width = "400px">API</a> helps us out:

![](\images\request_url.png)

It turns out that in order to use most Web APIs, a key is required, so that that the server is not bogged down with requests. In our case, the <a href = "https://dictionaryapi.com/register/index">API registration process</a> is quick and free, but that's not always the case.

So, we've got our API key, now we can construct our full URL:

{% highlight vb %}

'gets the url of a searched word
Function getURL(word As String) As String

    Dim p1, p2, p3, apiKey, url As String

    p1 = "https://www.dictionaryapi.com/api/v3/references/collegiate/json/"
    p2 = word
    p3 = "?key="

    apiKey = "your-api-key"

    url = p1 & p2 & p3 & apiKey

    getURL = url

End Function

{% endhighlight %}

The function above just concatenates a few strings together (using &) to create our URL used in the API request function.

At this point, we can programmatically get definitions, examples, etymologies, synonyms, pronunciations, parts of speech, etc. about any word using our functions like so:

{% highlight vb %}

getData(getURL(word))

{% endhighlight %}

The function above, by way of composition of functions, takes a word (i.e., a string) as input, and output a $\text{JSON}$ object with our data. Pretty cool!

The only problem is, this is what that $\text{JSON}$ object looks like:

![](\images\json.png)

Don't get too scared--it's just a really long, structured string. We'll need some other tools in order to efficiently break down the $\text{JSON}$ object into its parts and access our desired data (i.e., the word's most common definition). This process is called `parsing`.

### Parsing JSON

Take a look at this function below that we'll use to get the most common definition of a given word using our API:

{% highlight vb %}

'parses json data from api response and fetches the desired word's definition
Function getDef(word As String) As String

    Dim data As String
    data = getData(getURL(word)) 'this is the resultant JSON object from the API call
    Dim Json As Object
    Set Json = JsonConverter.ParseJson(data) 'this is the JSON object, parsed

    If Left(JsonConverter.ConvertToJson(Json(1)("def")(1)("sseq")(1)(1)(2), Whitespace:=2), 1) <> "[" Then

        getDef = JsonConverter.ConvertToJson(Json(1)("def")(1)("sseq")(1)(1)(2)("dt")(1)(2))

    Else

        getDef = JsonConverter.ConvertToJson(Json(1)("shortdef")(1))

    End If

End Function

{% endhighlight %}

It's not necessary to know exactly what's going on here, but here's the gist:

1. We send an API request for a specified word using `getData(getURL(word))`  
2. The resultant JSON object is stored in a variable called $\text{data}$
3. We use a module called $\text{JsonConverter}$ to parse the data into a more readable format
4. Once the data is parsed, we can access elements of it just like we would access elements of an array (i.e., [id]) or (i.e., list (id))
5. We navigate to the appropriate element in the data structure to find our word's definition

#### Using Open-Source Modules and Classes in VBA

So, what's $\text{JsonConverter}$? It's an `open-source` (located <a href = "https://github.com/VBA-tools/VBA-JSON">here</a>) VBA `module` (i.e., collection of functions) that uses the $\text{VBA-Dictionary}$ (found <a href = "https://github.com/VBA-tools/VBA-Dictionary">here</a>) `class` (i.e., custom VBA object) to turn our messy $\text{JSON}$ object into something more navigable. Other people did the tough work to create this useful code and made it freely available to the public, so let's not re-invent the wheel!

To use the aforementioned module and class, we just download them and import them in their respective section of the Visual Basic editor:

![](\images\import_modules.gif)

### Calling our getDef Function

Our `getDef` function can now take a word as input and output its most common definition. Or, at least, kind of. Here's what `getDef("weird")` outputs:

`"{bc}of strange or extraordinary character {bc}{sx|odd||}, {sx|fantastic||}"`

The definition of "weird" appears to be there, but there's a bunch of other tags that signify stylistic elements of the defintion for display on Merriam-Webster's website:

![](\images\merriam-def.png)

We need to further process the definition with another function.

### Transforming the Definition

It turns out that Merriam-Webster provide a slew of formatting tags in their definitions, so the task ends up require a good bit of work:

{% highlight vb %}

'cleans up definition string (i.e., formatting tags), preparing it for printing to screen
Function getTransformedDef(word As String) As String

    'removes " from start and end of def (and {bc} from start, if necessary)
    Dim temp As String
    temp = getDef(word)
    If Mid(temp, 2, 3) = "{bc" Then
        temp = Mid(getDef(word), 6, Len(getDef(word)) - 6)
    Else
        temp = Mid(getDef(word), 2, Len(getDef(word)) - 2)
    End If

    'removes {bc} tag, if present
    Dim posBC As Integer
    posBC = InStr(temp, "{bc}")
    If posBC > 0 Then
        temp = Left(temp, posBC - 1) & ": " & Mid(temp, posBC + 4, (Len(temp) - posBC) - 3)
    End If

    'removes {sx} tag(s), if present
    Dim posSX, endSX, wordLen As Integer
    posSX = InStr(temp, "{sx|")
    endSX = InStr(temp, "||")
    wordLen = (endSX - posSX) - 4
    While posSX > 0
        temp = Left(temp, posSX - 1) & Mid(temp, posSX + 4, wordLen) & Right(temp, Len(temp) - posSX - 6 - wordLen)
        posSX = InStr(temp, "{sx|")
        endSX = InStr(temp, "||")
        wordLen = (endSX - posSX) - 4
    Wend

    'removes {a_link} tag(s), if present
    Dim posALink, endALink, linkLen As Integer
    posALink = InStr(temp, "{a_link")
    While posALink > 0
        endALink = InStr(Mid(temp, posALink), "}") + posALink - 1
        linkLen = (endALink - posALink) - 8
        temp = Left(temp, posALink - 1) & Mid(temp, posALink + 8, linkLen) & Mid(temp, endALink + 1)
        posALink = InStr(temp, "{a_link")
    Wend

    'removes {d_link} tag(s), if present
    Dim posDLink, endDLink, dlinkLen, tagEnd As Integer
    posDLink = InStr(temp, "{d_link")
    While posDLink > 0
        endDLink = InStr(Mid(temp, posDLink + 8), "|") + posDLink + 7
        dlinkLen = (endDLink - posDLink) - 8
        tagEnd = InStr(Mid(temp, posDLink), "}") + posDLink - 1
        temp = Left(temp, posDLink - 1) & Mid(temp, posDLink + 8, dlinkLen) & Mid(temp, tagEnd + 1)
        posDLink = InStr(temp, "{d_link")
    Wend

    'removes {it} tags, if present
    Dim posIt, endIt As Integer
    posIt = InStr(temp, "{it")
    While posIt > 0
        endIt = InStr(temp, "{/it")
        temp = Left(temp, posIt - 1) & Mid(temp, posIt + 4, (endIt - posIt) - 4) & Mid(temp, endIt + 5)
        posIt = InStr(temp, "{it")
    Wend

    'removes {b} tags, if present
    Dim posB, endB As Integer
    posB = InStr(temp, "{b")
    While posB > 0
        endB = InStr(temp, "{/b")
        temp = Left(temp, posB - 1) & Mid(temp, posB + 3, (endB - posB) - 3) & Mid(temp, endB + 4)
        posB = InStr(temp, "{b")
    Wend

    'removes {dx_def} tags, if present
    Dim posDX, endDX As Integer
    posDX = InStr(temp, "{dx_def")
    endDX = InStr(temp, "{/dx_def")
    If posDX > 0 Then
        temp = Left(temp, posDX - 1) & Mid(temp, endDX + 9)
    End If

    'removes {dx} tags, if present
    Dim posD, endD As Integer
    posD = InStr(temp, "{dx")
    endD = InStr(temp, "{/dx")
    If posD > 0 Then
        temp = Left(temp, posD - 1) & Mid(temp, endD + 5)
    End If

    'replaces "\u2013" with the appropriate "–"
    Dim posDash As Integer
    posDash = InStr(temp, "\u2013")
    While posDash > 1
        temp = Left(temp, posDash - 1) & "–" & Mid(temp, posDash + 6)
        posDash = InStr(temp, "\u2013")
    Wend

    getTransformedDef = temp

End Function

{% endhighlight %}

### Defining a Subroutine for Printing the Definition

Now that the formatting is complete, let's define a subroutine that will generate the specified word's definition in the cell to the next cell over whenever some action occurs:

{% highlight vb %}

'when a word is input into the target cell, this sub is called; the word's definition is printed in the cell one place to its right
Sub printDef(row As Integer, col As Integer)

    Cells(row, col).Value = getTransformedDef(Cells(row, col - 1).Value)

End Sub

{% endhighlight %}

### Completing the Objective

The following subroutine completes our objective, as it prints the most common definition in the cell immediately to the right of a target cell (if the target cell happens to be in $\text{Column B}$):

{% highlight vb %}

'prints word's definition in cell to the right whenever a cell in Column B is modified
Private Sub Worksheet_Change(ByVal Target As Range)

    Dim temp As String

    If Target.Column = "2" And Target.Value <> "" Then

      Call printDef(Target.row, 3)

    End If

End Sub

{% endhighlight %}

## Going Further

I'm sure you can imagine, the example above can be augmented in a number of ways using even more of the data provided in our API response.

In fact, I've done just that as a way to improve my vocabulary (see below):

<img src = "https://raw.githubusercontent.com/JosephKnittel/VBA/main/Images/vocab_demo.gif">

The app, if you will, generates the most common definition (what we did above), the word's part of speech, and a link to an audio file with the word's pronunciation, as well as some conditional formatting and a delete feature, which clears a given row.

All of the code I developed can be found in <a href = "https://github.com/JoeKnittel/VBA">this repository</a>, and I look forward to continuing this project.

## Coming up

We've talk a lot about Excel in the most recent posts. Let's flip the script and start to discuss coding in Python.

More on what that entails next time.
