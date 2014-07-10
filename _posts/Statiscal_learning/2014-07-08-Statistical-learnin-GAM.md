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

做GAM的package有很多个，这里就使用gam这个package 

{% highlight r%}
install.packages("~/Downloads/gam_1.09.1.tar.gz")
library(gam)
{% endhighlight %}

这里使用gam自己的范例，不过步骤还是参照之间写的 [数据分析步骤][step].

{% highlight r%}
# 这是gam的范例,读入资料
data(gam.data)
gam.object <- gam(y ~ s(x,6) + z,data=gam.data)
plot(gam.object,se=TRUE)
data(gam.newdata)
preplot(gam.object,newdata=gam.newdata)
{% endhighlight %}

可以看到上面对于gam\.data 这笔资料，主要是建立x 和z 之间的关系,那么第一步看无模型假设的图形。

{% highlight r%}
pairs( y ~x+z, data = gam.data)
library(scatterplot3d)
par(mfrow = c(1,2))
scatterplot3d(x,z,y, data=gam.data)
{% endhighlight %}

 ![pairs][]
 ![scatter][]

第二步选择适合的模型: 从pairs plot看出对于x来说对y是有影响的，但是z看起来有点散，感觉z与y之间没有什么关系的样子。这里使用了 s(x,6) 这个其实是一个smoothing operation S,对于x做了一个6阶平滑的函数，其实平时可能不需要做这么多阶的平滑，选择几阶的平滑可以使用cross varidation或者AIC去选择，选一个最好了，先回到刚刚需要加z还是不加z的问题。 这里他的范例也给我们结果

{% highlight r%}
gam.object <- gam(y~s(x,6)+z,data=gam.data)
anova(gam.object)
gam.object2 <- update(gam.object, ~.-z)
anova(gam.object, gam.object2, test="Chisq")
{% endhighlight %}

 ![anova][]

从Anova 的分析表可以看到，对于s(x,6)这个转化来说，p\-value 比较小,几乎接近与0,也就是有影响的，从两个模型变异程度的解释性，看到用chi\-square的解释就并没有一个显著的影响，这里可以判定不需要加上z

第三步 估计的参数以及预测值

{% highlight r%}
gam.object2$coef
(Intercept)      s(x,6)
1.9218            -2.318
summary(gam.object2)
{% endhighlight %}

因为我们这里对于x做了一个转换，那么在做预测的时候可以看x转换后的值

{% highlight r%}
# term s(x,6)
predict(gam.object,type="terms")
# term response 估计值
predict(gam.data,type = "response")
# 预测 
data(gam.newdata)
predict(gam.data,type = "response",newdata = gam.newdata)
{% endhighlight %}

第四步: 模型配适情况

![summary][]

从summary的表格可以看到模型解释掉了 $\frac{38.21}{57.94} $ 的变异程度，还是比较不错的。

后面的假设验证跳过，

给点gam 的小结论，其实gam 相对与简单线性回归更有调整空间，同时也保留了一定的解释能力，不过算法对于资料比较大的情况，可以不太适合。


[Ng]: https://class.coursera.org/ml-005
[VGAM]: http://cran.r-project.org/web/packages/VGAM/index.html 
[step]: http://blog.xjchen.net/statistics/2014/07/09/Data-analysis-step/ 
[pairs]: {{BASE_PATH}}/images/GAM/GAM_pairs.png
[scatter]: {{BASE_PATH}}/images/GAM/GAM_scatter3d.png
[anova]: {{BASE_PATH}}/images/GAM/GAM_anova.png
[summary]: {{BASE_PATH}}/images/GAM/GAM_summary.png
