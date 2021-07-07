---
title: "AI in Actuarial Science, Part I: Overview and Common Approaches"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Joe"
date: '2021-07-07 00:01:01'
excerpt: ".rmd to .md, Jekyll-style"
layout: post
---

![](/images/ai.jpg)

You've heard it for years: <a href = "https://en.wikipedia.org/wiki/AI_takeover" target = "_blank">*"AI's taking over the world! Soon, all of your jobs will become obsolete!"*</a>

To some, such hyperbolic assertions engender in them deep fear of a dystopian future; to others, complete disregard, as only a brainwashed drone could have uttered such an absurd statement.

Ever the agnostic, I've sought to better understand the nature of AI—specifically, in the context of actuarial science—before coming to any conclusions.

We'll do the following in this post:

1. Describe the field of artificial intelligence and how it has evolved over the years
2. Get an overview of the types of statistical models used to make predictions
3. Consider the actuarial modeling toolkit, including traditional and more recent approaches
4. Evaluate the modern actuarial toolkit’s performance vs. state-of-the-art AI-based methods

## What is AI?

AI is intelligence demonstrated by non-biological entities (i.e., machines, rather than humans or animals).

### A Bit of History

From <a href = "https://en.wikipedia.org/wiki/Talos" target = "_blank">mythological automata</a> to <a href = "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/Frankenstein%27s_monster_%28Boris_Karloff%29.jpg/170px-Frankenstein%27s_monster_%28Boris_Karloff%29.jpg" target = "_blank">man-made monsters</a>, the concept of AI has been around since antiquity. It wasn’t, however, until Alan Turing's invention of the <a href = "https://en.wikipedia.org/wiki/Turing_machine" target = "_blank">"a-machine"</a> in 1936 that actualizing the notion became plausible.

Scientists began at once exploring ways in which this proto-computer could be used to bring about machine intelligence: <a href = "https://en.wikipedia.org/wiki/Artificial_neuron" target = "_blank">"artificial neurons"</a> mimicking—albeit at a very basic level—the computational infrastructure of the human brain were devised within years and real-world applications like <a href = "https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5392560" target = "_blank">checkers-playing agents</a> followed soon after.

Leading researchers understood the potential implications of AI applications and organized a <a href = "https://en.wikipedia.org/wiki/Dartmouth_workshop" target = "_blank">now-historic eight-week workshop</a> at Dartmouth University in the summer of 1956, which is recognized by many as the origination of the field as a formal research domain. In the years that followed, machines made short work of their human competitors in checkers, and went on to prove mathematical theorems and begin generating somewhat intelligible sentences.

The quick progress led many proponents to conclude that the world would soon be vastly transformed by the burgeoning field of AI. Nobel laureate and attendee of the Dartmouth summit, Herbert Simon, famously stated in 1965 that, "Machines will be capable, within twenty years, of doing any work a man can do."

Of course, we know that didn't happen, but it's worth considering briefly some significant advancements that have occurred since he made the bold prediction:

|                                    AI Advancement                                     | Year  |
|:-------------------------------------------------------------------------------------:|:-----:|
|                      First multi-layer neural network developed                       | 1965  |
| "Expert systems" use specialists' knowledge and rule-based models to help non-experts | 1980s |
|               Deep Blue defeats Garry Kasparov as World Chess Champion                | 1997  |
|               Roomba, the first autonomous vacuum cleaner, is released                | 2002  |
|        Watson defeats Ken Jennings and Brad Rutter as World Jeopardy Champion         | 2011  |
|    AI personal assistants (e.g., Siri, Alexa) begin responding to verbal requests     | 2011  |
|                    Machines outperform humans at image recognition                    | 2015  |
|                    AlphaGo defeats Lee Sedol as World Go Champion                     | 2016  |
|         Boston Dynamics develops autonomous animal- and human-inspired robots         | 2016  |
|                     Self-driving vehicles begin navigating roads                      | 2018  |
|         Alibaba language model surpasses humans in reading comprehension task         | 2018  |
|                  StyleGANs generate photorealistic, non-human faces                   | 2019  |
|               GPT-3 produces a broad range of uncannily human-like text               | 2020  |
|      AlphaFold 2 solves the "protein folding problem," aiding in drug discovery       | 2020  |

