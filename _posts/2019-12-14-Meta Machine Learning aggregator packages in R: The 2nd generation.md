---
layout: post
title: Meta Machine Learning aggregator packages in R, The 2nd generation
tags: [rstats, r]
---








## TL;DR
mlr was refactored into mlr3. caret was refactored into tidymodels. What are the main differences in terms of software design, and tweaking it for your own needs. R6 vs S3. Which one is less fraigle?


## Motivation
My [previous post](https://towardsdatascience.com/meta-machine-learning-packages-in-r-c3e869b53ed6) from mid 2018 described my learning experience with R packages for ‘meta’ machine learning aggregator packages: mlr, caret and SuperLerner. These packages unify a machine learning framework over multiple independent individual multivariate models/packages, and provide a ‘meta’ machine learning framework around them for common tasks such as resampling, tuning, benchmarking, ensemble and others.

Since then, a couple of major development has evolved and matured in the R ecosystem, replacing deprecated packages that, ahem, ahem, not comfortable to admit, have started to suffer from … [scope creep](https://medium.com/free-code-camp/scope-creep-and-other-software-design-lessons-learned-the-hard-way-edacf021965b).

The current refactoring packages I will discuss here are:

[mlr](https://mlr.mlr-org.com/) -> mlr3 ([page](https://mlr3.mlr-org.com/), [manual](https://mlr3book.mlr-org.com/)  
[caret](http://topepo.github.io/caret/index.html) -> tidyModels ([github repo](https://github.com/tidymodels/tidymodels))  

This post will attempt to describe the main differences between the next generation of these two packages, with an emphasis on the software design and object architecture / structure that each package utilize, and how it affects the user/practitioner/developer experience, not only for basic use, but also when taking it to the next level of customizing and modifying it for a more specific use.




## Scope creep… are you telling me your ‘yesterday’s state-of-the-art package is actually crap?
<img src="https://drorberel.github.io/img/blog/scope.png">  
Image credit: https://leankit.com/wp-content/uploads/2013/11/Screen-Shot-2013-11-25-at-4.25.52-PM.png

When I was first exposed to mlr, I thought to myself, WOW, what a huge effort was invested into this package, not even mentioning the extensive documentation, it will probably last forever. In retrospective I found it was definitely worth my time to learn it, in spite of the non-trivial ‘dialect’ it had, which was different from the good-old-friend ‘caret’ package I had already been accustomed to.

Then, one shiny day, I was surprised to discover that the developers of mlr are actually no longer satisfied with their current version of mlr, and are already working on developing a new package, mlr3, which mostly consists of R6, an advanced system for object oriented class that is way beyond my comfort zone.

My initial concerns were: Is this the end of my affair with mlr? How am I going to cope with not only the challenge of the complexity of machine learning by itself, but also adding a complex structure of R6 class on top of it, that I know nothing about?

But after processing the terrifying news for a while, a new realization emerged… if R6 is worth the learning curve for the developers of mlr3 in such a drastic way that they are going to throw away their entire former efforts on mlr, and never look back, it must have some functionality that give it some advantages… perhaps it is time for me as well to benefit from its advantages.

Don’t understand me wrong! I always wanted to learn and use R6, in spite of already being a huge fan of the S4 system for object oriented classes. However, maybe it was too convenient to seamlessly benefit from it run on the back of many sophisticated packages (tidyverse among many others), that have kept me away from diving into a new unknown world of R6.

While mlr3 is still under development, I thought that at least I can earn some time until it will officially mature into a stable version, so I won’t have to suffer through the painful growing time, and if mlr will no longer be maintained, I could always go back to caret.

But mlr was not alone to be deserted by its developers …caret was experiencing similar scope creep issues. However, in caret’s case, its developer(s) has joined hands with the experts of RStudio, to complete the missing link in the tidyverse framework, and released collection of packages under the umbrella term of ‘tidymodels’, including:

*[rsample](https://tidymodels.github.io/rsample/) and recipes for data pre-processing (transformation, hold-out sampling, etc).
*[parsnip](https://tidymodels.github.io/parsnip/) for model creation
*[yardstick](https://tidymodels.github.io/yardstick/) for Measure model performance

Tidymodels was designed to utilize the tidyverse approach, a mature popular intuitive approach, along with plenty of useful documentation and vignettes. As an avid user of tidyverse, I couldn’t wish for more.


## About cohesive machines…

My previous post discussed the main challenges in designing such ‘meta’ tools, among them, perhaps the most challenging, is the cohesiveness of the entire system, that is, when you work on specific feature, how to tackle it without accidentally harming the other delicate components that expect a very specific behavior / structure from the tinkered component.

<img src="https://drorberel.github.io/img/blog/paper_jam_1.gif">  
Image credit: https://oip.manual.canon/


Did you ever try to fix or troubleshoot one of the big copy machines that does everything from double side printing, binding, and maybe even a cup of milkshake, but with two spoons of sugar? So many delicate parts in such a cohesive system? Go figure. Yet, when an accidental paper jam occurs, what are you going to do next? How can you figure out where exactly the malfunction is? Are you expected to take each piece out until you diagnose the source of the problem?

Alas, no worries! Rest assure, you don’t even need to call a technician! The copy machine is so smart, it will automatically instruct you exactly where to look for the possible issue, so you don’t need to take it all apart piece by piece. Detailed instructions carefully instruct you which specific door/clip to push, to open another hidden drawer where the possible paper jam is. Voila. Fixed. All that is left to do is to close the door. The machine automatically runs a quick self inspection, and is ready for the next task.

What a magical and elegant machine. No need for us to worry about messy ink, destructive laser beams shooting all over. Instead, all the magic is done inside the copy machine. What’s happening in the machine stays in the machine. All the user care about is to get organized, clean, binned printout.


## So why object oriented? What’s happens in the object, stays in the object!

To me, an object oriented system is a constrained restricted class that wraps other things (attributes, functions/methods) inside of it, so things are kept ONLY where they belong, and not all over. Yes, there is a lot of pain in dealing with such constraints, but from a software design perspective, they were deliberately designed to be annoyingly ‘’unbroken” for the sole purpose of saving us from our own clutter!

S3 and S4 are common systems for object oriented classes, where S4 is the more restricted one, supporting multiple dispatches. In my [earlier series](https://drorberel.github.io/2019-04-25-Meta-analysis-of-multiple-multi-omics-data/) of [posts](https://drorberel.github.io/2019-04-15-Bioconductor-S4-classes-for-high-throughput-omics-data/) I described how Bioconductor developers utilize the S4 system for their own complex needs of analyzing large data sets, with advanced statistical methods. While caret and mlr does not support the unflatten structure of Bioconductor’s popular S4 SummarizedExperiment class for genomic assay data, there are Bioconductor packages that attempt to do similar stuff as caret and mlr, i.e. unify multivariate models around the SummarizedExperiment object. Another approach, by the [Bioc2mlr](https://drorberel.github.io/Bioc2mlr/) package, going the other way, flattening the S4 objects into the standard wide data frame format that mlr and caret expect, but with the cost of losing/breaking the tight knots that were holding the multiple slots tied to each other.

## Why R6? The self-assignment, object-frugality approach:

In my own experience, I have learned the hard way, that the less objects one manage in a code, the less clutter and cognitive burden there is to handle, or to put it simpler, there are less places to look for bugs when things are not going as expected.

In line with this object-frugality approach, I am a big fan of working with the pipe operator %>%, which I find is contributing to object-minimized cleaner code. Nevertheless, The %<>% ‘self-assignment’ operator, even do more than that by saving us the redundancy of using the object twice: one time as the origin class to begin with, and the second time as the destination to be assigned with the new revised content.

The R6 system supports an elegant way for object state-changes and reference semantics. What this means is, that you don’t even need an assignment. All you need is to call the method (function) on the object. It will automatically update the designated slot within the object, with the new content that the function creates. Voila. It might take time to get used to, but once you do, traditional code with assignment all of a sudden seems redundant, repetitive, cluttered, and less elegant.

Elegance is of course of personal taste, but to me, it translates into stronger, less “vulnerable” code. It might still break, but because it was very strict to begin with, you will know exactly where to begin exploring for the cause of the issue within the mechanics of the object, and once found, how to locally fix it in a safe way, without accidentally damaging other related parts of your code.

OK, by now you already figured out I am a big fan of object oriented classes, so I may be heavily biased in my following sections :), so back to ML…


## The good old gasoline engine vs the fancy electric one …

Do you drive an electric car? I don’t (yet), but what I know about it is that it is more reliable, and when it breaks, a traditional mechanics may not have the tools to open it and fix it. They might get a very detailed log message of the malfunction code, but they may not even have the tools to uncrack the electric engine cover. Instead, you might need to send it over to a special shop that specializes in this new advanced technology, and they will have the tools to quickly diagnose it, find the problem, and fix it.

Well, my friend, this might be also the case with the comparison of these two refactored ML approaches.


## Similarities:

On the one hand, both mlr3 and tidymodels share the same algorithms for multivariate models, resampling, etc, and will probably achieve very similar model performances (accuracy). There might also be differences in scalability, which are beyond the scope of this post.


## Dissimilarities:

To me, there are 3 main crucial differences, first two are directly derived from the architecture of the R6 class:

### 1. Having an isolated, compacted, separated ‘space’ for the ML analysis (mlr3) vs traditional R function that do something, and return a result in this structure or another (tidymodels).

I simply find it to be more elegant (i.e. stable, consistent, less fragile) when specialized tasks are done in their designated space, that carefully monitor and care only about what is going in, and what is spit out. Nothing more.

mlr3 R6 may not be trivial for many users, but for the ones who appreciate such strict class, it is heaven. tidymodels will keep many others happy with the traditional R S3 system, which also benefits from the flexibility of simply running a function, without worrying too much about edge scenarios that accidentally break the function only when you tweak it all the way to its boundaries. For many users, they may never even be near those boundaries, so why worry about it.

### 2. Connecting the wires by myself when I need to customize it for a new specific need, in the safest way.
<img src="https://drorberel.github.io/img/blog/MacGyver_Anderson_0.jpg">  
Image credit: https://www.syfy.com/

Good packages that last longer are designed with a very wide scope. wide enough for the developers to demonstrate how one can customize the main functions of the package. In the context of ML, customizing and adding new multivariate models (learners), new performance metrics, new transformations etc. Both mlr3 and tidymodels do this very nicely, in terms of documentation (tutorial showing you how to add new methods from scratch), and in terms of strict constraints (yet, up to some level).

However, mlr3’s R6 constraints may be more strict than the ones enforced by tidymodels, which scales up when you knit it together with other parts of the ML methods. In other words, in such powerful, multi function pipeline that integrates resampling + tuning + benchmarking + …, even a small splash of tin (e.g. NAs in a prediction vector that were not supposed to be there) that somehow gets stuck in the wheels, may cause side effects that might break the entire machine. How likely is this to happen? Maybe it will never happen at all, but the best practice to minimize the odds for this to happen, is with a more restricted system.

With my luck, (well, I do enjoy stretching such methods to their limits), it does occasionally happens, and when it does, it is not fun at all to spend precious time on long expeditions for finding the cause of the error. You might not care about it at all, but some people do.

### 3. Pre-processing: customize new composable (monad) transformations

As mentioned in my previous post, the tricky part of the pre-processing is to guarantee that whichever transformation(s) were applied on the training training data set, one should apply on the test data set. This is not a trivial task when one needs to pass on the parameters that keep the transformation details. Both packages also supports a monad programming, meaning, allowing to compose different transformation in any order, while guaranteeing compatibility of the ‘pipes’ to each other.

The less trivial task is to write your own customized pre-processing transformation. This is not trivial at all, in spite of extensive documentation at both [recipes](https://tidymodels.github.io/recipes/articles/Custom_Steps.html) and [mlr3pipelines](https://mlr3book.mlr-org.com/extending-mlr3pipelines.html). Overall, both approaches have similar capabilities, yet, each developed its own terminology and jargon, which might take some time to get used to.

### 4. Survival and unsupervised models

Currently supported by mlr3, but seems like tidymodels developers plan to add it as well.

### 5. Can you please just show me the code ?

[https://mlr3pipelines.mlr-org.com/articles/comparison_mlr3pipelines_mlr_sklearn.html](https://mlr3pipelines.mlr-org.com/articles/comparison_mlr3pipelines_mlr_sklearn.html) has nice code comparison between mlr3 and tidymodels (and others).


## Vision:

Similar to my vision at my previous post, what really matters is for the user to be comfortable with the documentation and to have access to reliable support channels. I also envision that there be developed ‘converter’ functions from one package to another, at least for the transformations. Rich collection of transformation libraries is a shared interest of both approaches, regardless of the above differences.

For some reason, not fully clear to me, both approaches do not yet provide a designated package/functions for ‘[Interpretable Model-Agnostic](https://christophm.github.io/interpretable-ml-book/)’ tools such as [LIME](https://lime.data-imaginist.com/), [DALEX](https://modeloriented.github.io/DALEX/) and [iml](https://cran.r-project.org/web/packages/iml/vignettes/intro.html) (among others). To me these post ML tools seem to be complementary to the common ML framework, though, some of the packages mentioned might already support them


## Bottom line:


### 1. Respect the good old deprecated packages. Don’t jump to the next level before you mastered them!

I would first encourage new ML practitioners to play with the deprecated versions of caret and mlr, just so you become familiar with the basic terms, scope, and functionality of common machine learning components.

Only then, when you feel comfortable with terms like classification, regression, feature engineering, model parameter tuning, cross-validation, ensemble, benchmarking and others, gradually move on and take it to the next level.

### 2. If you don’t have to make it complex, just keep it simple.

Many practitioners probably won’t even care about the differences I mentioned above. In that case, tidymodels would probably be the preferred option. It is simple, straight forward, well documented, and supported.

### 3. Didn’t you always want to learn R6, but never had enough motivation to do so?

Don’t miss this opportunity, or stay behind.

### 4. When things break…

For others, who enjoy tinkering and stretch the limits of the tool’s scope, such concerns about package objects structure, and what are the actual mechanics that are happening at the back end may be more crucial.

By the end of the day, even today’s state-of-the art tool is tomorrow’s deprecated deserted package, but living in the moment, at present, one should bet on the most stable solution, that is less likely to break.

Why would it break? If it wouldn’t break, there would be no need for this post to be written at all.


## Disclaimers:

The author is not affiliated with any of the groups who developed the packages listed above, besides the package [Bioc2mlr](https://drorberel.github.io/Bioc2mlr/).

What about SuperLearner refactored into [tlverse](https://tlverse.org/)? By the way, it was also done with R6. I don’t know. Maybe in a future blog. I would mention, however, that the academic interest of the developer group behind it is mostly motivated and leaning towards Targeted Maximum Likelihood / Minimum Loss-Based Estimation (TMLE), which is a statistical approach for parameter estimation, rather than the other two packages that are perhaps more ‘engineering’ oriented. There is a recent great documentation about it [here](https://tlverse.org/tlverse-handbook/).

Fun fact: mlr3 never mention p-values. Not even sure if that’s on purpose.

Did I forget a key player in this (semi-biased) review? If so let me know. If I don’t have a good reason to exclude it I don’t mind to add it.



Check my [blog](https://drorberel.github.io/blog/) and [github page](https://drorberel.github.io/) for more fun stuff.



[first published at my Medium blog](https://medium.com/@drorberel/meta-machine-learning-aggregator-packages-in-r-round-ii-71ee1ff68642)