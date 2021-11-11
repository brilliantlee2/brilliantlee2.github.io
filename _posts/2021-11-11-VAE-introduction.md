---
layout:     post
title:      "(Variational Auto-Encoders)VAE简介"
subtitle:   "VAE? VGAE? GAE?"
date:       2021-11-11 10:00:00
author:     "Brillee"
catalog: false
header-style: text
mathjax: true
tags:
  - GNN
  - VAE
---



> 最近在学习一些关于GNN的内容，碰到模型中用到了(**Variational Graph Auto-Encoder**)VGAE、VAE的内容，又回来补充一下VGAE、VAE的知识。



#### VAE介绍

​	VGAE的基础是VAE，这里先提出VAE的概念。VAE最早来源于2014年Kingma的一篇paper，[《Auto-Encoding Variational Bayes》](https://arxiv.org/abs/1312.6114)

​	VAE是人工神经网络和变分贝叶斯的结合。



​	为了引入VAE，我们简单介绍一下变分贝叶斯。我们知道统计模型由观察变量$X$、未知量$\theta$和隐变量$Z$组成，生成模型通过隐变量来估计观察变量：$p_\theta(z)p_\theta(x|z)$。但是很多时候这个后验概率并不容易得到（隐变量和参数都不知道），所以我们选择通过其他方式来近似估计这个后验概率。贝叶斯统计学传统的推断方法是采用马氏链蒙特卡洛(MCMC)的采样方法，通过抽取大量样本给出后验分布的数值近似，但这种方法的计算代价昂贵。而变分贝叶斯是把原本的统计推断问题转换成优化问题（两个分布的距离），并利用一种分析方法来近似隐变量的后验分布，从而达到原本统计推断的问题。

​	而 VAE 则是利用神经网络学习来学习变分推导的参数，从而得到后验推理的似然估计。下图实线代表贝叶斯推断统计的生成模型$p_\theta(z)p_\theta(x!z)$，虚线代变变分近似$q_\phi(z!x)$。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWM5NS5vc3MtY24tYmVpamluZy5hbGl5dW5jcy5jb20vcGljLWJlZC8yMDIwMDQyNzA5MDUzNi5wbmc?x-oss-process=image/format,png)





​		在paper中，作者提出（Auto-Encoding Variational Bayesian）算法来让$q_\phi(z!x)$近似$p_\theta(x!z)$，同时把最大似然函数的下界作为目标函数，从而避开了后验概率的计算。并且将问题转换为最优化问题，同时可以利用随机梯度下降来进行参数优化。

VAE模型中，我们假设$q_\phi(z!x)$这个后验分布服从正态分布，并且对于不同样本来说都是独立的，即样本的后验分布是独立相同的。

服从正态分布只是算法的一个假设，这个分布是神经网络可以学习到的网络。同时我们要求对于不同样本来说是独立的，如果不是独立的（如下图），这样的结构无法保证通过学到的分布进行采样得到的隐变量$z_i$能够与真实样本$x_i$一一对应，所以就无法保证学习效果了。

如果每个样本都是相对独立的（如下图），我们便能通过每个样本的专属分布来还原出真实样本。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWM5NS5vc3MtY24tYmVpamluZy5hbGl5dW5jcy5jb20vcGljLWJlZC8yMDIwMDQyNzE5MDk1OS5wbmc?x-oss-process=image/format,png)





这也是论文中最重要的一点：
$$
logq_\phi(z!x^{(i)})=logN(z;\mu^{(i)},\sigma^{2(i)},I)
$$
​		VAE 通过构建两个神经网络来分别学习均值和方差$\mu_k=f_1(X_k)$,$log\sigma_k^2=f_2(X_k)$,这样我们就能得到样本$X_k$的专属均值和方差了，然后从专属分布中采样出$Z_k$，然后通过生成器得到$\hat{X_k}=g(Z_k)$,并通过最小化重构误差来进行约束$D(\hat{X_k},X_k)$。



​		但隐变量是通过采样得到的，而不是经过编码器算出来的。这样的重构过程中免不了受到噪声的影响，噪声会增加重构的难度，不过好在这个噪声的强度可以通过方差反应，方差可以通过一个神经网络得到计算，所以最终模型为了更好的重构会尽量让模型的方差为零，而方差为零时，就不存在随机性了，这样模型最终会是一组均值，便退化成了普通的 AutoEncoder。

为了防止噪声为零不再起作用，VAE 会让所有的后验分布都向标准正态分布看齐，衡量两个分布的距离，我们有 KL 散度：
$$
L_{\mu,\sigma^2}=\frac{1}{2}\sum_{i=1}^d(\mu_i^2+\sigma_i^2-log\sigma_i^2-1)
$$
其中，$d$为隐变量的维度。

变分自编码中的变分是指变分法，用于对泛函 $KL(p!q)$求极值。我们将约束两个分布的 $KL$散度加入到损失函数中，则有:

$$
L=D(\hat{X_k})+\frac{1}{2}\sum_{i=1}^d(\mu_i^2+\sigma_i^2-log\sigma_i^2-1)
$$
简单来说，VAE 的本质就是利用两个编码器分别计算均值和方差，然后利用解码器来重构真实样本，模型结构大致如下:

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWM5NS5vc3MtY24tYmVpamluZy5hbGl5dW5jcy5jb20vcGljLWJlZC8yMDIwMDQyNzIxMzAxNy5wbmc?x-oss-process=image/format,png)



> Reference
>
> [《Variational Graph Auto-Encoders》](https://arxiv.org/abs/1312.6114)
>
> [《变分自编码器（一）：原来是这么一回事》](https://kexue.fm/archives/5253)
>
> [VGAE：利用变分自编码器完成图重构](https://blog.csdn.net/qq_27075943/article/details/106267746)





**_Have a nice day~~~_**
