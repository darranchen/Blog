---
layout: post
title: "Statistical learning- Generalized Additive Model"
date: 2014-07-08 14:46:00
categories: Statistics
tags: 统计  machine_learning Statistical_learning GAM
keywords: 统计 Statistical_learning Generalized_Additive_Model
---

写完Statistical learning 的开篇，一下就开始写 Generalized additive model (GAM) 有点突然，主要是这两天在看 《The elements of statistical learning》的第九章 "Additive Models, Trees and Related Method". 所以先大致介绍一下GAM.

这里的Generalized additive model 可以看成 Generalized linear model的扩充. 在统计学习过程中，最开始学习的是简单线性回归(Simple linear regression)。也就是只有一个解释变数x，对应只有一个因变数y. 建立的模型是

$$ y_i = \beta_0 +\beta_1 x_i + \epsilon_i $$

这里 $\epsilon $ 是一个独立同分布的常态. 之后扩展一点就是 multiple linear regress. 这里的解释变数就可以有多个($p$)

$$ y_i = \beta_0 + \beta_1 x_{1i}+\beta_2 x_{2i}+\ldots+ \beta_p x_{pi} +\epsilon_i= {\bf x}^T{\bf \beta}+\epsilon_i $$

这里的 $\epsilon_i $ 也是服从一个常态的分布. 这里的response 都是服从常态的分布。 如果把response 扩展到 exponential family 的话，那么就成了 generalized linear model. Exponential family 就是可以是binomial distribution, Possion distribution, Exponential distribution, Gamma distribution 等，这个时候需要把response的一个link function:

$$ g(E(y_i\|x_i)) = \beta_0 + \beta_1 x_{1i}+\beta_2 x_{2i}+\ldots+ \beta_p x_{pi}  $$


这个$g$ function 的作用就是将后面的多项式转化为想要的范围。 比如logitic function, 后面多项式的范围是 $(-\infty, \infty)$ 而response 的是 $\mu (X) = Pr(Y =1 \|X)$的范围就是(0,1), 那么 $g(\mu) = logit(\mu) = log(\frac{\mu(X)}{1-\mu(X)})$. 就可以做一个链接。 一般的link function有：

*  logit 和probit $\Phi^{-1}(\mu)$  for binomial probabilities.
*  $g(\mu) = log(\mu)$ for Poisson count 

而从generalized linear model 扩展到 generalized additive model。就是对于每一个变数而言，不是直接$x$, 而是x的某个函数$f(x)$.

$$ g(E(y\|x)) = \beta_0 + \beta_1 f(x_{1i})+\beta_2 f(x_{2i})+\ldots+ \beta_p f(x_{pi}) $$

# Fitting Additive Model

有了基本模型之后，怎么做估计呢。对于基本的Additive model

$$ Y = \beta_0 +\sum_{j=1}^p f_j({\bf X}_j)+\epsilon, $$

给定一些列的函数 $f_j, j = 1, 2,\ldots,p$ 之后，找到最佳的组合，使Penalty Residual sum-of-squares 最小

$$ PRSS = \sum_{i= 1}^N(y_i-\beta_0-\sum_{j=1}^p f_j(x_{ij}))^2+\sum_{j=1}^p \lambda_j\int f^{''}_j(t_j)^2dt_j,$$

上面这个方法是限制在f的二阶导数上面，要求二阶导比较小，给定的$f_j$都是一个cubic spline就是3次的多项式. 看待它的方式可以是看成在原来的估计上面加多一个二阶的惩罚项。 但是这样估计出来的结果不唯一，因为$\beta_0$ 不是identiable, 如果要唯一那么假定$\beta_0 = \bar{y}$. 如果input matrix 是full column rank, 那么上面的PRSS 是一个convex criterion，最小值是唯一存在的。就能找到唯一解，如果input matrix 是singular的话，那么$f_j $ 的linear部分不能被唯一决定，可以nonlinear 可以。

# r Package for GAM

这里先介绍用 [VGAM］




[Ng]: https://class.coursera.org/ml-005
[VGAM]: http://cran.r-project.org/web/packages/VGAM/index.html " 
