---
title: 'Gradient boosting'
date: 2021-12-10
permalink: /posts/gradient_boosting/
tags:
  - tutorial
  - machine learning
---

_THIS POST IS CURRENTLY UNDER CONSTRUCTION_

Boosting is a machine learning paradigm that entails constructing an ensemble of models in an iterative fashion where upon each iteration, a new model is added to the ensemble that compensates for the weaknesses of the previously selected models. 

Review of the supervised learning task
--------------------------------------

Before getting into the meat of gradient boosting, let's review the basic supervised learning task so that we are on the same page regarding both the problem setting and mathematical notation. 

In supervised learning, our goal is to map each element in a set $\mathcal{X}$ to a target variable in a set $\mathcal{Y}$. That is, we seek a specific function 

$$F: \mathcal{X} \rightarrow \mathcal{Y}$$

The space of objects $\mathcal{X}$ depends on the problem being solved. It may consist of text numerical vectors, documents, images, or graphs. The space $\mathcal{Y}$ usually consists of real numbers (in the case of regression) or a set of finite labels (in the case of classification). 

We don't have access to the optimal function, $F$, directly. Instead, we have access to a finite set of pairs $\{(x_i, y_i)}_{i=1, \dots, n}$ where $x_i \in \mathcal{X}$ and $y_i \in \mathcal{Y}$ such that $F(x_i) = y_i$.  Our goal is to approximate $F$ from this finite sample. 

To perform this approximation, we search a space of functions $\mathcal{H}$ for a function $f \in \mathcal{H}$ that minimizes a function, called the [loss function](https://en.wikipedia.org/wiki/Loss_function), that tracks the correspondence between each $y_i$ and each $F(y_i)$. That is, the loss function $\ell(y_i, f(x_i))$ tells us how much $f(x_i)$ deviates from $y_i$.

Then, we seek to find an $f \in \mathcal{H}$ that minimizes the average loss over all of the samples:

$$\hat{f} := \text{arg min}_{f \in \mathcal{H}} \frac{1}{n} \sum_{i=1}^n \ell(y_i, f(x_i))$$

Boosting
--------

Gradient descent in machine learning
------------------------------------

Gradient descent is a numerical approach for solving an optimization problem that involves iteratively improving our solution by taking small steps along the direction of steepest descent down the objective function's surface.

Specifically, let's say we are attempting to minimize the function $L(\theta)$ where $\theta \in \mathbb{R}^p$ and $L(\theta)$ is differentiable. Then, gradient descent starts by choosing an initial guess of the solution $\theta_0 \in \mathbb{R}^p$ and iteratively updates our estimate by following the direction of steepest descent.  Recall, the direction of steepest _ascent_ is given by the function's [gradient](https://en.wikipedia.org/wiki/Gradient). Specifically, at the $t$th iteration, the update is

$$\theta_t := \theta_{t-1} - \alpha \nabla L(\theta)$$

where $\alpha$ is called the [learning rate]() and simply dictates how far we will step in the direction of the negative gradient.

In machine learning, it is common to search over a set of models, $\mathcal{H}$, that are each characterized by a set of parameters $\theta \in \mathbb{R}^p$. That is, we can write each $f \in \mathcal{H}$ as $f(x; \theta)$. For example, if $\mathcal{H}$ is the set of all [neural networks](https://en.wikipedia.org/wiki/Artificial_neural_network) with a specific architecture, then $\theta$ are the weights of the neural network. 

Then, if $f(x; \theta)$ is differentiable with respect to $\theta$, we can let 

$$L(\theta) := \frac{1}{n}\sum_{i=1}^n \ell(y_i, f(x_i; \theta))$$

and, with help from the [chain rule](https://en.wikipedia.org/wiki/Chain_rule), we can apply gradient descent as described above to find the $\theta$ that minimizes $L(\theta)$. 

The gradient boosting algorithm
-------------------------------

In the previous section, we assumed that each model $f \in \mathcal{H}$ is parameterized by $\theta$. Furthermore, we assumed that $f(x; \theta)$ were differentiable with respect to $\theta$. With these two assumptions, we discussed how gradient descent could be applied to find the $\theta$ that minimized $L(\theta)$.

Now, what happens if each $f \in \mathcal{H}$ is not parameterized by a real-valued vector $\theta$. For example, what if $\mathcal{H}$ is a space of [decision trees](https://en.wikipedia.org/wiki/Decision_tree)? Now, we seek to minimize the function 

$$L(f) := \frac{1}{n} \sum_{i=1}^n \ell(y_i, f(x_i))$$

that is, we seek 

$$\hat{f} := \text{arg min}_{f \in \mathcal{H}} L(f)$$

Can we still apply gradient descent?

In fact, it turns out that we can indeed derive a gradient descent-like algorithm called **gradient boosting**. At a high level, this algorithm is analogous to gradient descent in the following way: we start with an initial model $f_0$ and iteratively update $f_0$ by taking small steps that improve $L(f)$. That is, at iteration $t$, we update our current estimate $f_t$ by

$$f_t := f_{t-1} + \alpha h_t$$

where $h_t$ is a function in $\mathcal{H}$ that represents a function that improves $L(f)$ the most. That is, $h_t$ is analogous to the gradient, $\nabla L(\theta)$, from traditional gradient descent. 

Notice that instead of performing gradient descent in a real-valued coordinate vector space over parameters $\theta \in \mathbb{R}^p$, we are now performing a gradient-descent-like process within the function space $\mathcal{H}$ itself!

The question we now must answer is, how do we derive this "gradient function" $h_t$? 

**Here's the crucial insight:** if we want a function $h_t$ that acts like the gradient of $L(f_{t-1})$, then for all $x_i$, it should hold that

$$h_t(x_i) = \frac{\partial \ell(f(x_i))}{\partial f(x_i)}$$

For simplicity of notation, let

$$r_i := \frac{\partial \ell(y_i, f(x_i)}{\partial f(x_i)}$$

Then, we simply need a function $h_t$ for which $h_t(x_i) = r_i$. We can find such a function by training a model to fit the pairs $(x_i, r_i)$! Once we fit this model, we can update our current estimate $f_{t-1}$ via

$$f_t := f_{t-1} + h_t$$

At the end of $T$ iterations, our full model $\hat{f}$ will have the form

$$\hat{f}(x) = \sum_{t=1}^T \alpha h_t(x)$$

Notice that $f(x)$ is actually an ensemble of models $h_1, \dots, h_T$! As we will show in the next section, this process can be viewed as a boosting algorithm!


Viewing gradient boosting as a boosting process
-----------------------------------------------



Calculating the pseudo-residuals for common loss functions
----------------------------------------------------------
