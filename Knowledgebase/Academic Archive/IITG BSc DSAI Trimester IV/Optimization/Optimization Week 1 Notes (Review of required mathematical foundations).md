---
course_name: optimization
trimester_week: 1
---

- Vectors and Matrices   
- Eigen values and Eigen vectors   
- Quadratic forms   
- Elements of calculus   
   
# Linear Algebra   
## Vectors   
A **column n-vector** is an array of n numbers denoted as:   

$$
\vec{a}=\begin{bmatrix}a_1\\a_2\\\vdots\\a_n\end{bmatrix}
$$
$a\_i$ is the $i^{th}$ component of the vector $\vec{a}$   
$\mathbb R$ denotes the set of real numbers. $\mathbb R^n$ denotes $n$ dimensional real vector space.   
A vector space is a set of column n-vectors with real components.   
A **row n-vector** is the transpose of a given column vector $\vec a$ and is denoted by $\vec{a}^T$:   

$$
\vec{a}^T=[a_1,a_2,\dots,a_n]
$$
### Addition of vectors   
If two vectors $\vec a$ and $\vec b$ are added, then the sum is:   

$$
\vec a+\vec b=[a_1+b_1,a_2+b_2,\dots,a_n+b_n]^T
$$
Addition of vectors is commutative, associative and contains an additive identity $\vec{0}$   
### Multiplication of vector with a scalar   
If there exists a scalar k that is multiplies by a vector $\vec a$, then the product is:   

$$
k\vec a=[k\vec a_1,k\vec a_2,\dots,k\vec a_n]^T
$$
Multiplication is distributive, associative and contains multiplicative identity.   
## Linear Independence   
A set of vectors $\vec a\_1,\dots,\vec a\_n$ are said to be linearly independent if for scalars $x\_1,\dots,x\_n$, the equation $x\_1\vec a\_1 +\dots+x\_n\vec a\_n = 0$ implies that all $x\_i$ are zero.   
If vectors are not linearly independent, then they are linearly dependent.   
A vector $\vec a$ is said to be a linear combination of other vectors $\vec a\_1,\dots,\vec a\_n$ if it can be represented as $\vec a = x\_1\vec a\_1 +\dots+x\_n\vec a\_n$ where $x\_i$ are scalars.   
### Another interpretation of Linear dependence   
A set of vectors are said to be linearly dependent if any vector from the set can be represented as a linear combination of other vectors.   
## Vector subspace   
A subset $V$ of $\mathbb R^n$ is called a subspace of $\mathbb R^n$ if $V$ is closed under vector addition and scalar multiplication.   
Which means that if $\vec a$ and $\vec b$ are in $V$, then $\vec a + \vec b, ka$ and $kb$ are also in $V$.   
### Span of a vector   
Let $a\_1,\dots,a\_k$ be arbitrary vectors in $\mathbb R^n$. The set of all the linear combinations of these arbitrary vectors is called the span of all these arbitrary vectors.   

$$
\text{span}[\vec a_1,\dots,\vec a_k]=\sum_{i=1}^kx_i\vec a_i
$$
The span of any set of vectors is a subspace.   
### Basis of a subspace   
Given a subspace $V$, any set of linearly independent vectors in $V$ such that the span of those linearly independent vectors equals $V$ are called the basis vectors of $V$.   
Any vector in $V$ can be uniquely represented as a linear combination of the basis vectors.   
## Matrix   
A matrix is a rectangular array of numbers. A matrix with m rows and n colums is called a m x n matrix.   
Any column of a matrix is a vector. The kth column of a matrix is denoted as a vector $a\_k$   
### Rank of a matrix   
The maximum number of linearly independent columns of a matrix is called the rank of a matrix. $\text{rank}(A)$   
Rank of a matrix is the dimension of span of columns of the matrix.   
The rank of a matrix doesn't change under the following operations:   
1. Multiplication of the columns of the matrix by nonzero scalars   
2. Interchange of the columns   
3. Addition of a linear combination of other columns to a column.   
   
## Determinant   
Associated with each n x n square matrix is a scalar called the determinant. $\|A\|$   
Determinant is a linear function of each column.   
If two columns of a matrix are same, then the determinant is zero.   
For an Identity matrix, determinant is one.   
### p-th order minor   
A pth-order minor of an (m x n) matrix A, with p < min(m, n) is the determinant of a (p x p) obtained by deleting m - p rows and n - p columns from the matrix A.   
If an (m x n (*m > n*)) matrix A has a nonzero nth-order minor, then the columns of A are linearly independent, that is, rank(A)=n.   
## Solutions of system of equations using matrices   
If there are m equations and n unknowns:   

