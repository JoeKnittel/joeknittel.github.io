---
layout: post
title:  "Hello, World!"
date:   2021-01-13 20:30:30
author: "Joe"
use_math: true
---

This is just a test post to make sure everything's set up properly.

**Locally-Stored Static Image**:

![](/images/soa.jpg)

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

$$x^2=1$$

**To-do List**:

- Get images to display (Jekyll issue) [fixed!] ✔️
- Configure YAML for direct .rmd -> blog post conversion [fixed!] ✔️
- Get MathJax to render LaTeX (works locally, but not online -- so annoying)
  [fixed! had to convert gemfile theme to regular theme] ✔️
- Probably a bunch of other things that I'll notice later ❔
