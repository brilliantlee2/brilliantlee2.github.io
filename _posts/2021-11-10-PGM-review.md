---
layout:     post
title:      "Probabilistic Graphical Models(PGM)简介"
subtitle:   "PGM初识"
date:       2021-11-10 10:00:00
author:     "Brillee"
catalog: false
header-style: text
tags:
  - PGM
---









##### 概率图模型：

- 概率图模型是概率论和图论相结合的产物，为统计推理和学习提供了一个灵活的框架。
- 概率图模型用节点表示变量，节点之间的边表示局部变量间的概率依赖关系。
- 在概率图模型的表示框架下，系统的联合概率分布表示为局部变量势函数的连乘积
- 该表示框架不仅避免了对复杂系统的联合概率分布直接进行建模，而且易于在图模型建模中引入先验知识。



##### 概率图模型统一了目前广泛应用的许多统计模型和方法：

- 马尔科夫随机场(MRF)
- 条件随机场(CRF)
- 隐马尔科夫模型(HMM)
- 多元高斯模型
- Kalman滤波、粒子滤波、变分推理等



##### 概率图模型的例子有：

<img src="https://i.loli.net/2021/11/10/do12LShmFpOPMG3.png" alt="image-20211110105600677" style="float;" />

![image-20211110105941848](https://i.loli.net/2021/11/10/PwpK3hJdl4EZgtq.png)

![image-20211110110157091](https://i.loli.net/2021/11/10/pvY9XjaIrO2HEnN.png)

#### 概率图模型的研究内容：

- 概率图模型的表示
- 概率图模型的推理
- 概率图模型的学习



##### 概率图模型的表示：

- 概率图模型的表示刻画了模型的随机变量在变量层面的依赖关系，反映出问题的概率结构，为推演算法提供了数据结构。概率图模型的表示方法主要有贝叶斯网络、马尔科夫随机场、因子图等。
- 概率图模型主要研究的问题是，为什么联合概率密度可以表示为**局部势函数**的联乘积形式，如何在图模型建模中引入先验知识等。

##### 概率图模型的推理：

- 概率图模型推理包括求边缘概率、求最大后验概率状态、求归一化因子等问题。
- 概率推理相当于模型求解，在一般图模型中，概率推理是 NP 难问题。

![image-20211110111203841](https://i.loli.net/2021/11/10/kEbgzFpA9cswKOQ.png)



##### 概率图模型的应用举例：

- 图像分割



![image-20211110111326791](https://i.loli.net/2021/11/10/jle9h2vUpIDAJ8W.png)

​                             Kim, T.; Nowozin, S.; Kohli, P. & Yoo, C. Variable Grouping for Energy Minimization. *CVPR,* **2011**

- 医学图像处理



![image-20211110111413689](https://i.loli.net/2021/11/10/cj9qPgb1k4C7JG3.png)

Jianwu Dong et al. Phase unwrapping with graph cuts optimization and dual decomposition acceleration for 3D high‐resolution MRI data. MRM 2016



- 图像去噪



![image-20211110111503795](https://i.loli.net/2021/11/10/EtGspLUvifuYAWr.png)

Felzenszwalb P F, Huttenlocher D P. Efficient Belief Propagation for Early Vision. International Journal of Computer Vision, 2006, 70(1):41–54.







**_Have a nice day~_**
