---
title: "Introduction to Vectors"
excerpt_separator: "A brief introduction to vectors and linear algebra for anyone who wants to start out the journey on linear algebra."
last_modified_at: 2020-10-02T16:20:02-05:00
categories:
  - Linear Algebra
tags:
  - vectors
header:
  image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
  caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
author: rizzurohit
---

# Introduction to Vectors

## 1.1 Linear combination
### Vectors  
A vector is an object that has both a magnitude and a direction.  

But for our purpose, consider vectors as groups of numbers. 

Let's say I am 6ft tall and 70 kgs this information could be written as a vector.


$$\vec R = \begin{bmatrix}6\\70\end{bmatrix}$$

Graphically,

![2d vetor representation in graph]({{ site.url }}{{ site.baseurl }}/assets/images/linear_algebra/Introduction_to_vectors/2d_vetor_representation_in_graph.png)

If I added my age to the vector then it would be plotted in a 3D space.

So, ***Vectors can be thought of as points in a n-dimensional space. These points are represented by a series of numbers which are the magnitudes of the vector to each axis of the n-dimensions.***  

Like, how 6 is my magnitude in the height axis and 70 is my magnitude in the weight axis.  

### Vector Operations
These Vectors can have some operations done on them.  
#### Addition     

$$\vec C = \begin{bmatrix}0.5\\2\end{bmatrix}$$

$$\vec R + \vec C = \begin{bmatrix}6+0.5\\70+2\end{bmatrix}$$

$$\vec R + \vec C = \begin{bmatrix}6.5\\72\end{bmatrix}$$

It is like putting one vector at tip of another and seeing where it lands.  
![2D vector addition]({{ site.url }}{{ site.baseurl }}/assets/images/linear_algebra/Introduction_to_vectors/2d_vector_addition.png)  

To add 2 vectors, you add the corresponding components.  

Subtracting 2 Vectors is just adding one vector and the negative of the other vector.  
The negative of a vector is a vector with the same magnitude but opposite direction.

$$\vec R - \vec C = \begin{bmatrix}6-0.5\\70-2\end{bmatrix}$$


### Scalars
Let's suppose a vector of house price and interest rate. And imagine if the prices and rates doubled. We would denote it as 

$$\vec A = \begin{bmatrix}4\\5\end{bmatrix},  C = 2$$ (4 crores, 5%)  
$$C.\vec A = \begin{bmatrix}8\\10\end{bmatrix}$$ 

Here, C is a scalar.  
Scalars are numbers which scale a vector when multiplied with a vector. A scaled vector has the same direction or the opposite direction depending on whether it is scaled by a positive or negative scalar.  

### Linear Combination   
In mathematics, a linear combination is an expression constructed from a set of terms by multiplying each term by a constant and adding the results.  
In the case of vectors
Let $\vec A$ and $\vec B$ be 2 vectors and $C$ and $D$ be 2 scalars.  
Then the linear combination is given by  

$$C \vec A + D \vec B$$  

Now, the combination can map to any point on the x y plane.  

Let's suppose we have

$$\vec A = \begin{bmatrix}60\\70\end{bmatrix}$$

$$\vec B = \begin{bmatrix}100\\200\end{bmatrix}$$

and using this we want to map to  

$$\vec Z = \begin{bmatrix}70\\80\end{bmatrix}$$ 

Then with C = 1.68 and D = -0.44 we can map to our point.  

$$C \vec A + D \vec B$$  

Like, this we can choose C and D such that we can map to any point in the xy plane.

### Linear Combination of 3D Vectors  

$\vec V$, $\vec W$ and $\vec U$ are 3 vectors in 3 dimensions.

$C,D,E$ are 3 scalars

The linear combination is  

$$C\vec V + D\vec W+ E\vec U$$  

Similar to 2D in 3D, combination of 2 vectors can map to any point on a plane. But this time it is not just the xy plane. This can be any plane.   

A combination of of 3 vectors can map to any point in the 3D space.   
But, if the third vector also lies on the plane that the combination of the first two vectors fill then it is not possible to fill the space. This is known as ***linearly dependent***.  

## 1.2 Lengths and Dot products  
### Dot product

