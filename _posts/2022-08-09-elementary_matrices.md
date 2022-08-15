---
title: 'Elementary matrices and the general linear group'
date: 2022-08-09
permalink: /posts/elementary_matrices/
tags:
  - tutorial
  - mathematics
  - linear algebra
---

_THIS POST IS CURRENTLY UNDER CONSTRUCTION_

Introduction
------------

In a [previous blog post](https://mbernste.github.io/posts/systems_linear_equations/), we showed how systems of linear equations can be represented as a matrix equation. For example, the system of linear equations,

$$\begin{align*}a_{1,1}x_1 + a_{1,2}x_2 + a_{1,3}x_3 &= b_1 \\ a_{2,1}x_1 + a_{2,2}x_2 + a_{2,3}x_3 &= b_2 \\ a_{3,1}x_1 + a_{3,2}x_2 + a_{3,3}x_3 &= b_3 \end{align*}$$

can be represented succinctly as

$$\boldsymbol{Ax} = \boldsymbol{b}$$

where $\boldsymbol{A}$ is the matrix of coefficients $a_{1,1}, a_{1,2}, \dots, a_{3,3}$ and $\boldsymbol{b}$ is the matrix of coefficients of $b_1, b_2,$ and $b_3$.  Furthermore, we noted that this system will have exactly one solution if $\boldsymbol{A}$ is an [invertible matrix](https://mbernste.github.io/posts/inverse_matrices/). 

In this post, we will discuss how one can solve for this exact solution using a series of operations of the system that involve a particular class of invertible matrices called the **elementary matrices**. We will show that any invertible matrix can be converted to another invertible matrix by performing some sequence of matrix multiplications with elementary matrices. In fact, this reveals a deep mathematical structure regarding invertible matrices: they form a **group**. That is, you can always go from any invertible matrix to another via a series of multipliciations by elementary row operations!

Elementary row operations
--------------------------

Before digging into matrices, let's first discuss how one can go about solving a system of linear equations. Say we have a system with three equations and three variables:

$$\begin{align*}a_{1,1}x_1 + a_{1,2}x_2 + a_{1,3}x_3 &= b_1 \\ a_{2,1}x_1 + a_{2,2}x_2 + a_{2,3}x_3 &= b_2 \\ a_{3,1}x_1 + a_{3,2}x_2 + a_{3,3}x_3 &= b_3 \end{align*}$$

To solve such a system, our goal is perform simple algebraic operations on these equations until we convert the system to one with the following form:

$$\begin{align*}x_1 &= c_1 \\ x_2 &= c_2 \\ x_3 &= c_3 \end{align*}$$

where $c_1, c_2$, and $c_3$ are the solutions to the system -- that is, they are the values we can assign to $x_1, x_2$, and $x_3$ so that all of the equations in the system are valid.  

Now, what kinds of algebraic operations can we perform on the equations of the system to solve it? There are three main categories, called **elementary row operations** (we'll see soon, why they have this name):

1. **Scalar multiplication**: Simply multiply both sides of one of the equations by a scalar. For example, 
2. **Row swap**: You can move one equation above or below another. Note, the order the equations are written is irrevalent to the solution, so swapping rows is really just changing how we organize the formulas. Nonetheless, this organization will be important as we demonstrate how elementary matrices can be used to solve the system.
3. **Row sum**: Add a multiple of one equation to another. Note, this is a perfectly valid operation because, if the equality truly holds, then this equations to simply adding the same quantity to both sides of a given equation.

Let's use these operations to solve the following system:

$$\begin{align*}-x_1 - 2 x_2 + x_3 &= -3 \\  3 x_2 &= 3 \\ 2 x_1 + 4 x_2  &= 10\end{align*}$$

1. First, we _row swap_ the first and third equations:

$$\begin{align*}2 x_1 + 4 x_2 &= 10  \\ 3 x_2  &= 3 \\ -x_1 - 2 x_2 + x_3 &= -3\end{align*}$$

2. Next, let's perform _scalar multiplication_ and multiply the first equation by 1/2:

$$\begin{align*}x_1 + 2 x_2 &= 5  \\ 3 x_2 &= 3 \\ -x_1 - 2 x_2 + x_3 &= -3\end{align*}$$

3. Next, let's perform a _row sum_ and add the first row to the third:

$$\begin{align*}x_1 + 2 x_2 &= 5  \\  3 x_2  &= 3 \\ x_3 &= 2\end{align*}$$

4. Next, let's perform _scalar multiplication_ and multiply the second equation by 1/3:

$$\begin{align*}x_1 + 2 x_2 &= 5  \\ x_2 &= 1 \\  x_3 &= 2\end{align*}$$

5. Finally, let's perform a _row sum_ and add -2 multiplied by the second row to the first:

$$\begin{align*}x_1 &= 3  \\ x_2 &= 1 \\ x_3 &= 2\end{align*}$$

And there we go, we've solved the system using these elementary row operations.

Elementary row operations in matrix notation
--------------------------------------------

Recall, we can represent a system of linear equations as a [matrix equation](https://mbernste.github.io/posts/systems_linear_equations/), we showed how systems of linear equations can be represented as a matrix equation.

The linear system that we just solved can be written as:

$$\begin{bmatrix}-1 & -2 & 1 \\ 0 & 3 & 0 \\ 2 & 4 & 0 \end{bmatrix}\begin{bmatrix}x_1 \\ x_2 \\ x_3\end{bmatrix} = \begin{bmatrix}-3 \\ 3 \\ 10\end{bmatrix}$$

When solving the system using the elementary row operations, we needn't write out all of the equations. Really, all we need to do is keep track of how $\boldsymbol{A}$ and $$\boldsymbol{b}$$ are being transformed upon each iteration. For ease of notation, we can join $\boldsymbol{A}$ and $\boldsymbol{b}$ into a single matrix, called an **augmented matrix**. In our example, this augmented matrix would look like:

$$\begin{bmatrix}-1 & -2 & 1 & -3 \\ 0 & 3 & 0 & 3 \\ 2 & 4 & 0 & 10 \end{bmatrix}$$

In the augmented matrix, the final column stores $\boldsymbol{b}$ and all of the previous columns store the columns of $\boldsymbol{A}$. Our execution of the row operations can now operate only on this augmented matrix as follows:

1. _Row swap_: swap the first and third equations:

$$\begin{bmatrix}2 & 4 & 0 & 10 \\ 0 & 3 & 0 & 3 \\ -1 & -2 & 1 & -3  \end{bmatrix}$$

2. _Scalar multiplication_: Multiply the first equation by 1/2:

$$\begin{bmatrix}1 & 2 & 0 & 5 \\ 0 & 3 & 0 & 3 \\ -1 & -2 & 1 & -3  \end{bmatrix}$$

3. _Row sum_: add the first row to the third:

$$\begin{bmatrix}1 & 2 & 0 & 5 \\ 0 & 3 & 0 & 3 \\ 0 & 0 & 1 & 2  \end{bmatrix}$$

4. _Scalar multiplication_: Multiply the second equation by 1/3:

$$\begin{bmatrix}1 & 2 & 0 & 5 \\ 0 & 1 & 0 & 1 \\ 0 & 0 & 1 & 2  \end{bmatrix}$$

5. _Row sum_ and add -2 multiplied by the second row to the first:

$$\begin{bmatrix}1 & 0 & 0 & 3 \\ 0 & 1 & 0 & 1 \\ 0 & 0 & 1 & 2  \end{bmatrix}$$

Now, let's re-write the augmented matrix as a matrix equation:

$$\begin{bmatrix}1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}\begin{bmatrix}x_1 \\ x_2 \\ x_3\end{bmatrix} = \begin{bmatrix}3 \\ 1 \\ 2\end{bmatrix}$$

Note that $$\boldsymbol{A}$$ has been _transformed_ to the identity matrix $$\boldsymbol{I}$$. This will be a key observation as we move into the next section.

Elementary matrices
-------------------

Notice on each elementary row operation, we transformed the matrix $\boldsymbol{A}$ using a series of steps until it became the identity matrix $\boldsymbol{I}$. In fact, each of these elementary row operations can be represented as a matrix that is operating on $\boldsymbol{I}$. Such a matrix that represents an elementary row operation is called an **elementary matrix**. 

To demonstrate, that our elementary row operations can be performed using matrix multiplication, let's look back at our example. We start with the matrix 

$$\boldsymbol{A} := $$

Then, first we _row swap_ the first and third equations:



Then perform _scalar multiplication_ and multiply the first equation by 1/2:

Then perform a _row sum_ and add the first row to the third:

Then perform _scalar multiplication_ and multiply the second equation by 1/3:

Then perform a _row sum_ and add -2 multiplied by the second row to the first:


More generally, the elementary matrices take the following form: An elementary matrix that represents a scalar multiplication by $c$ can be written as:

An elementary matrix that represents a row swap can be written as:

An elementary matrix that represents a row sum can be written as:

This may seem cumbersome, but in the next section we'll show something quite elegant that emerges from using elementary matrices to represent elementary row operations: invertible matrices form a [mathematical group](https://en.wikipedia.org/wiki/Group_(mathematics))!

The general linear group
------------------------

Note that the elementary matrices all all invertible. The inverse to an elementary matrix representing scalar multiplication by a constant $c$ is simply the elementary matrix that scales by $\frac{1}{c}$. The inverse to an elementary matrix representing a row swap is simply the elementary matrix that swaps the rows back to their original configuration. The inverse to an elementary matrix representing a row sum is simply the elementary matrix that represents a row sum but performs the corresponding subtraction.

From this fact, we can see that we can go from any invertible matrix to another by multiplying the matrix with some series of elementary matrices. That is, if we have two invertible matrices $\boldsymbol{A}$ and $\boldsymbol{B}$, then there exists a sequence of elementary matrices $\boldsymbol{E}_1, \dots, \boldsymbol{E}_n$ such that 

$\boldsymbol{B} = $\boldsymbol{A}\boldsymbol{E}_1, \dots, \boldsymbol{E}_n$

How do we know this is true?