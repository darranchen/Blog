---
layout: post
title: "使用贝式模型分析每期六合彩尾数出现的机率值"
tagline:
date: 2014-08-24
categories: Program
tags: program Bayesian
keywords:   六合彩 Bayesian Hypergeometric
discription: 
---

这个项目主要是源于我在硕一的时候修的贝式分析， 这门课的期末作业是自己找一个可以应用贝式分析的资料，进行分析， 有一次回家，到一个朋友家做客，发现他自己在统计六合彩的资料，就是对于每期哪个尾数有出现，哪个生肖有出现做了一个很漂亮的excel表格，表格的格式如下

![table][]

我这朋友本来并没有读过大学或者统计之类的，但是用他自己觉得适合的方式去呈现他想看的数据，这不是我们专业干的事么，于是就把他的资料拿过来，看看能不能做一些统计的分析。 

六合彩全部有49个号码，一期开出6个平码，1个特码，共7个号码， 那么拿到这笔资料最原始的想法就是，这些号码是不是每一期开出的机率都是一样的，如果是一样的那么每一期买哪个号码都没差，如果有一些号码出现的机率比较高，有一些号码出现的机率值比较低，那么决策应该是买出现机率比较高的。单纯看六合彩开奖的问题， 就是从49个号码里面取出不放回的抽出7个号码，理想的情况下，也就是每一个号码被抽到的机会是一样。 这是在理想状况下面产生的，但是事实上每一个号码由于放置位置的不同，球的重量不一定一致的问题，这种情况都会使某些号码出现的机会会高一些。六合彩的摇奖放置如下图

![jiang][]


但是如果纯粹去看每个球出现的机会，因为有49个球，资料并没有很多，这里有的资料只有262期的资料，并没有足够的资料可以来表现是否有不同，换个方式，在这里，某种程度上相信球的摆放位置会对于出现的机率有影响，可以看到摆放方式是按最后一位数字0，1，2，...,9 的顺序排放的，其实在六合彩的赌博里面是有尾数这个东西的，尾数也就是看最后一个数字是什么，这样子49个球就成为了10组尾数的集合，0的尾数有4个， 其他的尾数有5个球。 现在我关心的东西就变成了下一起会出现什么尾数。

数学化这个问题，这个问题就成了总共有49个球，有10类里面抽7个球，关心每一次7个球的类别组合的机率，它的机率分布就成了一个多维超几何分布。 课堂上面经常介绍的超几何分布的例子是二维的超几何分布，有两种粉笔，红色和黑色，红色的粉笔有 $M_r = 4$ 条，黑色的粉笔有 5 条，总数 $N = 9$根粉笔，从中取出不放会的抽出 $n= 3$ 个粉笔，在二维的超几何问题里面可以关心这次抽取出来红色会有多少根的分布就是一个超几何分布，如果知道了有多少根红色，那么也就知道了有多少根黑色的， 对于多维的超几何分布，在这49个球里抽7个，各个类别的个数是多少的机率值，知道这个机率值就可以得到对应于这一期某个尾数出现的机率。对于二维的超几何分布，其机率值为

$$
p(red = x)  = \frac{\binom{M_r}{x}\binom{N-M_r}{n-x}}{\binom{N}{n}}
$$

扩展到多维的情况，假如有 k 类， 每一类有 $M_i$ 个， $ i = 1,2,\ldots,k$ ,总共有 $N = \sum_{i=1}^k M_i$, $n$ 是取出不放回抽取的个数，那么考虑在这 $n$ 次抽取里面各个类别有 $x_1,x_2,\ldots,x_k$, 那么他的机率值就是

$$
p(X_1,X_2,\dots,X_k|N,n,M_1,M_2,\dots,M_k) = \frac{\prod_{i=1}^k\binom{M_i}{X_i}}{\binom{N}{n}}
$$

贝式的想法是针对未知参数 $M_1, M_2, \ldots, M_k$ 有一个先验分布 (Prior distribution)，获得资料后，根据我们的资料得到一个后验分布(Posterior distribution)，事实上我们知道这里每一类球的个数分别是 $4,5,5,\ldots,5$, 但在这里我们考虑但是上面的机率计算公式是在考虑每一个球被抽到的机率都是一样的，对于这里我们关心的是每一类球可能出现的机率并不一致，我们把这机率的不一致性转化成为假设 $M_1,M_2,\ldots,M_k$ 是未知的，只由出现的机率决定的， 那么对于这些未知参数 $M_1,M_2,\ldots,M_k$ 要寻找一个适合的Prior distribution. 

先从简单的只有两个类别开始，两个类别（ $k=2$)的模型为 Beta-binomial Model. 前面有介绍到对于两类的 likelihood function 

$$
L(x;N,n,M)  = \frac{\binom{M}{x}\binom{N-M}{n-x}}{\binom{N}{n}}
$$
这里 $N$ 是总球数， $n$ 是抽取的个数，$M$ 是第一类的个数，这里 $N,n $ 是固定的， $M $ 是服从一个二项分布(binomial distribution). 

$$ 
P(M|N,p) = \binom{N}{M}p^M(1-p)^{N-M}
$$

而这里的未知参数 $p$ 服从一个 Beta( $\alpha,\beta$) 分布。

$$
P(p|\alpha,\beta)= \frac{p^{\alpha-1}(1-p)^{\beta-1}}{B(\alpha,\beta)}
$$
其中

$$
B(\alpha,\beta)=\frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}
$$

