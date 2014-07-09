---
layout: post
title: "数据分析步骤"
date: 2014-07-09 23:34:00
categories: Statistics
tags: Statistics
keywords: 统计
---

在这一章将要介绍统计分析的基本步骤，这里统计分析的基本步骤其实是郑顺林老师在可靠度的课堂上面教的， 步骤的来源其实是郑老师的指导教授Meeker. 他介绍的数据分析的基本步骤如下：

1. Model-free graphical data analysis
2. Model-fitting(parametric, including possible using of prior information)
3. Inference： estimation or prediction( e.g statistical intervals to reflect statistical uncertainty/variablity)
4. Graphical display of model-fitting results
5. Graphical and analytical diagnostics and assessment of asumptions.
6. Sensitivity analysis
7. Conclusion


# 第一步 无模型资料图

第一步, 也是最重要的一步，就是画图，这里的Model-free 的画图，在做数据分析的时候，这里加入是有一个反应变数y, 我们就是跟根据解释变数(input) x来估计y,最简单的资料生成如下面所示

 $$  y = 2+2*x_1+(x_2-1)^2+\epsilon $$

{% highlight r%}
## 生成资料
x1 <- rnorm(20,mean=4)
x2 <- rnorm(20,mean=4,sd=2)
error <- rnorm(20)
y <- 2+2*x1+(x2-1)^2+error
Data <- data.frame(x1,x2,y)
{% endhighlight %}

像上面一笔资料，总共有20笔，有两个解释变数，x1 和x2, 我们现在想用x1,x2,来猜y,那么第一件事就是找出y跟x1和x2之间的关系大概是怎么样子的，因为模型是在太多种了，不能够随便就给了吧。最简单方式就是画散点图,可以有两种方式，一种直接用pairs 就可以了，另一种就是自己生成一对一的scatterplot

{% highlight r%}
pairs(y ~ x1+x2,main = "pairs(y~x1+x2)")
{% endhighlight %}

 ![pairs][]