Wow, a lot has occurred in AI over the years! There were a few “AI winters” mixed in there, but the field has undeniably undergone a real renaissance recently.

### Relevance

As demonstrated by the above list of advancements, AI concerns itself with all subsets of machine intelligence: perception, knowledge representation, learning, reasoning, action, language, etc.

<a href = "https://github.com/researchmm/img2poem" target = "_blank">Computers writing poetry</a> and <a href = "https://thispersondoesnotexist.com/" target = "_blank">generating images of fake humans</a> is cool and all, but such tools don't pertain to the field of actuarial science.

Let's focus our attention on something underlying almost all of the work actuaries do: predictive analytics.

## Predictive Analytics

Predictive analytics is the practice of leveraging statistical techniques and historical data to elucidate information about the nature of the data and make predictions about future states of the world.

This generally entails training a model based on a set of predictor (input) variables and using it to assign a value to a target (output) variable.

If the target variable is categorical (e.g., “Small” “Medium” “Large”), we call this a classification model. Conversely, if the target is numerical, it’s considered a regression model.

<img src = "/images/class-reg.png">

### Breiman’s Two Cultures

Statistical models, though they aim to accomplish the similar objectives, often vary significantly in structure.

In 2001, acclaimed statistician, Leo Breiman, <a href = "http://www2.math.uu.se/~thulin/mm/breiman.pdf" target = "_blank">famously made the case</a> that two distinctly different groups of models had emerged: data models and algorithmic models.  

#### Data Models

Guided by expert knowledge and intuition about the nature of the phenomenon being investigated, models take a parametric form: $y_i = f\left(x_{1i},x_{2i},…,x_{ni}\right)$

Breiman estimated that, at the time, 98% of statisticians employed such models.

*NB: These models are not considered instances of AI, as computers are not used to ascertain the underlying structure of the model.*

#### Algorithmic Models

These models have no pre-defined, human-biased form—their structures are determined based on algorithms which aim to maximize predictive performance.

We're now stepping into the world of AI, into a subfield called machine learning (ML).

<img src = "/images/ai-ml-dl.png">

In addition to making predictions about target variables (i.e., classification and regression), some algorithmic techniques, in a process called *unsupervised learning*, can be used to cluster observations into groups without having access to target variable values on the training set:

<img src = "/images/ml.png">

### Types of Predictive Models

#### Data Models

<hr>

<p class="" style="margin: 14px;"></p>

<b>Linear Model</b> (<a href = "https://galton.org/essays/1880-1889/galton-1886-jaigi-regression-stature.pdf" target = "_blank">1886</a>)

For this type of model, we can approximate the value of a target variable for a given observation as the sum of a linear combination of predictor variables, a bias term ($\beta_0$) representing the value of the target in the absence of all predictors, and a normally-distributed noise term ($\epsilon_i$) representing random fluctuations due to some unknown influences:    

$y_i = \beta_0 + \sum\limits_{j=1}^{p} \beta_j x_{ij} + \epsilon_i$

This can be expressed in vector form as:

$\bf{y} = \bf{X} \bf{\beta} + \bf{\epsilon}$

*Example*:

<img src = "/images/lm.png">

<hr>

<p class="" style="margin: 14px;"></p>

<b>Logistic Model</b> (<a href = "https://www.nuffield.ox.ac.uk/users/cox/cox48.pdf" target = "_blank">1958</a>)

This model is frequently used for binary classification tasks. The linear combination of predictor variables, instead of being used directly, is now mapped to a probability by way of a logit link function:

$\log\left(\frac{p}{1-p}\right) = \beta_0 + \sum\limits_{j=1}^{p} \beta_j x_{ij} + \epsilon_i$

The probability $p$ can then be used to classify the target as $\text{TRUE}$ or $\text{FALSE}$ based on whether or not it is above the threshold value (e.g., $p = 0.8 \rightarrow \text{TRUE}$).