$$
a_{11}x_1+a_{12}x_2+\dots+a_{1n}x_n=b_1\\
a_{21}x_1+a_{22}x_2+\dots+a_{2n}x_n=b_2\\
\vdots\\
a_{m1}x_1+a_{m2}x_2+\dots+a_{mn}x_n=b_m

$$
We can represent this system using:   

$$
A\vec x=\vec b
$$
where $A$ is a matrix of all the coefficients.   
The augmented matrix is denoted as:   

$$
[\space A\mid b\space] = [\vec a_1, \vec a_2,\dots, \vec a_n, \vec b]
$$
The system of equations has a solution if and only if:   

$$
\text{rank}A=\text{rank}[\space A\mid b\space]
$$
## Inner products and Norms   
The Euclidean Inner product is defined as:   

$$
\langle \vec x, \vec y\rangle=\sum_{i=1}^nx_iy_i=\vec x^T\vec y
$$
The Euclidean Norm of a vector $\vec x$ is defined as:   

$$
||\vec x||=\sqrt{\langle\vec x,\vec x\rangle}=\sqrt{\vec x^T\vec x}
$$
### Cauchy Schwarz inequality   

$$
|\langle \vec x, \vec y\rangle|\leq||\vec x||||\vec y||
$$
## Linear Transformation   
A function $T: \mathbb R^n → \mathbb R^m$ is called a linear transformation if:   
1. $T(a\vec x)=aT(\vec x)$ for every $\vec x \in \mathbb R^n$ and $a\in \mathbb R$   
2. $T(\vec x+\vec y) = T(\vec x) + T(\vec y)$ for every $\vec x,\vec y \in \mathbb R^n$   
   
## Eigenvalues and Eigenvectors   
A scalar $\lambda$ and a nonzero vector $\vec v$ satisfying the equation $A\vec v = \lambda \vec v$ are said to be, respectively, an eigenvalue and eigenvector of matrix $A$.   
For $\lambda$ to be an eigenvalue, it is necessary and sufficient that the matrix $\lambda I - A$ is singular, that is, determinant of $\lambda I - A$ is zero.   
### Characteristic polynomials and characteristic equations   
The determinant of $\lambda I - A$ can be expanded as follows:   

$$
|\lambda I - A| = \lambda^n+a_{n-1}\lambda^{n-1}+\dots+a_1\lambda+a_0
$$
This expanded form of determinant is called the characteristic polynomial and $\|\lambda I-A\|=0$ is called the characteristic equation.   
### Roots of characteristic equation   
If lambda is an eigenvalue of a matrix A, then lambda satisfies the characteristic equation.   
There are n roots of the characteristic polynomial and all of them are eigenvalues of the matrix A.   
If there are n distinct roots of characteristic equation ($\lambda-1,\dots,\lambda\_n$), then there are n linearly independent vectors ($\vec v\_1,\dots,\vec v\_n$) associated to each eigenvalue such that $Av\_i=\lambda\_iv\_i$   
### Other properties of eigenvalues and eigenvectors   
All eigenvalues of a real symmetric matrix are real.   
Any real symmetric n x n matrix has a set of n eigenvectors that are mutually orthogonal.   
## Quadratic form   
A quadratic form is a function $f:\mathbb R^n → \mathbb R$ such that:   

$$
f(\vec x)=\vec x^TQ\vec x
$$
where $Q$ is a (n x n) matrix which, without loss of generality can be assumed symmetric.   
### Definite matrices   
A symmetric matrix $Q$ is positive definite if and only if all eigenvalues of $Q$ are positive. (and vice-versa for negative definite) or $x^TAx>0$   
## Some elements of calculus with vectors   
### First order derivative   
If there is a function in the vector space $f:\mathbb R^n → \mathbb R$, then the first order derivative of $f$ is:   

$$
Df = [\frac{\partial f}{\partial x_1},\frac{\partial f}{\partial x_2},\dots,\frac{\partial f}{\partial x_n}]
$$
While taking partial derivative with respect to $x\_k$, treat every other $x\_i$ as a constant.   
### Gradient   
The gradient of $f$ is the transpose of the first order derivative:   

$$
\nabla f=[Df]^T
$$
### Hessian   
The second order derivative of $f$ is called the Hessian of $f$:   

$$
F(\vec x)=D^2f(\vec x)\\=\begin{bmatrix}\frac{\partial^2f(\vec x)}{\partial x_1^2}&\dots&\frac{\partial^2f(\vec x)}{\partial x_n\partial x_1}\\\vdots&\ddots&\vdots\\\frac{\partial^2f(\vec x)}{\partial x_1\partial x_n}&\dots&\frac{\partial^2f(\vec x)}{\partial x_n^2}\end{bmatrix}
$$
Hessian matrix is a symmetric matrix   
