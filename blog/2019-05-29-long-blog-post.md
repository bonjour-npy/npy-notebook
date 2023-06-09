---
slug: www
title: 激活函数与Loss的梯度
authors:
  name: 倪培洋
  title: Writer
  url: https://github.com/bonjour-npy
  image_url: https://avatars.githubusercontent.com/u/73021573?s=400&u=fd0ab2ee83e7131d53c99d4a4e8a1e18786c6f80&v=4
tags: [hola, docusaurus]
---

## 一、激活函数

1. Sigmoid函数 / Logistic函数

   $$
   \sigma(x) = \frac{1}{1 + e^{-x}}
   \tag{1}
   $$

   $$
   \frac{{\rm d}\sigma}{{\rm d}x} = \sigma{(1 - \sigma)}
   \tag{2}
   $$


   优点：可以将数据压缩至[0, 1)区间内，有较大实用意义

   致命问题：在输入值较小或较大时，Sigmoid函数的梯度趋近于零，会导致网络参数长时间得不到更新，即梯度弥散问题

   ```python
   from torch.nm import functional as F
   import torch
   x = torch.linspace(-100, 100, 10)
   F.sigmoid(x)  # 当x为100时，sigmoid(x)就接近于0了
   ```

2. 线性整流单元（Rectified Linear Unit, ReLU）
   $$
   f(x) = 
   \begin{cases}
   0 & x < 0\\
   x & x \geq 0\\
   \end{cases}
   \tag{1}
   $$

   $$
   \frac {{\text d}f(x)}{{\text d}x} = 
   \begin{cases}
   0 & x < 0\\
   1 & x \geq 0\\
   \end{cases}
   \tag{2}
   $$

   ```python
   from torch.nm import functional as F
   import torch
   x = torch.linspace(-100, 100, 10)
   F.relu(x)
   ```

------

## 二、损失函数

1. Mean Squared Error 均方误差

   - L2范数是对元素求平方和后再开根号，需要.pow(2)后才可作为损失函数
     $$
     Loss_{MSE} = \sum{[{y - f(x)]^2}}
     \tag{1}
     $$

     $$
     \bigg\vert \big\vert y - f(x) \big\vert \bigg\vert_2 = \sqrt[2]{\sum{[y - f(x)]^2}}
     \tag{2}
     $$

     

   - 微小的误差可能对网络性能带来极大的影响

2. Cross Entropy Loss 交叉熵损失

   - binary 二分类问题
   - multi-class 多分类问题
   - 经常与softmax激活函数搭配使用

