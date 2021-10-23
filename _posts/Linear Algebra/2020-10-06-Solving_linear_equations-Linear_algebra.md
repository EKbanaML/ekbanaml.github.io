---
title: "Solving Linear Equations"
excerpt_separator: "An introduction to using matrices to solve linear equations."
last_modified_at: 2020-10-22
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

In linear algebra, systems of linear equations are viewed as matrix-vector operations. Elimination is a method of solving systems of linear equations. It is the method that is used by almost every software package to solve the equation. The elimination technique can succeed and may fail, if it succeeds it helps to get the solution. The key idea of elimination is it converts coefficient matrix A to upper triangular matrix U if everything goes fine(success).
The main diagonal elements are called **pivots**. In elimination, we will use pivot elements and we will make all the elements below pivot elements to 0 which finally leads to the Upper triangular matrix U(if succeeds).

**Example:**  
$x - 2y + z = 0$  
$2x + y - 3z = 5$  
$4x - 7y + z = -1$  

The augmented matrix which represent this system is:  

$$\begin{bmatrix}1&-2&1&:0\\2&1&-3&:5\\4&-7&1&:-1\end{bmatrix}$$  

The `first` step is to produce zeros below the first entry in the first column. The row operations are -2 times row1 added to row2 and -4 times row1 added to row3. The result is:  

$$\begin{bmatrix}1&-2&1&:0\\0&5&-5&:5\\0&1&-3&:-1\end{bmatrix}$$  

The `second` step is to produce a zero below the second entry in the second column. For this inorder to avoid fraction, first interchange row2 and row3 and then add -5 times the second row to the third row. The overall result is:  

$$\begin{bmatrix}1&-2&1&:0\\0&1&-3&:-1\\0&0&10&:10\end{bmatrix}$$  

Since the coefficient matrix has been transformed into an echelon form, the forward part of Gaussian elimination is complete. We later see how to obtain a solution using this obtained augmented matrix using Backsubstitution.

**Failure of Elimination:**  
Suppose we have a 3x3 matrix, then its pivots lie in the main diagonal i.e. (R1, C1), (R2, C2), (R3, C3).

- If the first pivot element i.e. (R1, C1) is 0, as we can’t have pivot element as 0 in such a case the solution is to interchange rows from any other row below.
- If the second pivot element i.e. (R2, C2) is 0, the solution is to interchange with the below row and get a non-zero element in this position i.e. (R2, C2).
- If the third pivot element is 0, then for the 3x3 matrix case there will be no more Row left below, so in this case, the elimination method may fail.

`Permutation matrix` helps to execute row exchange in order to get rid of the problem of zero pivots if possible. We will discuss the permutation matrix in detail in the upcoming section.

### Identity Matrix

An identity matrix $I$ is $n \times n$ square matrix where the elements of the  main diagonal are $1$ and rest are $0$.

Example:

$$I = \begin{bmatrix}1&0&0\\0&1&0\\0&0&1\end{bmatrix}$$

It is a matrix that when multiplied with any other matrix gives the same matrix.

$$AI = IA = A$$

### Elimination matrix

As discussed in the previous chapter, we perform eliminations and row exchanges on $A$ to obtain $U$ which is an upper triangular matrix.

Each elimination step can be viewed as a matrix. The elimination matrix $E$ combines all the steps that are required to obtain the upper triangular matrix $U$ from a coefficient matrix $A$ i.e. $EA=U$. Here matrix multiplication $E$ and $A$ is easy if we consider the linear combination of rows of $A$ using the rows of $E$.

**Example**  
If we have matrix of size 3x3, then the single steps which leads $A$ to $U$ using an elimination matrix is $$(E_{32}E_{31}E_{21}) A = U $$  
Here, $E_{21}$ is an elimination matrix that converts $2^{nd}$ row and $1^{st}$ column to 0.

$$E_{21}.A = U_{21}$$  

$$\begin{bmatrix}1 & 0 & 0 \\ -1 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}.\begin{bmatrix}1 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$ 

$$=\begin{bmatrix}1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 \end{bmatrix}$$  