*Example*:

<img src = "/images/logistic.png">

<hr>

<p class="" style="margin: 14px;"></p>

<b>Generalized Linear Models (GLMs)</b> (<a href = "https://docs.ufpr.br/~taconeli/CE225/Artigo.pdf" target = "_blank">1972</a>)

In the 1970s, linear models were generalized further to handle a wide variety of target distributions (i.e., not just the Gaussian distribution, as was the assumption with the vanilla linear model) and transformations via link functions (as seen in the logistic model).

For instance, a discrete, non-negative target variable whose sample mean is similar to its variance could now be modeled more appropriately by using a GLM with a Poisson family and a logarithmic link function:

$\text{ln}(\lambda_i) = \beta_0 + \sum\limits_{j=1}^{p} \beta_j x_{ij}$

$y_i \sim \text{Poisson}(\lambda_i)$

<p class="" style="margin: 14px;"></p>

*Example*:

<img src = "/images/glm.png" width = 450>

<hr>

<p class="" style="margin: 14px;"></p>

<b>Regularized Regression Models</b> (L1: <a href = "https://statweb.stanford.edu/~tibs/lasso/lasso.pdf" target = "_blank">1996</a>, L2: <a href = "https://www.math.arizona.edu/~hzhang/math574m/Read/RidgeRegressionBiasedEstimationForNonorthogonalProblems.pdf" target = "_blank">1970</a>, EN: <a href = "https://web.stanford.edu/~hastie/Papers/B67.2%20(2005)%20301-320%20Zou%20&%20Hastie.pdf" target = "_blank">2005</a>)

