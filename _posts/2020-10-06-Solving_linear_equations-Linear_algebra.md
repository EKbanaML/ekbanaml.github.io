---
title: "Solving Linear Equations"
excerpt_separator: "An introduction to using matrices to solve linear equations."
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
## 2.3 Elimination using matrices  

In linear algebra systems of linear equations are viewed as matrix-vector operations.

### Identity Matrix

It is a matrix I that when multiplied with any other matrix gives the same matrix.

$$AI = IA = A$$

### Elimination matrix

Each elimination step can be viewed as a matrix.  

$$E_{21}.A = U_{21}$$  

$$\begin{bmatrix}1 & 0 & 0 \\ -1 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$.$$\begin{bmatrix}1 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$ = $$\begin{bmatrix}1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$

All these steps can be combined to form a single elimination matrix.  
 
$$E_{32}.E_{31}.E_{21} = E$$  

$$E.A = U$$

Where U is the upper triangular form.

$$A = E^{-1}U$$

$$A = LU$$

Where L is the inverse of E. This allows us to get A back from U.

### Permutation matrix  
Each row exchange step can be viewed as a matrix.  
A Permutation matrix $P_{ij}$ lets us exchange the row i with row j. 

$$P_{31}= \begin{bmatrix}0&0&1\\0&1&0\\1&0&0\end{bmatrix}$$

$$A = \begin{bmatrix}0&2&3\\8&6&7\\5&9&4\end{bmatrix}$$  

$$P.A = \begin{bmatrix}5&9&4\\8&6&7\\0&2&3\end{bmatrix}$$   

### Augmented Matrix 
All the above eliminations steps and permutation steps have to applied on the right side(B) also.

$$AX = B$$

So, to make it simpler we can add B as a column to A separated by ':'.  
This is known as an augmented matrix.   

$$3x + 4y + 5z = 22$$  

$$2x + 5y + 7z = 23$$  

$$4x + 3y + 6z = 24$$  

$$\begin{bmatrix}3&4&5\\2&5&7\\4&3&6\end{bmatrix}.\begin{bmatrix}x\\y\\z\end{bmatrix}=\begin{bmatrix}22\\23\\24\end{bmatrix}$$    

$$A\;B = \begin{bmatrix}3&4&5&:22\\2&5&7&:23\\4&3&6&:24\end{bmatrix}$$

$$E = \begin{bmatrix}1&0&0\\0&1&0\\0&1&1\end{bmatrix}.\begin{bmatrix}1&0&0\\0&1&0\\ \frac{-4}{3}&0&1\end{bmatrix}.\begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\0&0&1\end{bmatrix}$$  

$$E = \begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\-2&1&1\end{bmatrix}$$

$$E.(A\;B)= \begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\-2&1&1\end{bmatrix}.\begin{bmatrix}3&4&5&:22\\2&5&7&:23\\4&3&6&:24\end{bmatrix}$$

$$E.(A\;B)=\begin{bmatrix}3&4&5&:22\\0& \frac{7}{3}& \frac{11}{3}&: \frac{25}{3}\\0&0&3&:3\end{bmatrix}$$

Now this can be solved with back substitution.


### Rules for matrix multiplication
### Associative law

$$A(BC) = (AB)C$$
### Commutative law

$$AB \neq BA$$  


## 2.4 Rules for Matrix Operations

Matrices $$A$$ with $$n$$ columns multiply matrices $$B$$ with $$n$$ rows :  $$A_{mxn}.B_{nxp} = C_{mxp}$$ 

### First way
Take the dot product of each row of A with each column of B:  

Here, $$C_{ij} = A_{i^{th} row}.B_{i^{th} col}$$.  

For example :  

$$\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}$$

$$\begin{bmatrix} b_{11} & b_{12} \\ b_{21} & b_{22} \end{bmatrix}$$ 

=

$$\begin{bmatrix} a_{11}b_{11}+a_{12}b_{21} & a_{11}b_{12}+a_{12}b_{22} \\ a_{21}b_{11}+a_{22}b_{21} & a_{21}b_{12}+a_{22}b_{22} \end{bmatrix}$$  

### Second way
Matrix A times every column of B:  

$$\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}$$

$$\begin{bmatrix} b_{11} & b_{12} \\ b_{21} & b_{22} \end{bmatrix}$$  

=

$$\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}$$$$\begin{bmatrix} b_{11} \\ b_{21} \end{bmatrix}$$

and

$$\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}$$$$\begin{bmatrix} b_{12} \\ b_{22} \end{bmatrix}$$ 

=

$$\begin{bmatrix} a_{11}b_{11}+a_{12}b_{21} && a_{11}b_{12}+a_{12}b_{22} \\ a_{21}b_{11}+a_{22}b_{21} && a_{21}b_{12}+a_{22}b_{22} \end{bmatrix}$$


### Third way
Every row of A times matrix B : Each row of A multiplies the whole matrix B. The result
is a row of AB. 