Similary, $E_{31}$ is an elimination matrix that eliminates row3 and column1 and $E_{32}$ is an elimination matrix that eliminates row3 and column2.

All these steps can be combined to form a single elimination matrix.  
 
$$E_{32}.E_{31}.E_{21} = E$$  

$$E.A = U$$

Where $U$ is the upper triangular matrix.

$$A = E^{-1}U$$

$$A = LU$$

Where $L$ is the inverse of $E$. This allows us to get $A$ back from $U$.

### Backsubstitution

It is the process of solving a linear system of equations that has been transformed into a row-echelon form or reduced row-echelon form. So in this, the last equation is solved first, then the next-to-last, and so on.
Say we have a system of linear equations expressed as **AX = B**. Firstly we create an augmented matrix by concatenating columns of A (coefficient matrix) and B (right side of equations) which results **[A | B]**. Once the augmented matrix is constructed then repeating the process of elimination, the augmented matrix expressed in the form [A | B] gets converted to **[U | C]**, where U is the upper triangular matrix, and C is some right side vector obtained after the process of elimination. Then using U and C we will express the augmented matrix in terms of a system of linear equations and solve using Backsubstitution starting from the bottom of equations.
Hence, Backsubstitution is a technique for solving systems of linear equations in reverse order.  
**Example:**  
Suppose we have three equations with three unknowns  

$$3x + 4y + 5z = 22$$  

$$2x + 5y + 7z = 23$$  

$$4x + 3y + 6z = 24$$  

Equation in the matrix form i.e. $AX=B$ is given as,

$$\begin{bmatrix}3&4&5\\2&5&7\\4&3&6\end{bmatrix}.\begin{bmatrix}x\\y\\z\end{bmatrix}=\begin{bmatrix}22\\23\\24\end{bmatrix}$$  

All the eliminations steps and permutation steps that were applied on $A$ have to applied on the right side i.e. on $B$ as well.

So, to make it simpler we can add $B$ as a column to $A$ separated by ':'. This is known as an **augmented matrix**.  
$$A\;B = \begin{bmatrix}3&4&5&:22\\2&5&7&:23\\4&3&6&:24\end{bmatrix}$$  

Now, The elimination matrix $E$ obtained by combining several other sub-elimination matrices i.e. $E_{32}$, $E_{31}$, $E_{21}$ is given by,  

$$E = \begin{bmatrix}1&0&0\\0&1&0\\0&1&1\end{bmatrix}.\begin{bmatrix}1&0&0\\0&1&0\\ \frac{-4}{3}&0&1\end{bmatrix}.\begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\0&0&1\end{bmatrix}$$  

$$E = \begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\-2&1&1\end{bmatrix}$$

$$E.(A\;B)= \begin{bmatrix}1&0&0\\ \frac{-2}{3}&1&0\\-2&1&1\end{bmatrix}.\begin{bmatrix}3&4&5&:22\\2&5&7&:23\\4&3&6&:24\end{bmatrix}$$

The obtained Augmented matrix after performing operation using an elimination matrix is given by, 

$$E.(A\;B)=\begin{bmatrix}3&4&5&:22\\0& \frac{7}{3}& \frac{11}{3}&: \frac{25}{3}\\0&0&3&:3\end{bmatrix}$$

Let us write obtained Augmented matrix interms of system of linear equation,

$$3x + 4y + 5z = 22$$  

$$\frac{7}{3}y + \frac{11}{3}z = 23$$  

$$3z = 3$$ 

Now this can be solved using a technique known as Backsubstitution i.e. solving system of linear equation in a reverse order.

`Matrix multiplication follows associative law but doesn't follow commutative law i.e.`    

- Associative law: $$A(BC) = (AB)C$$  
- Commutative law: $$AB \neq BA$$  

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

A matrix can be considered as some transformation that transforms the input vector into some other vector. The inverse of the same matrix does reverse operation i.e. it recovers the original vector from that transformed vector provided that inverse of a matrix exists.

## Invertible matrices  

Invertible matrices are those matrices whose inverse exists. We know,  

