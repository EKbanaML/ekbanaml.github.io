---
title: "Solving Linear Equations"
excerpt_separator: "An introduction to using matrices to solve linear equations."
last_modified_at: 2020-10-06T16:20:02-05:00
categories:
  - Linear Algebra
tags:
  - matrices
header:
  image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
  caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
author: rizzurohit
---
## 2.3 Elimination using matrices  

In linear algebra systems of linear equations are viewed as matrix-vector operations.


### Identity Matrix

An identity matrix $I$ is $n \times n$ square matrix where the elements of the  main diagonal are $1$ and rest are $0$.

Example:

$$I = \begin{bmatrix}1&0&0\\0&1&0\\0&0&1\end{bmatrix}$$

It is a matrix that when multiplied with any other matrix gives the same matrix.

$$AI = IA = A$$

### Elimination matrix

As discussed in the previous chapter, we perform eliminations and row exchanges on $A$ to obtain $U$ which is an upper triangular matrix.

Each elimination step can be viewed as a matrix.  

$E_{21}$ is a matrix that converts $2^{nd}$ row and $1^{st}$ column to 0.

$$E_{21}.A = U_{21}$$  

$$\begin{bmatrix}1 & 0 & 0 \\ -1 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}.\begin{bmatrix}1 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$ 

$$=\begin{bmatrix}1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$

All these steps can be combined to form a single elimination matrix.  
 
$$E_{32}.E_{31}.E_{21} = E$$  

$$E.A = U$$

Where $U$ is the upper triangular matrix.

$$A = E^{-1}U$$

$$A = LU$$

Where $L$ is the inverse of $E$. This allows us to get $A$ back from $U$.

### Permutation matrix  
Each row exchange step can be viewed as a matrix.  

A Permutation matrix $P_{ij}$ lets us exchange the row $i$ with row $j$. 

$$P_{31}= \begin{bmatrix}0&0&1\\0&1&0\\1&0&0\end{bmatrix}$$

$$A = \begin{bmatrix}0&2&3\\8&6&7\\5&9&4\end{bmatrix}$$  

$$P.A = \begin{bmatrix}5&9&4\\8&6&7\\0&2&3\end{bmatrix}$$   

### Augmented Matrix 


$$3x + 4y + 5z = 22$$  

$$2x + 5y + 7z = 23$$  

$$4x + 3y + 6z = 24$$  

$$\begin{bmatrix}3&4&5\\2&5&7\\4&3&6\end{bmatrix}.\begin{bmatrix}x\\y\\z\end{bmatrix}=\begin{bmatrix}22\\23\\24\end{bmatrix}$$    

All the eliminations steps and permutation steps that were applied on $A$ have to applied on the right side i.e. on $B$ as well.

$$AX = B$$

So, to make it simpler we can add $B$ as a column to $A$ separated by ':'.  
This is known as an **augmented matrix**.   


$$A\;B = \begin{bmatrix}3&4&5&:22\\2&5&7&:23\\4&3&6&:24\end{bmatrix}$$

Below, $E$ is the matrix formed by multiplying all the individual elimination matrices

$$E = \begin{bmatrix}1&0&0\\0&1&0\\0&1&1\end{bmatrix}.\begin{bmatrix}1&0&0\\0&1&0\\ \frac{-4}{3}&0&1\end{bmatrix}.\begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\0&0&1\end{bmatrix}$$  

$$E = \begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\-2&1&1\end{bmatrix}$$

$$E.(A\;B)= \begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\-2&1&1\end{bmatrix}.\begin{bmatrix}3&4&5&:22\\2&5&7&:23\\4&3&6&:24\end{bmatrix}$$

$$E.(A\;B)=\begin{bmatrix}3&4&5&:22\\0& \frac{7}{3}& \frac{11}{3}&: \frac{25}{3}\\0&0&3&:3\end{bmatrix}$$

Now this can be solved with back substitution.

$$3x + 4y + 5z = 22$$  

$$\frac{7}{3}y + \frac{11}{3}z = 23$$  

$$3z = 3$$  


### Rules for matrix multiplication
### Associative law

$$A(BC) = (AB)C$$

### Commutative law

$$AB \neq BA$$  


## 2.4 Rules for Matrix Operations

Matrix $A$ with $n$ columns can multiply matrix $B$ with $n$ rows : 

$$A_{mxn}.B_{nxp} = C_{mxp}$$   

If this condition is not satisfied then the matrices cannot be multiplied.

### First way
Take the dot product of each row of A with each column of B:  

Here, $$C_{ij} = A_{i^{th} row}.B_{i^{th} col}$$.  

For example :  

$$\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}.\begin{bmatrix} b_{11} & b_{12} \\ b_{21} & b_{22} \end{bmatrix}$$ 

$$=\begin{bmatrix} a_{11}b_{11}+a_{12}b_{21} & a_{11}b_{12}+a_{12}b_{22} \\ a_{21}b_{11}+a_{22}b_{21} & a_{21}b_{12}+a_{22}b_{22} \end{bmatrix}$$  

### Second way
Matrix $A$ times every column of $B$:  