Regression models, whether they be simple linear regression of more complicated GLMs, are generally fit by finding the set of parameters (i.e., $\beta$'s) that minimize the sum of squared errors (SSE) between predictions and actual values of the target.

Mathematically, the *standard regression* parameter optimization problem can be described as follows:

$\displaystyle \min_{\beta \in \mathbb{R}^p} \left\\{ \sum\limits_{i=1}^{n} \left( y_i - \sum\limits_{j=1}^{p} \beta_{j} x_{ij}  \right)^2 \right\\} $

This approach often yields favorable results, but there are cases in which overfitting occurs, and the model’s predictions will fail to generalize to unseen data.

In an attempt to reduce overfitting and, in some cases, to aid in feature selection, methods were devised to adjust the standard SSE cost function by adding a penalty term; this process is called regularization.

*Lasso (L1) regularized regression* parameter optimization problem:

$\displaystyle \min_{\beta \in \mathbb{R}^p} \left\\{ \sum\limits_{i=1}^{n} \left( y_i - \sum\limits_{j=1}^{p} \beta_{j} x_{ij}  \right)^2 + \lambda \sum\limits_{j=1}^{p} \left \\lvert \beta_j \right \\rvert \right\\} $

*Ridge (L2) regularized regression* parameter optimization problem:

$\displaystyle \min_{\beta \in \mathbb{R}^p} \left\\{ \sum\limits_{i=1}^{n} \left( y_i - \sum\limits_{j=1}^{p} \beta_{j} x_{ij}  \right)^2 + \lambda \sum\limits_{j=1}^{p} \beta_j^2 \right\\} $

*Elastic net regularized regression* parameter optimization problem:

$\displaystyle \min_{\beta \in \mathbb{R}^p} \left\\{ \sum\limits_{i=1}^{n} \left( y_i - \sum\limits_{j=1}^{p} \beta_{j} x_{ij}  \right)^2 + \lambda \left(   \alpha \sum\limits_{j=1}^{p} \left \\lvert \beta_j \right \\rvert + (1-\alpha) \sum\limits_{j=1}^{p} \beta_j^2 \right) \right\\}, \quad \alpha \in [0,1] $

<p class="" style="margin: 14px;"></p>

*Example*:

<img src = "/images/regularization.png" width = 450>

<hr>

<p class="" style="margin: 14px;"></p>

<b>Survival Models</b> (<a href = "https://rss.onlinelibrary.wiley.com/doi/pdfdirect/10.1111/j.2517-6161.1972.tb00899.x" target = "_blank">1972</a>)

Often, we seek to predict the duration of time until a given event will occur. Such models are used in a broad set of fields from economics to sociology, but they’re especially important in actuarial science.

A particularly famous example is the Cox model, which models the multiplicative influence of a set of covariate predictors $\textbf{X}_i$ on the instantaneous death rate $\lambda(t \\lvert \textbf{X}_i)$ of a subject $i$ at time $t$:

$\lambda(t \\lvert \textbf{X}_i) = \lambda_0(t) \cdot \text{exp}\left( \textbf{X}_i \cdot \beta \right)$, where $\lambda_0(t)$ is the baseline hazard function

<p class="" style="margin: 14px;"></p>

*Example*:

<img src = "/images/survival.png" width = 450>

<hr>

<p class="" style="margin: 14px;"></p>

#### Algorithmic Models

<b>Classification and Regression Tree (CART) Model</b> (<a href = "https://www.taylorfrancis.com/books/mono/10.1201/9781315139470/classification-regression-trees-leo-breiman-jerome-friedman-richard-olshen-charles-stone" target = "_blank">1984</a>)

The CART model was one of the first algorithmic approaches to predictive analytics.

The algorithm splits the predictor space into an optimal set of non-overlapping regions based on a chosen iterative splitting criterion (e.g., minimize entropy) and the target is assigned the mean (regression) or most common class (classification) associated with its corresponding region in the predictor space.

*Example*:

<img src = "/images/tree.png" width = 550>

<hr>

<p class="" style="margin: 14px;"></p>

<b>Random Forest Model</b> (<a href = "https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf" target = "_blank">2001</a>)

This model is what is considered an ensemble model: it’s comprised of several component models. The idea is that by building numerous decision trees on different subsets of the training data, their predictions can be averaged to yield a more stable result.

Obviously, fitting multiple models is more computational expensive and introduces a sometimes problematic level of obscurity to the nature of the prediction (i.e., low interpretability); however, by reducing overfitting via limiting the extent to which the trees resemble one another, this model often yields very accurate predictions.

*Example*:

<img src = "/images/random-forest.png" width = 550>

<p class="" style="margin: 1px;"></p>

<hr>

<p class="" style="margin: 14px;"></p>

<b>Boosted Trees Model</b> (<a href = "https://statweb.stanford.edu/~jhf/ftp/trebst.pdf" target = "_blank">1999</a>)

Another ensemble model is called the boosted trees model. Rather than building a number of trees in parallel, then averaging to get a final prediction, this approach builds one tree, identifies its weaknesses, then iteratively builds new trees to better model the predictor space not well modeled by the preceding trees. This process limits the model’s bias, but can sometimes lead to overfitting to the training data.

*Example*:

<img src = "/images/boosting.png" width = 550>

<hr>

<p class="" style="margin: 14px;"></p>

<b>Neural Network (Deep Learning) Models</b> ($\alpha$: <a href = "http://www.cse.chalmers.se/~coquand/AUTOMATA/mcp.pdf" target = "_blank">1943</a>, DL: <a href = "https://www.iro.umontreal.ca/~lisa/pointeurs/TR1312.pdf" target = "_blank">2009</a>)

We briefly touched on the idea that was introduced in the early years of AI, the concept of “artificial neurons.” Researchers thought that if they could construct a model whose structure mirrors that of the human brain, it could be as good at—if not better than—people at making predictions.

The model, at its core, is rather simple (it’s just a bunch of interwoven GLMs bound to one another via non-linear activation functions), but the sheer architectural complexity (the figure below contains three layers, whereas fifty layers wouldn’t be uncommon in a real-world deep learning model) often leads to computational limitations.  

*Example*:

<img src = "/images/neural-net.jpg" width = 550>

<p class="" style="margin: 30px;"></p>

<hr>

<p class="" style="margin: 14px;"></p>

## The Actuarial Toolkit  

### Traditional Approaches

Actuarial science is not a new field—it <a href = "https://en.wikipedia.org/wiki/Actuary#History" target = "_blank">traces its roots</a> all the way back to the 1600s. It’s no surprise, then, that many in the field continue to adhere steadfastly to the foundational principles that have historically made actuaries esteemed prognosticators in the eyes of the general public.

Data models (GLMs, survival models, regularized regression, etc.) present actuaries with powerful ways of exploiting their expert knowledge of statistical methods to make predictions. Such models, by way of their formulaic structure, also maintain a high level of interpretability, making prescribing actionable strategies a breeze.

### Recent Modifications

For decades, actuaries have been able to turn the blind eye to advances in AI, <a href = "https://theactuarymagazine.org/the-ai-revolution-is-coming/" target = "_blank">but it’s becoming no longer a feasible strategy</a>. In the past few years, actuaries have begun to re-consider machine learning models: *maybe that decision tree is better than the GLM I had intended to use*; or, *perhaps the exceptional predictive performance of that random forest is too good to disregard*.

With that said, neural network-based deep learning models (which we’ll talk more about in Part II of this series), responsible for many of the most important recent AI advancements, remain a completely foreign concept to the vast majority of actuarial scientists.

## Evaluating the Modern Actuarial Toolkit

For the reasons listed above, many have argued that the actuarial field has failed to keep up with AI’s pace of advancement, and that the modern actuarial toolkit pales in comparison to the state-of-the-art in predictive analytics.

I wanted to test this hypothesis, so I put the SOA curriculum-based tools to the test versus the world of data science practitioners in a <a href = "https://www.kaggle.com/c/titanic" target = "_blank">Kaggle competition</a>.

### Experiment: Predicting Titanic Passenger Survival

Kaggle is a platform that allows teams to compete against one another (often with prizes at stake) in their ability to make predictions. The nature of the competitions and the set of competitors vary widely, with submissions coming from amateur investigators to industry experts with access to vast computational resources and the latest tools of the trade.

The competition I chose has already had over 50,000 submissions from all over the globe. It’s a classification task (objective: predict whether a passenger on the Titanic would survive, based on a number of passenger-specific predictors) that uses tabular data. Though it’s not an actuarial dataset, I figured the nature of the problem was sufficiently similar in order to get some insight into how well our models work, in practice.

All <a href = "https://github.com/JoeKnittel/Kaggle-Competitions/tree/main/Titanic" target = "_blank">data, code, and plots for the analysis</a> can be found on my GitHub, and the report summarizing my findings is shown below:

<p class="" style="margin: 30px;"></p>

<iframe src="/experiments/final-analysis.html" width = "100%" height = 500 frameborder = 0> </iframe> <br>

### Results

GLMs, elastic net regression, classification trees, random forests, and boosted tree models were all put to the test.

Techniques to optimize performance included: log-transformed predictors, feature generation, feature selection, KNN-imputation, up/down sampling, hyperparameter tuning using repeated cross-validation.

Not surprisingly, the ensemble tree-based methods yielded the best results, with random forests and eXtreme boosted trees correctly classifying around 80% of the passengers on the test set.

That's not bad! Only 4.6% of the 50,000+ teams who competed were able to achieve this result. I guess <a href = "https://www.replacedbyrobot.info/69657/actuaries" target = "_blank">our jobs are safe for a while</a>.

One might wonder whether we could do better, though. After all, we’re supposed to be leaders in the statistical approaches to prediction-making, right?

## Coming Up

The tools actuaries have used for decades and, to an even greater extent, the machine learning-based techniques that they’ve begun exploring in recent years, perform rather well in prediction tasks.

But what did the upper echelon teams in our experiment do differently to obtain state-of-the-art results? I have a sneaking suspicion that they've utilized the subfield of machine learning that we brushed over earlier: deep learning.

In the second part of this series, we'll look to take our actuarial toolkit to the next level, by introducing practical ways of employing complex, neural network-based deep learning models.