$$\vec V.\vec W = V1*W1+V2*W2+V3*W3…$$

### Length of a vector

To find the length of a vector first we need to calculate the Euclidean distance of the point from the origin.

$$Length ||V|| = \sqrt{(v1.v1+v2.v2+v3.v3...)}$$

$$Length ||V|| = \sqrt{\vec V. \vec V}$$

$$\vec V.\vec V  = ||V||^2$$

$$Length ||V|| = \sqrt{||V||^2}$$

### Unit vector

A unit vector is a vector whose length is 1.
Example can be:  

$$V = \begin{bmatrix}1/\sqrt{2} \\ 1/ \sqrt{2}\end{bmatrix}$$

$$\vec V.\vec V = ½+½ = 1 $$  

$$\||V|| = \sqrt{1} =1 $$

### Basis vectors 
A set $B$ of vectors is called basis, if elements are not linearly dependent, and every vector in the space is a linear combination of elements of B.

$$i = \begin{bmatrix}1\\0\end{bmatrix}$$ 

$$j = \begin{bmatrix}0\\1\end{bmatrix}$$  

$$\vec V = \begin{bmatrix}49\\50\end{bmatrix}$$ 

$$\vec V = 49\begin{bmatrix}1\\0\end{bmatrix} + 50\begin{bmatrix}0\\1\end{bmatrix}$$  

$$\vec V = 49i + 50j$$


Thus any vector can be represented as a linear combination of $i$ and $j$. So, $i$ and $j$ can be our basis for $R^2$.  


If $\vec V$ is divided by its length then the result is a unit vector in the same direction.     

A unit vector $\vec U$ can also be denoted as  

$$\vec U = \begin{bmatrix}cos(\theta)\\sin(\theta)\end{bmatrix}$$  

Where $\theta$ is the angle made by the vector with $i$.  
Any other vector $\vec V$ with length $r$.  

$$\vec V = \begin{bmatrix}r.cos(\theta)\\r.sin(\theta)\end{bmatrix}$$  

### Angles between vectors

The dot product is $\vec V$·$\vec W$ = 0 when $\vec V$ is perpendicular to $\vec W$.   

The dot product of any vector with a zero vector is a zero vector. Hence zero vector is perpendicular to every other vector.  

### Cosine formula:

$$cos(\theta) = \vec V.\vec W / ||V||*||W||$$

Which also gives another definition for dot product

$$\vec V.\vec W = ||V||*||W|| * cos(\theta)$$

Where theta is the angle between the vectors.  

## 1.3 Matrices  
Matrices are 2 dimensional arrangements of number.  
They can be viewed as a group of column vectors or row vectors.  

### Multiplying matrix and vectors  
When multiplying a matrix with a vector, we can perform linear combinations of the vector components with the corresponding columns of the matrix.  

$$\vec X = \begin{bmatrix}x1 \\ x2 \\ x3\end{bmatrix}$$ 

$$A = \begin{bmatrix}a & b & c \\ d & e & f \\ g & h & i\end{bmatrix}$$

$$A.\vec X = x1.\begin{bmatrix}a \\ d \\ g \end{bmatrix} + x2.\begin{bmatrix}b \\ e\\ h \end{bmatrix} + x3.\begin{bmatrix}c \\ f \\ i\end{bmatrix}$$

This gives an important insight. Unless the columns of the matrices are linearly dependent, a matrix can transform a vector to any vector in the space.

Matrix-vector multiplication can also be the dot product of the vector with each row of the matrix. 

$$A = \begin{bmatrix}a & b & c & .(x1,x2,x3) \\ d & e & f & .(x1,x2,x3)\\ g & h & i& .(x1,x2,x3)\end{bmatrix}$$


### Inverse matrix 
In linear algebra, an n-by-n square matrix A is called invertible (also nonsingular or nondegenerate) if there exists an n-by-n square matrix B such that  

$$A.B = B.A = I$$ 

where I denotes the n-by-n identity matrix and the multiplication used is ordinary matrix multiplication. If this is the case, then the matrix B is uniquely determined by A and is called the inverse of A, denoted by $A^{-1}$.  