$$\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}.\begin{bmatrix} b_{11} & b_{12} \\ b_{21} & b_{22} \end{bmatrix}$$  

$$=\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}.\begin{bmatrix} b_{11} \\ b_{21} \end{bmatrix}
and
\begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}.\begin{bmatrix} b_{12} \\ b_{22} \end{bmatrix}$$ 

The result of the first matrix-vector multiplication gives the elements of the first column.

The result of the second matrix-vector multiplication gives the elements of the second column.

$$=\begin{bmatrix} a_{11}b_{11}+a_{12}b_{21} && a_{11}b_{12}+a_{12}b_{22} \\ a_{21}b_{11}+a_{22}b_{21} && a_{21}b_{12}+a_{22}b_{22} \end{bmatrix}$$

In $AB = C$, each column of $C$ is the linear combination of the columns of $A$. Where the constants are taken from the columns of $B$.

### Third way
Each row of $A$ multiplies the whole matrix $B$. The result
is a row of $C$. 

$$C = \begin{bmatrix}A_{row1}.B &.&.\\ A_{row2}.B&.&.\\A_{row3}.B&.&.\end{bmatrix}$$

This approach takes us back to the first or second approach.

Because we still have to calculate

$$A_{i^{th}row}.B$$

### Fourth way
Multiply columns $1$ to $n$ of $A$ times rows $1$ to $n$ of $B$:  

$$\begin{bmatrix} a_{11} \\ a_{21} \end{bmatrix}.\begin{bmatrix} b_{11} & b_{12} \end{bmatrix}
+
\begin{bmatrix} a_{12} \\ a_{22} \end{bmatrix}.\begin{bmatrix} b_{21} & b_{22} \end{bmatrix}$$
    
$$=\begin{bmatrix} a_{11}b_{11} && a_{11}b_{12} \\ a_{21}b_{11} && a_{21}b_{12} \end{bmatrix}+\begin{bmatrix} a_{12}b_{21} && a_{12}b_{22} \\ a_{22}b_{21} && a_{22}b_{22} \end{bmatrix}$$ 


$$=\begin{bmatrix} a_{11}b_{11}+a_{12}b_{21} && a_{11}b_{12}+a_{12}b_{22} \\ a_{21}b_{11}+a_{22}b_{21} && a_{21}b_{12}+a_{22}b_{22} \end{bmatrix}$$

## 2.5 Inverse Matrix


- An inverse of $A$ i.e. $A^{-1}$ when multiplied with $A$ gives the identity matrix.

$$A^{-1} A = AA^{-1} = I$$

- When taken from left to right $A$ becomes $A^{-1}$.

$$AX=B$$

$$A^{-1}AX = A^{-1}B$$

$$IX = A^{-1}B$$

$$X = A^{-1}B$$

- Not all matrices have inverses.

- Only square matrices with linearly independent rows and columns are invertible.

- If the inverse of a matrix does not exist it is called a non-invertible matrix. 

### Testing Inverse

- If matrix is `diagonally dominant`, it has inverse.

- $A^{-1}$ exists iff elimination produces $n$ `pivot points`. That is no diagonal element should be zero.

- Matrix $A$ cannot have two different inverses which means while solving $AX=B$ for $X$ 

$$X = A^{-1}B$$ 

- $X$ is a unique solution.

- If $AX = 0$, and $X \neq 0$ then $A^{-1}$ does not exist.

- $A^{-1}$ exists if determinant of $A \neq 0$.

### Diagonally dominant 
A square matrix is said to be diagonally dominant if for every row the diagonal element is greater than the sum of the other elements.

$$A =\begin{bmatrix} a_{11} & a_{12} & a_{13}\\ a_{21} & a_{22} & a_{23}\\a_{31} & a_{32} & a_{33}\end{bmatrix}$$

If $A$ is diagonally dominant $a_{11} > a_{12} + a_{13}$ , $a_{22} > a_{21} + a_{23}$ and so on.

### The inverse of a product

If $A$ and $B$ are invertible. Then $AB$  is also invertible.

$$B^{-1} A^{-1}  = (AB)^{-1}$$  

$$C^{-1} B^{-1} A^{-1}   = (ABC)^{-1}$$

### Calculating $A^{-1}$ using Gauss-Jordan

Consider a matrix $K$.

$$K= \begin{bmatrix}2 & -1 & 0 \\-1 & 2 & -1 \\ 0 & -1 & 2 \end{bmatrix}$$

Let there be a sequence of elimination steps that can convert $K$ to $I$.

These steps can be written as elimination matrices.

$$E_{21}.E_{31}....K = I$$

The elimination matrices can be represented as a single matrix.

$$EK = I$$

It is obvious that $E$ is nothing but $K^{-1}$.

If we multiply the same matrix $E$ with $I$

$$EI = E = K^{-1}$$

**Thus, it can be said that the sequence of steps that converts $K$ to $I$ also converts $I$ to $K^{-1}$**

Gauss-Jordan method uses this concept on an augmented matrix to compute the inverse of a matrix.

