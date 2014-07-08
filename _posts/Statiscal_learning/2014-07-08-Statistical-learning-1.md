---
layout: post
title: "Statistical learning 开篇"
date: 2014-07-08 00:26:00
categories: Statistics
tags: 统计  machine_learning Statistical_learning
keywords: 统计 Statistical_learning
---

这是statistical learning 的开篇，开始一步一步介绍基本的statistical learning 的基本知识，对于statistical learning 知识的来源其实专门的课是郑顺林老师的data mining (参考课本是Stanford 的 [The Elements of Statistical Learning][ESL]) 以及在coursera 上 Andrew Ng的[machine learning][Ng]. 但是基本上这两年在统计系学到的东西都是statistical learning的一部分。

这里有很多热门的词语， Machine learning, Artificial intelligence 以及Data mining. 到底这些跟Statistical learning有什么关系呢。

# Data mining

先讲一下 Data mining, 这里是引用了维基上面的定义 [Data mining][DM]

> Data mining (the analysis step of the "Knowledge Discovery in Databases" process, or KDD), an interdisciplinary subfield of computer science, is the computational process of discovering patterns in large data sets involving methods at the intersection of artificial intelligence, machine learning, statistics, and database systems. The overall goal of the data mining process is to extract information from a data set and transform it into an understandable structure for further use. Aside from the raw analysis step, it involves database and data management aspects, data pre-processing, model and inference considerations, interestingness metrics, complexity considerations, post-processing of discovered structures, visualization, and online updating.

可以看到Data mining 的定义其实讲的很清楚，这个它是computer science 的一个分支, 主要是在大的资料集里面发现数据的趋势作为应用，他会和上面讲到的AI, Machine learning, Statistical learning 会有交互的，因为目的都是近似的，就是从数据里面找出信息。 他们主要的交互部分就是如何建立模型，除此之外，Data mining 其实还包括了怎么存储资料，怎么管理资料，资料可视化，还有怎么交互使用资料，这些都是data mining 有别与statistaical learning 的地方。

# Artificial Intelligence, Machine learning and Statistical learning

根据维基上面的解释是

* Statistical learning 是Machine learning 的一个主要的架构

* Machine learning 又是Artificial Intelligence 的一个分支

这里先将Artificial Intelligence 人工智能，根据维基的定义 [AI][AI]

>  Major AI researchers and textbooks define the field as "the study and design of intelligent agents", where an intelligent agent is a system that perceives its environment and takes actions that maximize its chances of success. John McCarthy, who coined the term in 1955, defines it as "the science and engineering of making intelligent machines".

它是指研究和设计一个智能的系统，这个系统可以自动根据周围的环境自动作出收益最高的决策。

而 [Machine learning][ML] 是AI 的分支，是指能够从资料中学习的智能系统

> Machine learning, a branch of artificial intelligence, concerns the construction and study of systems that can learn from data. 


另外有两个非常喜欢的定义，一个是 [Arthur Samuel][AS] 在1959 年定义的:

> "Field of study that gives computers the ability to learn without being explicitly programmed".

这个的大致意思的我们不必给定固定的程式，清楚的目的，设计出一个系统可以让计算机自动学习.

另外一个定义是来自 Tom M.Mitchell 的更加准确的定义

> A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E"

他是指一个程式，这个程式可以根据experience E, 针对于很多个目的， 已经可以衡量表现的P 作出一个清晰的判断，最早看到这个例子是在 Andrew Ng的[machine learning][Ng] 上看到的. Arthur Samuel 开发了一个智能的下国际象棋的程式，以这个程式为例子，程式的任务 T 是赢得棋局， 而 experient E是下过的棋步以及之后的结果，measure 是指每一步下完之后赢还是输的个数。

跟statistical learning 不同的是，它是一个系统，就是包括了整个执行的系统。而statistical learning 是作为根据资料进行判断的一个统计模型。正如维基定义的

> Statistical learning theory is a framework for machine learning drawing from the fields of statistics and functional analysis.

这里注重是数据分析，根据资料进行分析的方式。之后就是将主要介绍数据分析的模型.

[Ng]: https://class.coursera.org/ml-005
[DM]: http://en.wikipedia.org/wiki/Data_mining
[AI]: http://en.wikipedia.org/wiki/Artificial_intelligence
[AS]: http://en.wikipedia.org/wiki/Arthur_Samuel
[ML]: http://en.wikipedia.org/wiki/Machine_learning
[ESL]: http://statweb.stanford.edu/~tibs/ElemStatLearn/