### Singular Matrix
It is a square matrix that doesn't have an inverse. 

### Dependence
Dependence is when one vector denoted as $\vec A$ can be produced by the linear combination of the other vectors in the set.  

In other words,  
A set of vetors ${ \vec V_{1}, \vec V_{2},... , \vec V_{n} }$  are said to be **Linearly dependent** if  

$$C_{1} \vec V_{1} + C_{2} \vec V_{2}+...  + C_{n} \vec V_{n} = 0$$  

When no $C= { C_{1},C_{2},... ,C_{n}} = 0$

# Solving linear equations
Linear equations are equations that describe the characteristic of a straight element in an ND space.   
Two lines intersect at a point.  
Similarly planes, intersect at a points or line.     

## 2.1 Vectors and linear equations


$$x-2y  = 1$$  

$$3x + 2y  = 11$$

With reference to Gilbert Strang's book "Introduction to Linear Algebra". In this chapter the author paints 2 pictures to to depict linear equations.

### Row picture
The row picture shows two lines meeting at a single point. The solution for x and y comes by figuring out where these lines meet. This can be done by finding y in terms of x in one equation. then substituting the y in the other equation.  

![Row picture showing intersection of two lines]({{ site.url }}{{ site.baseurl }}/assets/images/linear_algebra/Introduction_to_vectors/row_picture_intersection_two_lines.png)

This picture is good for 2 or 3 dimensions but it becomes impossible to visualize in the higher dimensions.  

### Column picture

$$x\begin{bmatrix}1\\3\end{bmatrix} + y \begin{bmatrix}-2\\2\end{bmatrix}  = \begin{bmatrix}1\\11\end{bmatrix}$$

Now in this picture a system of linear equations can be viewed as a linear combination of vectors. 

`My own preference is to combine
column vectors. It is a lot easier to see a combination of four vectors in four-dimensional
space, than to visualize how four hyperplanes might possibly meet at a point. (Even one
hyperplane is hard enough. . . )`

-Gilbert Strang

### Matrix form of equations  
A is a matrix of the coefficients of the equations.
X is a vector of the variables of the equations.  
 
$$A\vec X = B$$

$$\begin{bmatrix}a & b & c \\ d & e & f \\ g & h & i\end{bmatrix}\begin{bmatrix}x \\ y \\ z\end{bmatrix} = \begin{bmatrix}b1 \\ b2 \\ b3\end{bmatrix}$$

## 2.2 Idea of Elimination  
The aim of elimination is to get upper triangular form.  
We subtract rows from other rows such that the such that some specific coefficients are 0. 

### What is a pivot?
It is the first non zero number in the row that does the elimination. In the example below, 4 is the pivot for row 1.


### What is a multiplier?
The number to be eliminated, divided by the pivot, serves as the multiplier. In the example below, 3/4 is the multiplier.  

We multiply row 1 by the multiplier and subtract it from row 2 to get 0 in the position below the pivot.  


![Upper triangular form of equation]({{ site.url }}{{ site.baseurl }}/assets/images/linear_algebra/Introduction_to_vectors/upper_triangular_form_of_equation.png)  

But there can be situations when there are infinitely many or no solutions. We'll call situations as failure conditions.

### Failure conditions

There is no solution when the lines are parallel. If the lines never meet how can there be a point of intersection.  

There are $\infty$ solution if the lines are collinear.  

We must remember that 0 can never be a pivot.  

We need to exchange rows if a pivot is zero if such row is available.  

The best situation is when n no of pivots exists for n equations.  

### Change to U matrix.
Use the first equation to make the first column all zeros under the first element.  

$$\begin{bmatrix}1 & 1 & 0 \\ 1 & 2 & 1 & multiply row1 by (1/1) and subtract\\ 0 & 1 & 2 & multiply row1 by (0/1) and subtract\end{bmatrix}$$ 

$$ = \begin{bmatrix}1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$

Use the second equation to make the second column all zero under the second diagonal element.

$$ = \begin{bmatrix}1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 & multiply row2 by (1/1) and subtract\end{bmatrix}$$  

$$ = \begin{bmatrix}1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix}$$
And so on.