$$A^{-1} = 1/det {A} * adj(A)$$  

For inverse to exist determinant of matrix must exist. In 2D determinants is an area of a parallelogram, and in 3D determinants is the volume of a parallelepiped. So, in 2D for determinants to exist, 2 columns in the matrix must be independent, and in 3D for determinants to exist, 3 columns in the matrix must be independent. Hence we can say if the matrix has determinants then it is invertible otherwise it is not. 

**Invertible condition:**  $det(A) != 0$  
**Non-invertible condition:** $det (A) = 0$


Invertible matrices are also called `Non-Singular matrices` and Non-invertible matrices are also called `Singular matrices`.

**Some fact:**

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

**Idea:**  
Start with long matrix $[A|I]$ then using forward and backward elimination you will get $$[I|A^{-1}]$$ i.e. $$[A|I] = [I|A^{-1}]$$. In this way Gauss-Jordan method helps to find the inverse of matrices.  

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

**Q. Why $$[A | I]$$ must equal $$[I | A^{-1}]$$?**  
Let $E$ be the elimination matrix that eliminates $A$ to $I$ using forward and backward elimination techniques. So,  
$$E*[A|I] = [E*A|E*I]$$  
We know, $$E*A=I$$, then,  
$E$ must be inverse of $A$ i.e. $E$ is the $$A^{-1}$$, also $$E*I=E$$.  

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

#### A sneak-peak:  

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

In the section of elimination, we have seen word permutation matrix, so let's try to understand what really permutation matrix is?

### Permutation

Permutation matrices denoted by $P$ are those matrices that simply exchange either rows or columns of a given matrix. Permutation matrices are basically identity matrices with reordered rows. Multiplication is done in two ways depending on whether to exchange rows or columns of a given matrix.  

- If we want to exchange rows, the permutation matrix multiplies the given matrix from the left side.  
- If we want to exchange columns, the permutation matrix multiplies the given matrix from the right side.

During the process of Elimination, permutation matrices can be useful if the pivot in a matrix is zero, As permutation matrices execute row exchange operation to produce non-zero in pivot position (if possible).  

- Description of elimination without row exchange is $A=LU$
- Description of elimination with row exchange is $PA=LU$  

**Important point about Permutation matrices**  

- $P^{-1}$ = $P^{T}$
  - This means if we exchange row1 and row2 using P, then its inverse again exchange same row1 and row2 to get the original matrix.
- If we multiply any two permutation matrix then again we get a permutation matrix in the list.
- There are `n!` possibility of row exchange (n is number of rows).  

**Example:**  

A Permutation matrix $P_{ij}$ lets us exchange the row $i$ with row $j$.  

$$P_{31}= \begin{bmatrix}0&0&1\\0&1&0\\1&0&0\end{bmatrix}$$  

$$A = \begin{bmatrix}0&2&3\\8&6&7\\5&9&4\end{bmatrix}$$  

$$P_{31}.A = \begin{bmatrix}5&9&4\\8&6&7\\0&2&3\end{bmatrix}$$  

As we can see above, $P_{31}$ execute row exchange i.e. row1 and row3

### Symmetric matrices

Symmetric matrices means transposing doesn't change the matrix i.e.  $S = S^{T}$  

**Example:**  

$$S = \begin{bmatrix}3&1&7\\1&2&9\\7&9&4\end{bmatrix}$$  

$$S^{T} = \begin{bmatrix}3&1&7\\1&2&9\\7&9&4\end{bmatrix}$$  

Hence, $S$ is a symmetric matrices.

`Note:` $$R^{T}R$$ is always symmetric, where R is a rectangular matrix.

We will get to know about symmetric matrices in detail in the chapter `Eigen values and Eigen vectors` later.

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

<strong>co-author: </strong><a href="https://www.linkedin.com/in/anish-thapaliya-83a806194/">Anish</a>  

## References

1. Introduction to Linear Algebra, W. G. Strang
2. Essence of linear algebra : [https://www.3blue1brown.com/](https://www.3blue1brown.com/)