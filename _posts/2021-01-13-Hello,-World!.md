---
layout: post
title:  "Hello, World!"
date:   2021-01-13 20:30:30
author: "Joe"
use_math: true
tags: test
---

This is just a test post to make sure everything's set up properly.

**Locally-Stored Static Image**:

<img src = "/images/soa.jpg" width = "500">

**GIF Functionality**:

![](https://media.giphy.com/media/bAplZhiLAsNnG/source.gif)

**A Bit of Python Code**:

{% highlight python %}

x = 10
while(x):
    print("TRUE")
    x-=1

{% endhighlight %}

**Some R Code**:

{% highlight r %}
n <- floor(rnorm(10000, 500, 100))
t <- table(n)
barplot(t)
{% endhighlight %}

**Even $\LaTeX$?**:

$\Gamma(\alpha) = \int_0^\infty t^{\alpha-1}e^{-t} \ dt, \quad \alpha > 0$

$$\chi^2 = \sum_{j=1}^k \frac{(E_j-O_j)^2}{E_j}$$

| To-do List                                                          | Complete? |
| ------------------------------------------------------------------- | --------- |
| Get images to display (Jekyll issue)                                | ✔️        |
| Configure YAML for direct .rmd -> blog post conversion              | ✔️        |
| Get MathJax to render LaTeX (works on local server, but not online) | ✔️        |
| Modify CSS to adjust base theme                                     | ✔️        |
| Probably a bunch of other things that I'll notice later             | ❔        |