$$A.B = \begin{bmatrix}A_{row1}.B &.&.\\ A_{row2}.B&.&.\\A_{row3}.B&.&.\end{bmatrix}$$

### fourth way
Multiply columns 1 to n of A times rows 1 to n of B:  

$$\begin{bmatrix} a_{11} \\ a_{21} \end{bmatrix}$$

$$\begin{bmatrix} b_{11} & b_{12} \end{bmatrix}$$

+

$$\begin{bmatrix} a_{12} \\ a_{22} \end{bmatrix}$$

$$\begin{bmatrix} b_{21} & b_{22} \end{bmatrix}$$
    
= 

$$\begin{bmatrix} a_{11}b_{11} && a_{11}b_{12} \\ a_{21}b_{11} && a_{21}b_{12} \end{bmatrix}$$ 

+

$$\begin{bmatrix} a_{12}b_{21} && a_{12}b_{22} \\ a_{22}b_{21} && a_{22}b_{22} \end{bmatrix}$$ 

=

$$\begin{bmatrix} a_{11}b_{11}+a_{12}b_{21} && a_{11}b_{12}+a_{12}b_{22} \\ a_{21}b_{11}+a_{22}b_{21} && a_{21}b_{12}+a_{22}b_{22} \end{bmatrix}$$

## 2.5 Inverse Matrix

- Not all matrices have inverses.

- $$A^{-1} A = AA^{-1} = I$$

#### Invertible and non-invertible matrices.

If the inverse of a matrix does not exist it is called a non-invertible matrix. 

### Testing Inverse

- If matrix is `diagonally dominant`, it has inverse.

- $A^{-1}$ exists iff elimination produces $n$ `pivot points`. That is no diagonal element should be zero.

- Matrix A can not have two different inverse which means x = $$A^{-1}B$$ has one and only solution.

- If $Ax = 0$, we cannot divide by 0 to create $A^{-1}$.

- $A^{-1}$ exists if determinant of $A \neq 0$.

### Diagonally dominant 
It the diagonal element is greater than the sum of other elements for all rows. 

### Singular matrix 
It is a square matrix which does not have an inverse.  
  
### The inverse of a product

If A and B are invertible. Then AB  is also invertible.

$$A^{-1} B^{-1} = (AB)^{-1}$$  

$$A^{-1} B^{-1} C^{-1} = (ABC)^{-1}$$

### Calculating $$A^{-1}$$ using Gauss-Jordan

The Gauss-Jordan method computes $$A^{-1}$$ by solving all n equations together.

$$ K:I= \begin{bmatrix}2 & -1 & 0 & 1 & 0 & 0\\-1 & 2 & -1 & 0 & 1 & 0 \\ 0 & -1 & 2 & 0 & 0 & 1 \end{bmatrix}$$

#### Perform Row Elimination

$$= \begin{bmatrix}2 & -1 & 0 & 1 & 0 & 0\\0 & \frac{3}{2} & -1 & \frac{1}{2} & 1 & 0\\0 & 0 & \frac{4}{3} & \frac{1}{3} & \frac{2}{3} & 1\end{bmatrix}$$

#### Zero above pivots

$$= \begin{bmatrix}2 & 0 & 0 & \frac{3}{2} & 1 & \frac{1}{2}\\0 & \frac{3}{2} & 0 & \frac{3}{4} & \frac{3}{2} & \frac{3}{4} \\ 0 & 0 & \frac{4}{3} & \frac{1}{3} & \frac{2}{3} & 1\end{bmatrix}$$

#### Divide row by diagonal elements

$$= \begin{bmatrix}1 & 0 & 0 & \frac{3}{4} & \frac{1}{2} & \frac{1}{4}\\0 & 1 & 0 & \frac{1}{2} & 1 & \frac{1}{2} \\ 0 & 0 & 1 & \frac{1}{4} & \frac{1}{2} & \frac{3}{4}\end{bmatrix}$$ = $$I:K^{-1}$$  

- $K$ is symmetric then $K^{-1}$ is also symmetric.  
    
## 2.6 Elimination = Factorization : LU

- $EA = U$, U is an upper triangular matrix.
- $E^{-1} = L$
- Then the original matrix $A$ can be recovered by $A = LU$
- Elimination on $AX = B$ gives $UX = C$. 

## Transposes and permutations

### Transpose

Transposing a matrix means to exchange its rows and columns.

$$A = \begin{bmatrix}1&2\\3&4\\5&6\end{bmatrix}$$  

$$A^{T} = \begin{bmatrix}1&3&5\\2&4&6\end{bmatrix}$$  

### some rules for transpose

$$(A+B)^{T} = A^{T} + B^{T}$$

$$(A.B)^{T} =  B^{T}.A^{T}$$

$$(A^{-1})^{T} =  (A^{T})^{-1}$$

If $$A$$ is invertible $$A^{T}$$ is also invertible.

### Symmetric matrices

$S$ is a symmetric matrix if $S = S^{T}$