另一种是一对一的散点图
{% highlight r%}
par(mfrow=c(1,2))
plot(Data$x1,Data$y,xlab = "x1",ylab="y")
plot(Data$x2,Data$y,xlab="x2",ylab = "y)
{% endhighlight %}

![ELplot][]

还可以画三维的散点图
{% highlight r%}
par(mfrow = c(1,2))
scatterplot3d(x1,x2,y)
scatterplot3d(x2,x1,y)
{% endhighlight %}

![scat][]


从上面的三张图形都可以看的出来，y和x2有明显的关系，而且不是直线的，从x2对y的散点图可以感觉是曲线的，这里可以用exp或者用二次来描述这条曲线，这里就用二次,而对于x1来说，尽管散点图有点分散，但是还有可以看到x和y有一定的直线线性关系的.

# 第二步  选定模型与参数估计

根据上面，我们就选定了模型为

$$ y_i = \beta_0+\beta_1*x_1 + \beta_2*x_2+\beta_3*x_2^2 +\epsilon $$

$\epsilon \sim N(0,\sigma)$
使用r里面的lm package 估计

{% highlight r%}
model.fit <- lm(y ~x1+poly(x2,2))
summary(model.fit)
anova(model.fit)
{% endhighlight %}

可以看到估计以后的结果:
![mdfit][]


从summary的表格可以看到估计得到的模型为

$$ y_i = 14.08+2.00*x_1+151*x_2+57*x_2^2 $$

其中$sigma$ 可以由anovo table看出是1.1

# 第三步 模型论断

参数估计完了之后，那么就要针对对应的模型进行推论，推论所估计的参数的取值范围以及参数的显著性，就是这个模型的参数到底有没有用，比如我们使用 $y = \beta_0+\beta_1x_1$ , 那么 $x_1$ 是不是有效果，有没有效果就是根据 $\beta_1$的估计值来决定的，如果 $\beta_1$估计值很可能为0，那么其实 $\beta_1 x_1$ 可能就是0，也就是没有加上$x_1$也没关系。这里估计出来的取值范围可以从上面的summary表格得到。可以看到p-value 都是小于0.05的.

模型估计出来之后，怎么预测值呢,使用predict

{% highlight r%}
new = data.frame(x1=2,x2=2)
predict(model.fit,newdata=new)
23.31237
{% endhighlight %}

另外估计值信赖区间和预测值的信赖区间都可以求出来

![LM_con][]

{% highlight r%}

png("LM_conf.png",width =640,height=480)
par(mfrow= c(1,2))
new = data.frame(x1 = seq(-2,6,by =0.5),x2=seq(-2,6,by=0.5))
predict(lm(y ~ x1+poly(x2,2)),new,  se.fit = TRUE)
pred.w.plim <- predict(lm(y ~ x1+poly(x2,2)),new, interval = "prediction")
pred.w.clim <- predict(lm(y ~ x1+poly(x2,2)),new, interval = "confidence")
matplot(new$x2, cbind(pred.w.clim, pred.w.plim[,-1]),lty = c(1,2,2,3,3), type = "l", ylab = "predicted y")
points(x2,y,pch = 2,cex = 0.2)
matplot(new$x1, cbind(pred.w.clim, pred.w.plim[,-1]),lty = c(1,2,2,3,3), type = "l", ylab = "predicted y")
points(x1,y,pch = 2,cex = 0.2)
dev.off()
{% endhighlight %}


# 第四步 模型的配适情况

模型配好之后，下一个关心的问题就是到底模型配得好不好，很正常的想法，模型配得好了，预测才会准啊，那么怎么看模型配的好不好呢，最直接的指标就是看$R^2$ 上面看到$R^2 =0.992$ 这个是非常好的，也就是模型解释掉了99.2%的变异，这里除了看指标之外，还希望能通过画图来判断，那么怎么画图呢，因为我们是配适一个三位的平面,直接看其实也是有难度的，如果是$y = \beta_0 +\beta_1 x_1+\beta_2 x2$这种线行的，其实比较好画


{% highlight r%}
s3d <- scatterplot3d(x1,x2,y,highlight.3d =TRUE,type = "h")
lm.model.fit <- lm(y~x1+x2)
s3d <- plane3d(lm.model.fit)
{% endhighlight %}

![LM_plane3d][]

线性配适的效果其实也是不错的，这里主要是讲步骤，所以也没有专门去找怎么看，其实另一个做法是自己把多项式做出来的surface画出来，再把点画上去看效果，夜深了就忽略过去吧。大概想法就是要用图去看效果.其实可以从上面的边际图看出效果，这个是marginal的图，不是三位图，所以也不是很精准.

# 第五步 模型假设的检查

在linear model中，其实假设检查是很重要的，因为上面的推断都是根据定好的假设，比如之前的系数$\beta$的信赖区间，和显著水平都是建立在假设$\epsilon \mathop{\sim}^{iid} N(0,sigma)$ 上面的， 基本的假设就有三种

* 独立
* 同分布
* 常态分布

这个检查是有意义的，我们可以从residual 中看到模型还有什么地方没有补充完整的，如图所示 ![diag1][]

第一个独立性可以由 residual VS fitted 的图看出来，它的residual 跟y=0 对称而且比较乱，没有明显的趋势，所以是可以猜测是独立的,而从Q-Qplot 可以看出residual 是服从常态的


如果是用 $ y = \beta_0+\beta_1x_1+\beta_2x_2$ 效果如下图所示 ![diag2][]

就会看到第一张 residual VS fitted 的图有明显的residual有趋势存在, Q-Qplot 也没有在一条线上。

这个时候就说明模型还有一些东西没有找出来，再看针对 $x_1$ 和 $x_2$ 的residual 图 ![resi][]

由图形可以看到，对于第二个模型来说，$x_1$ 和$x_2$对residual 都没有跟y=0对称，而且对于$x_2$来说，还有明显的曲面趋势，可以猜测是一个二次的趋势.这样就可以判断第一个模型是有用的，而且是假设满足了。

# 第六步 敏感度分析 Sensitivity analysis

这个词可遭人恨了，因为郑老师最喜欢就是敏感度分析，这个时候就要来看一下，到底什么是敏感度分析了.看下维基的定义 [Sensitivity analysis][sens]

> Sensitivity analysis is the study of how the uncertainty in the output of a mathematical model or system (numerical or otherwise) can be apportioned to different sources of uncertainty in its inputs. A related practice is uncertainty analysis, which has a greater focus on uncertainty quantification and propagation of uncertainty. Ideally, uncertainty and sensitivity analysis should be run in tandem. 

他这里的意思就是，我们对于我们的建立的统计模型来说,对于一些不确定的因素是有多么敏感。在我们这里，这个不确定因素包括了我们刚刚看到的对于 $x_2$ 跟 $y$之间的关系，我们是给二次，也有可能是exp 这些可以描绘那个曲线的,这个不确定性到底会不会对结果造成很大的影响.建立模型 $lm(y\~ x_1+\exp(x_2))$ 看残差图，对于给定函数还是很敏感的，所以要给对的函数。 ![diag3][]

# 最后一步 给结论

正常都要给结论。困了就算了。哈哈


[pairs]: {{BASE_PATH}}/images/LM-pairs.png
[ELplot]: {{BASE_PATH}}/images/Element-wise-plot.png
[scat]: {{BASE_PATH}}/images/LM_scatterplot3d.png
[mdfit]:  {{BASE_PATH}}/images/LM_model_fit.png
[LM_plane3d]:  {{BASE_PATH}}/images/LM_plane3d.png
[LM_con]:  {{BASE_PATH}}/images/LM_conf.png
[diag1]:  {{BASE_PATH}}/images/LM_diag.png
[diag2]:  {{BASE_PATH}}/images/LM_diag2.png
[diag3]:  {{BASE_PATH}}/images/LM_diag3.png
[resi]:  {{BASE_PATH}}/images/LM_residual.png
[sens]: http://en.wikipedia.org/wiki/Sensitivity_analysis
