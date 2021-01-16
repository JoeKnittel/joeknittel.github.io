---
layout: post
title:  "Hello, World!"
date:   2021-01-13 20:30:30
author: "Joe"
tags: test
---

This is just a test post to make sure everything's set up properly.

### Locally-Stored Static Image:

<img src = "/images/gamma.png" width = "500">

### Interactive Plot:

<iframe src="https://joeknittel.github.io/plotly_test.html" height="500" width="600" title="plotly demo" frameborder = "0"></iframe>

### GIF Functionality:

<img src = "/images/abstract-gif.gif" width = "500">

### A Bit of Python Code:

{% highlight python %}

x = 10
while(x):
    print("TRUE")
    x-=1

{% endhighlight %}

### Some R Code:

{% highlight r %}
n <- floor(rnorm(10000, 500, 100))
t <- table(n)
barplot(t)
{% endhighlight %}

### Even $\LaTeX$?:

$\Gamma(\alpha) = \int_0^\infty t^{\alpha-1}e^{-t} \ dt, \quad \alpha > 0$

$\chi^2 = \sum\limits_{j=1}^k \frac{(E_j-O_j)^2}{E_j}$

### To-Do List:

| Task                                                                   | Complete? |
| ---------------------------------------------------------------------- | --------- |
| Get images to display (Jekyll issue)                                   | ✔️        |
| Get MathJax to render $\LaTeX$ (works on local server, but not online) | ✔️        |
| Enable R Markdown -> blog post conversion (configure YAML)             | ✔️        |
| Enable Jupyter notebook -> blog post conversion (nbconvert issue)      | ✔️        |
| Modify CSS to adjust base theme (something with gemfile)               | ✔️        |
| Design favicon.ico                                                     | ✔️        |
