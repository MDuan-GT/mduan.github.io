---
title: 'Demystifying determinants'
date: 2021-11-28
permalink: /posts/determinants/
tags:
  - tutorial
  - mathematics
  - linear algebra
---


_THIS POST IS CURRENTLY UNDER CONSTRUCTION_

Introduction
------------

In introductory linear algebra, one is taught that the the **determinant**, $\text{Det}$, is a function that maps square [matrices](https://mbernste.github.io/posts/matrices/) to real numbers,

$$\text{Det} : \mathbb{R}^{m \times m} \rightarrow \mathbb{R}$$ 

for which the absolute value of a matrix's determinant is the area of the parallelepided formed by its columns. 

While conceptually, this is fairly straightforward, the analytical formula for the determinant is quite confusing. Specifically, the determinant of an $m \times m$ matrix is defined as:

$$\text{Det}(\boldsymbol{A}) := \begin{cases} a_{1,1}a_{2,2} - a_{1,2}a_{2,1} & \text{if $m = 2$} \\ \sum_{i=1}^m (-1)^{i+1} a_{i,1} \text{Det}(\boldsymbol{A}_{-1,-i}) & \text{if $m > 2$}\end{cases}$$

where $\boldsymbol{A}_{-1, -i}$ denotes the matrix formed by deleting the first row and $i$th column of $\boldsymbol{A}$. Note that this is a [recursive definition](https://en.wikipedia.org/wiki/Recursive_definition) where the base case is a $2 \times 2$ matrix. 

When one is usually first taught determinants, they are supposed to take it as a given that this formula calculates the volume of an $m$-dimensional parallelepided; however, if you're like me, this is not at all obvious. How on earth does this formula calculate volume? Moreover, why is it recursive?

In this post, I am going to attempt to demystify this definition. We will start with the base case of a $2 \times 2$ matrix, verify that it indeed computes the volume of the parallelogram formed by the columns of the matrix, and then move on to the determinant for larger matrices. 

$2 \times 2$ matrices
---------------------

Let's first only look at the $m = 2$ case and verify that this equation computes the area of the parallelogram formed by the matrix's columns. Let's say we have a matrix

$$\boldsymbol{A} := \begin{bmatrix}a & b \\ c & d\end{bmatrix}$$

Then we see that the area can be obtained by computing the area of the rectangle that encompasses the parallelogram and subtracting the areas of the triangles around it:

<center><img src="https://raw.githubusercontent.com/mbernste/mbernste.github.io/master/images/TwoByTwoDeterminant.png" alt="drawing" width="700"/></center>

Simplifying the equation above we get

$$\text{Det}(\boldsymbol{A}) = ad - bc$$

which is exactly the definition for the $2 \times 2$ determinant.  So far so good. 

Defining the determinant for $m \times m$ matrices: abstracting the notion of geometric volume
----------------------------------------------------------------------------------------------

Moving on to $m > 2$, the definition of the determinant is

$$\text{Det}(\boldsymbol{A}) := \sum_{i=1}^m (-1)^{i+1} a_{i,1} \text{Det}(\boldsymbol{A}_{-1,-i})$$

Before understanding this equation, we must first ask ourselves what we even mean by "volume" in $m$-dimensional space. In fact, it is through answering this very question that will arrive at the equation above. Specifically, we will formulate a set of three axioms that attempt to capture our notion of "geometric volume" in a very abstract way. Then, we will show that the only formula that satisfies these axioms is the formula for the determinant shown above! 

These axioms are as follows:

**1. The determinant of the identity matrix is one**  

The first axiom states that

$$\text{Det}(\boldsymbol{I}) := 1$$

Why do we want this to be an axiom? First, we note that the parallelepided formed by the columns of the identity matrix, $\boldsymbol{I}$, is a hypercube in $m$-dimensional space:

<center><img src="https://raw.githubusercontent.com/mbernste/mbernste.github.io/master/images/DeterminantIdentityMatrix.png" alt="drawing" width="400"/></center>

We would like our notion of "geometric volume" to match our common intuition that the volume of a cube is simply the product of the sides of the cube. In this case, they're all of length one so the volume, and thus the determinant, should be one.


**2. If two columns of a matrix are equal, then its determinant is zero**  

For a given matrix $\boldsymbol{A}$, if any two columns $\boldsymbol{a}_{*,i}$ and $\boldsymbol{a}_{*,j}$ are equal, then determinant of $\boldsymbol{A}$ should be zero.

Why do we want this to be an axiom? We first note that if two columns of a matrix are equal, then the parallelapipde formed by their columns is flat. For example, here's a depiction of a parallelepided formed by the columns of a $3 \times 3$ matrix with two columns that are equal:

<center><img src="https://raw.githubusercontent.com/mbernste/mbernste.github.io/master/images/FlatParallelepiped.png" alt="drawing" width="400"/></center>

We see that the parallelepided is flat and lies within a hyperplane.

We would like our notion of "geometric volume" to match our common intuition that the volume of a flat object is zero. Thus, when any two columns of a matrix are equal, we would like the determinant to be zero.

**2. The determinant of a matrix is linear with respect to each column vector**



Note, for the remainder of this blog post, we will often represent the determinant of a matrix as a function with either a single matrix argument, $\text{Det}(\boldsymbol{A})$, or with multiple vector arguments $\text{Det}(\boldsymbol{a}_{\*,1}, \dots, \boldsymbol{a}_{\*,1})$ where $\boldsymbol{a}_{\*,1}, \dots, \boldsymbol{a}_{\*,1}$ are the columns of $\boldsymbol{A}$.





Deriving the formula for a determinant
--------------------------------------

What is meant by "signed volume"?
---------------------------------
