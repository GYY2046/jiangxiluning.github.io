---
layout: post
title:  "Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization (Outline)"
categories: 深度学习
comments: true
---
* content
{:toc}

# Week One

## Regularizing your neural network

* L2 正则化
* Dropout
* Early Stop
* 动量法
* Adam

## Setting up your optimization problem

### Normalizing inputs

使得优化过程更快
Substract mean and normalize variance $ x := \frac{x - \mu}{\sigma^2}, \mu = \frac{ \sum_{i=1}^{m}{ x_{(i)} } }{m}, \sigma =\frac{1}{m}\sum_{i=1}^{m}{x^{(i)} ** 2}  $

### Vanishing / Exploding gradients

随着层数多的情况，$W$ 会变得很小或者很大。

### Weight Initialization For Deep Networks

输入乘以一个常数 $ w*C $

### Numerical approximation of gradients

### Gradient Checking

$$ gradapprox = \frac {f(\theta + \epsilon) + f(\theta - \epsilon)}{2\epsilon} \approx grad \\
\frac{\lVert gradapprox - grad \rVert_2 }{ \lVert gradapprox \rVert_2 + \lVert grad \rVert_2 } \leq \epsilon
$$

- Don't use in training - only to debug
- If algorithm fails grad check, look at components to try to identify bug.
- Remember regulariazation
- Doesn't work with dropout. (hard to compute $J$)
- Run at randowm initialization; perhaps again after some training. 

# Week Two

## Mini-batch 

- Mini-batch's size is (1, m) 
  - Vectorization
  - Don't need to see all the samples
  - If small train set, use mini-batch size (64, 128, 256, 512, ..., 1024)
    and make sure mini-batch fits CPU/GPU memory
- Batch is size m Mini-batch
  - takes long time to train
- Stocastic is szie 1 Mini-batch
  - loses speeding-up

## Exponentially Weighted Average
$$ V_t = \beta V_{t-1} + (1-\beta)\theta_t \\ averaging \approx \frac{1}{1-\beta} days $$

## Bias Correction
$$ V_t:= \frac{V_t}{1-\beta^t} （make low bias，fast warming up）$$

## Gradient descent with momentum ##
$$ Vdw := \beta Vdw + (1-\beta)dw \\ Vdb := \beta Vdb + (1-\beta)db \\
w := w - \alpha Vdw \\ b := b - \alpha Vdb $$

## RMSprop
$$ Sdw := \beta Sdw + (1-\beta) dw^2 \\ Sdb := \beta Sdb + (1-\beta) db^2 \\
w := w - \alpha \frac{dw}{\sqrt{Sdw} + \epsilon} \\ b := b - \alpha \frac{db}{\sqrt{Sdb} + \epsilon} $$


## RMSprop + Momentum + Bias Correction (Adam)

$$ Vdw = 0,Sdw = 0,Vdb = 0,Sdb = 0 \\ Vdw = \beta_1 Vdw + (1 - \beta_1)dw Vdb= \beta_1 Vdb + (1 - \beta_1)db \\
Sdw = \beta_2 Sdw + (1 - \beta_2) dw^2 \\ Sdb = \beta_2 Sdb + (1 - \beta_2) db^2 \\
Vdw^{corrected} = \frac{Vdw}{1 - \beta_{1}^{t} } \\ Vdb^{corrected} = \frac{Vdb}{1 - \beta_{1}^{t} } \\
Sdw^{corrected} = \frac{Sdw}{1 - \beta_{2}^{t} } \\ Sdb^{corrected} = \frac{Sdb}{1 - \beta_{2}^{t} } \\ 
  w = w - \alpha \frac{ Vdw^{correted} }{ \sqrt{Sdw^{corrected} } + \epsilon } \\
b = b - \alpha \frac{ Vdb^{corrected} }{ \sqrt{ Sdb^{corrected} } + \epsilon }  $$

## Hyperparameters
$$ \alpha = learning\_rate, \beta_1 = 0.9 (dw), \beta_2 = 0.999 (dw^2), \epsilon = 10^{-8} $$

## Learning rate decay

$$ \alpha = \frac{1}{ 1 + decay\_rate*epoch \alpha_0 } \\
\alpha = 0.95^{epoch} \\
\alpha = \frac{k}{ \sqrt{epoch} }\alpha_0 or \frac{k}{ \sqrt{t} } \alpha_0 $$

# Week Three

## Hyperparameters tuning

### Tuning Process

- $\alpha$
- $\beta$
- number of hidden units
- mini-batch size
- number of layers
- learning rate decay

### Using an appropriate scale to pick hyperparameters

- $\alpha$ log scale search
- $\beta = 0.9 ... 0.999,  1 - \beta = 0.1( 10^{-1} ) ... 0.001( 10^{-3} ), r \in [-3, -1], \beta = 1 - 10^{r}  $ 

## Batch Normalization

### Normalize activation in a network

$$ Z^{(i)} ... Z^{(m)} \\
\mu = \frac{1}{m}\sum_{i}Z^{(i)} \\
\sigma^2 = \frac{1}{m}\sum_{i} (Z^{(i)} - \mu)^2 \\
Z_{Norm}^{(i)} = \frac{ Z^{(i)}-\mu }{ \sqrt{ \sigma^2 + \epsilon} } \\
\widetilde{Z}_{Norm}^{(i)} = \gamma Z_{Norm}^{(i)} + \beta \\
if \gamma = \sqrt{ \sigma^2 + \epsilon }, \beta = \mu, \widetilde{Z}_{Norm}^{(i)} = Z_{Norm}^{(i)}
$$

### Fitting batch norm into a neural network

### Why does batch norm work?

- Make input value more stable, make hidden layer stable
- Each mini-batch is scaled by the mean/variance computed on just that mini-batch
- Adds some noise to the values $Z^{[l]}$ within that mini-batch. So similar to dropout, it adds some noise to each hidden layer's activation
- This has a slight regularation effects

### Batch Norm at test time

Estimate using exponentially weighted  average to estimate $\mu$ and $\sigma^2$

## Multiple-class classification

## Softmax Regression

$$ g(Z^{[l]}) = \frac{ Z^{[l]} }{ \sum_{i=0}^{m}Z^{[l](i)} } $$

## Training a softmax classifier

$$ L(\hat{y}, y) = - \sum_{j=1}^{C} y_2 log \hat(y_2)  $$