那么 Beta-binomial distribution $\pi(M|N,\alpha,\beta)$

$$
\pi(M|N,\alpha,\beta)=\binom{N}{M}\frac{B(M+\alpha,N-M+\beta)}{B(\alpha,\beta)}
$$

给定这一抽取的球类的个数 $x$，以及参数 $\alpha, \beta$ 之后，那么Postribution distribution 为

$$
\pi(M|N,\alpha,\beta,x)= \frac{f(x|M,N,n)\pi(M|N,\alpha,\beta)}{f(x|N,n)}
                                                         = \binom{N-n}{M-x}\frac{B(M+\alpha,N-M+\beta)}{B(\alpha+x,\beta+n-x)}\\  = \pi(M|N,\alpha+x,\beta+N-x)
                                                         $$

在Prior distribution 是beta-binomial distribution 的前提下， Posterior Distribution 还会是一个 beta-binomial distribution, 不过参数变成了 $x+\alpha, N-x+\beta$.

根据上面的推导方式, 考虑多维的情况，对于beta-binomial distribution, 它的多维分布为Dirichelet-multinomial distribution, 给定参数 
$\vec{\alpha} = (\alpha_1,\alpha_2,\ldots,\alpha_k)$ , 假如下一期开出的结果在每一类中的个数为 $x_1,x_2,\ldots,x_k$. 那么得到资料之后它的Posterior distribution也是服从一个Dirichelet-multinomial distribution,它的参数为 $(\alpha_1+x_1,\alpha_2+x_2,\ldots,\alpha_k+x_k)$. 

那么这些参数要如何给比较好， 一开始有的资讯就是0的尾数的球的个数是4个， 1到9的尾数的球的个数是5个， 因此 Prior distribution 的参数给定 $ (\alpha_1,\alpha_2,\ldots,\alpha_k) = (4, 5, \ldots, 5)$, 我们的资料是到2013年6月18号为止的 262期的开奖记录，统计到这10类总共出现的个数 $(x_1, x_2, \ldots, x_k) = ( 145,194,184,185,205,172,167,217,187,178) $ 

<table class="table table-bordered table-striped table-condensed">
<tr>
<td>尾数</td>
<td>0</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
<td>6</td>
<td>7</td>
<td>8</td>
<td>9</td>
</tr>

<tr>
<td>Prior $\vec{\alpha}$</td>
<td>4</td>
<td>5</td>
<td>5</td>
<td>5</td>
<td>5</td>
<td>5</td>
<td>5</td>
<td>5</td>
<td>5</td>
<td>5</td>
</tr>

<tr>
<td>$\vec{x}$</td>
<td>145</td>
<td>194</td>
<td>184</td>
<td>185</td>
<td>205</td>
<td>172</td>
<td>167</td>
<td>217</td>
<td>187</td>
<td>178</td>
</tr>

<tr>
<td>$\vec{x}+\vec{\alpha}$</td>
<td>149</td>
<td>199</td>
<td>189</td>
<td>190</td>
<td>210</td>
<td>177</td>
<td>172</td>
<td>222</td>
<td>192</td>
<td>183</td>
</tr>

</table>

有了Posterior distribution 的参数之后，那么就能得到我们关心的东西，也就是下一期各个尾数出现的机率值为多少，这里使用蒙地卡罗模拟的方式得到我们关心的机率值，具体步骤如下:

1. 定义重复抽取的次数为K, 也就是做K次模拟实验,
2. 从Dirichelet($\vec{\alpha}+\vec{x}$) 中产生每一类被抽到的机率 $\vec{p} = (p_1,p_2,\ldots,p_{10})$,
3. 有了各类抽取的机率 $\vec{p}$ 之后，从一个Multinomial(N=49, $\vec{p}$) 的分布抽取 $ M_1, M_2,\ldots,M_k$ 从10类里面产生N = 49 个,每一个出现的机率值为 $\vec{p}$ 得到在这49个球里面，每一类的个数 $M_1,M_2,\ldots,M_k$,
4. 有了这10类的个数 $\vec{M}$ 总数49，以及要抽取7个球，这就会到了49个球取出不放回的抽样，抽出7个球，各类的个数 $x_1,x_2,\ldots, x_{10}$ 就是服从超几何分布的，
5. 重复上面2-4的动作知道K次位置，计算我们关心的机率，也就是在这K次里面有多少抽到了我们关心的尾数，如果出现了 $K_i$ 那么机率就是 $\frac{K_i}{K}$

令重复次数为K = 10000, 通过模拟之后得到各个尾数下一期出现的机率值为下表所示:


<table class="table table-bordered table-striped table-condensed">
<tr>
<td>尾数</td>
<td>0</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
<td>6</td>
<td>7</td>
<td>8</td>
<td>9</td>
</tr>

<tr>
<td>$P(Y|\vec{\alpha})$</td>
<td>0.429</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
<td>0.508</td>
</tr>

<tr>
<td>$P(Y|\vec{\alpha}+\vec{x})$</td>
<td>0.437</td>
<td>0.541</td>
<td>0.523</td>
<td>0.524</td>
<td>0.562</td>
<td>0.498</td>
<td>0.488</td>
<td>0.584</td>
<td>0.528</td>
<td>0.510</td>
</tr>
</table>



[table]: {{BASE_PATH}}/images/Hypergeometric/table.png 
[jiang]: {{BASE_PATH}}/images/Hypergeometric/01.jpg 
