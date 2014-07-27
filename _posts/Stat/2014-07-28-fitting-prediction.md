---
layout: post
title: 统计思想系列-model fitting 和prediction之间不同。
date: 2014-07-28 03:24:00
categories: Statistics
tags: model_fitting model_prediction Statistics
keywords: model_fitting model_prediction
---

一个模型有它的 fitting 能力和 prediction 的能力。在做统计模型的时候经常会把 model_fitting 和model_prediction这两件事情混起来，model_fitting 在台湾翻译成为模型配适，在大陆有翻译成为模型拟合度。而model prediction 大家都一样，预测。为了避免用词上面的困惑，还是都使用英语. 这两件事情会混起来的主要原因是我们经常作出一个model 之后，去看这个model fitting 的情况，然后再做prediction. 如果model fitting 做得好，那么做prediction就会好。Model fitting 做得不好，那么prediction就不会好。事实上他们做的事情是很大差别的，目标，作用以及所看的指标也不一样，下面就慢慢解释开来。

首先讲一下model fitting 和 model prediction 各自在做什么事情。 Model fitting， 根据它的名字就是这个模型跟这笔资料的适合程度。在通俗一点，就是手头上有一笔资料，有x 和y, 我们提出一个模型x去解释y,这个模型能不能跟这笔资料匹配，匹配程度如何。举我们之前做房地产资料来讲，现在有台北市的房价资料，里面记录了每个房子有多少个房间x，以及出售的单价是多少y, 我们建立的

$$y = \beta_0+\beta_1 x$$

这个模型，那么这个模型跟这笔资料给我们的讯息到底符不符合。而对于做model prediction这件事情来说，是指我们手上有一笔资料，那么我们建立了一个模型，之后随便给一个新的x，然后去预测y的值，预测的准不准。对应于上面，就是如果建立了上面房价对应于房间数的模型，我给了一个房间数目，那么就要来预测单价是多少。可以看到 model fitting 跟model prediction之间有一个很不一样的地方。Model fitting 重在解释，而prediction重在预测。

那么Model fitting 的作用是什么。 前面讲的，就是解释，根据这笔资料得到x和y之间的关系是怎么样的。 比如上面如果我得到的模型是

$$ y = 3+0.2 x$$

那么我可以得到的信息是如果房间数目增加一个，那么房子的单价会多0.2个单位。 这个0.2到底准还是不准，那就要加入分配假设进来，计算confidence interval 了。

而 Model prediction 的作用就不用讲了，就是做预测啊。 

用什么指标去看模型的fitting 能力和prediction能力呢。 对于fitting 的能力，可以用统计上面常见的 $R^2$. $R^2$的意思可以解释为这个模型解释的y多少不确定性(变异程度). 比如我们模型的是$y = 3+0.2 x$, 给定一个房间数x, 根据这个模型房价估计值就用 $\hat{y}$来表示，我们有这笔资料，那么我们就知道它真实的房价 $y$值。

$$ R^2 = 1- \frac{\sum_i^n (y_i-\hat{y}_i)^2}{\sum_i^n (y_i-\bar{y}} $$

其中 $\bar{y}$ 就是放入模型这笔资料的平均数。 后面的分式可以这样理解， 在分子是指 $y-\hat{y}$ 也就是真实值和这个模型的估计值之间的差别，而分母是 $y-\bar{y}$，可以想象没有任何信息的时候，我们会用平均数去猜房价的值，后面的分式就是我的模型跟我去瞎猜它们差别的比值，后面分式越小，也就是分子越小，这个模型的估计值和我看到的真实值之间的差别越小，可以看到 $R^2$ 越大，后面分式越小，模型fitting 越好。

而对于prediction的能力而言，看的指标就跟fitting 的 $R^2$ 不一样的。预测是随便来一个值，去预测，$R^2$ 实际上面在做的是，我用了这笔资料建立模型，然后又用这个模型来猜这笔资料，如果用 $R^2$ 看prediction的能力，这不就是球员兼裁判么。预测就需要用cross validation的prediction error 作为指标了。 Cross validation 的做法时，把资料切成两份，一份来建立模型，一份用建立起来的模型去根据x值去预测y值。 这个做法就是假装了我们只拿到了前面用于建立模型的资料，后面又来了一份新的资料，我们去看前面建立好的模型应用于这笔新的资料上面的预测情况。

建立起一个模型的时候一定要先考虑清楚，这个模型到底是在做什么事情的，到底是要用来做fitting 的还是要做prediction的。我们建模过程都会先去看fitting 的情况，一般而言fitting 的error 会比prediction的error 小。就跟我前面说的，球员兼裁判，你不好谁好啊。 会不会有模型是fitting 比较好，但是prediction很差的。 这必须有的。 这就是标准的overfitting。也就是过度对于个体的行为进行建模，在做prediction的时候用其他个体的特有的行为去预测新的个体，那么预测就会差的啊。 