$$ K\;I= \begin{bmatrix}2 & -1 & 0 & :1 & 0 & 0\\-1 & 2 & -1 & :0 & 1 & 0 \\ 0 & -1 & 2 & :0 & 0 & 1 \end{bmatrix}$$

#### Perform Row Elimination

Use the pivots to convert to upper triangular form.

$$= \begin{bmatrix}2 & -1 & 0 & :1 & 0 & 0\\0 & \frac{3}{2} & -1 & :\frac{1}{2} & 1 & 0\\0 & 0 & \frac{4}{3} & :\frac{1}{3} & \frac{2}{3} & 1\end{bmatrix}$$

#### Zero above pivots

Use the pivots again to convert each element above the pivot to zero.

$$= \begin{bmatrix}2 & 0 & 0 & :\frac{3}{2} & 1 & \frac{1}{2}\\0 & \frac{3}{2} & 0 & :\frac{3}{4} & \frac{3}{2} & \frac{3}{4} \\ 0 & 0 & \frac{4}{3} & :\frac{1}{3} & \frac{2}{3} & 1\end{bmatrix}$$

#### Divide row by diagonal elements

Divide each row by the diagonal element of that row to obtain identity matrix on the left.

$$= \begin{bmatrix}1 & 0 & 0 & :\frac{3}{4} & \frac{1}{2} & \frac{1}{4}\\0 & 1 & 0 & :\frac{1}{2} & 1 & \frac{1}{2} \\ 0 & 0 & 1 & :\frac{1}{4} & \frac{1}{2} & \frac{3}{4}\end{bmatrix}$$

$$=I\;K^{-1}$$  

- If $K$ is symmetric then $K^{-1}$ is also symmetric.  
    
## 2.6 Elimination = Factorization : LU

- $EA = U$, $U$ is an upper triangular matrix.
- $E^{-1} = L$
- Then the original matrix $A$ can be recovered by $A = LU$
- Elimination on $AX = B$ gives $UX = C$. 

## Transposes and permutations

### Transpose

Transpose of a matrix means to exchange its rows and columns.

$$A = \begin{bmatrix}1&2\\3&4\\5&6\end{bmatrix}$$  

$$A^{T} = \begin{bmatrix}1&3&5\\2&4&6\end{bmatrix}$$  

**Although it looks quite simple, transpose has a lot of uses which we will discuss in chapters to come.**

#### A sneak-peak :

Consider a two vectors as single column matrices,

$$A = \begin{bmatrix}a\\b\end{bmatrix}, B = \begin{bmatrix}c\\d\end{bmatrix}$$

Then the dot product $A.B$ is just the matrix multiplication of $A^{T}$ and $B$.

$$A^{T}B = \begin{bmatrix}a & b\end{bmatrix}\begin{bmatrix}c\\d\end{bmatrix}$$

$$A^{T}B = ac+bd$$


### Some rules for transpose

$$(A+B)^{T} = A^{T} + B^{T}$$

$$(A.B)^{T} =  B^{T}.A^{T}$$

$$(A^{-1})^{T} =  (A^{T})^{-1}$$

- If $A$ is invertible $A^{T}$ is also invertible.

### Symmetric matrices

- $S$ is a symmetric matrix if $S = S^{T}$

## Summary

- Each elimination step can be viewed as a matrix.  

$$E_{21}.A = U_{21}$$  

- All the steps can be combined to form a single elimination matrix.  
 
$$E_{32}.E_{31}.E_{21} = E$$  

$$E.A = U$$

- All the eliminations steps that were applied on $A$ have to applied on the right side i.e. on $B$ as well.

$$AX = B$$

- So, to make it simpler we can add $B$ as a column to $A$ separated by ':'.  
- This is known as an augmented matrix.   

- Associative law

$$A(BC) = (AB)C$$

- Commutative law

$$AB \neq BA$$

- Matrix $A$ with $n$ columns can multiply matrix $B$ with $n$ rows : 

$$A_{mxn}.B_{nxp} = C_{mxp}$$   

- An inverse of $A$ i.e. $A^{-1}$ when multiplied with $A$ gives the identity matrix.

$$A^{-1} A = AA^{-1} = I$$

- When taken from left to right $A$ becomes $A^{-1}$.

$$X = A^{-1}B$$

- Only square matrices with linearly independent rows and columns are invertible.

- $A^{-1}$ exists if determinant of $A \neq 0$.

- Inverse

$$B^{-1} A^{-1}  = (AB)^{-1}$$  

$$C^{-1} B^{-1} A^{-1}   = (ABC)^{-1}$$

- The sequence of steps that converts $K$ to $I$ also converts $I$ to $K^{-1}$

- $EA = U$

- $E^{-1} = L$

- The original matrix $A$ can be recovered by $A = LU$

- Transpose of a matrix means to exchange its rows and columns

$$(A.B)^{T} =  B^{T}.A^{T}$$

$$(A^{-1})^{T} =  (A^{T})^{-1}$$


- $S$ is a symmetric matrix if $S = S^{T}$


References

1. Introduction to Linear Algebra, W. G. Strang
2. Essence of linear algebra : [https://www.3blue1brown.com/](https://www.3blue1brown.com/)