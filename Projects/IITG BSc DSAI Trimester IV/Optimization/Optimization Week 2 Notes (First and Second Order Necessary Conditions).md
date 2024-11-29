---
course_name: optimization
trimester_week: 2
---

## Standard minimization problem   
The standard minimization problem is written as:   

$$
\text{minimize } f(\vec x)\\\text{subject to }x\in \Omega
$$
where $f: \mathbb R^n → \mathbb R$ is the objective function or the cost function.   
$\vec x$ is the vector containing all the decision variables   
and $\Omega \subset \mathbb R^n$ is the constraint set or feasible set.   
Optimization problems contain either minimizing or maximizing an objective function, a maximization problem can be viewed as minimizing $-f$ instead of $f$, hence without loss of generality we can say that optimization problems are minimization problems.   
If $\Omega=\mathbb R^n$, then the problem is called an unconstrained optimization problem.   
Constrain $x\in\Omega$ is a set constraint that $x$ must belong to a particular set or interval.   
Functional constraints also exist in which we say that some function of $x$ must be equal to zero, or some function of $x$ must be less than or greater than zero.   
## Local and Global minimizers   
For a function $f: \mathbb R^n → \mathbb R$, a vector $\vec x^\* \in \mathbb \Omega$ is a local minimizer if there exists an $\epsilon > 0$, such that $f(\vec x)>f(\vec x^\*)$ for all $\vec x \in (\Omega \setminus \vec x^\* )$ and $\|\|\vec x - \vec x^\*\|\|\leq \epsilon$   
A vector $\vec x^\*$ is a global minimizer if for every $\vec x \in (\Omega \setminus \vec x^\* )$, $f(\vec x) \geq f(\vec x^\*)$    
## arg min   

$$
\text{argmin}_{\vec x \in \Omega} f(\vec x)
$$
arg min is a function that takes in an objective function and gives the minimizer for that function.   
A condition or constraint can be written under the arg min to specify constraints.   
## Feasible direction   
A vector $\vec d \in \mathbb R^n$ is a feasible direction at $\vec x \in \Omega$ if there exists some $\alpha\_0 > 0$, such that $x+\alpha \vec d \in \Omega$ for all $[0, \alpha\_0]$   
If for any point in the domain of object function, we move in some direction, and still remain in the domain, then that direction is called a feasible direction.   
If the magnitude of feasible direction vector $\|\|\vec d\|\|$ is 1, then it is the change of $f$ at $\vec x$ if we move in the direction $\vec d$   
## Directional derivative   
The change in $f$ with respect to moving in the direction $\vec d$ from some point $\vec x$ is:   

$$
\frac{\partial f}{\partial \vec d} = \lim_{\alpha\to0}\frac{f(\vec x+\alpha\vec d)-f(\vec x)}{\alpha}
$$
for some very small step $\alpha$ in the direction $\vec d$.   
Solving the above gives:   

$$
\bf{\vec d}^T\nabla f(\vec x)=\langle \nabla f(\vec x),\bf{\vec d} \rangle=\nabla f(\vec x)^T\bf{\vec d}
$$
## Neighbourhood   
A neighbourhood of a point $\vec x$ is a set of vectors such that the euclidean distance between those vectors and $\vec x$ is less than some positive $\epsilon$   
$\vec x$ is an interior point of $\Omega$ if for some values of $\epsilon$, the neighbourhood of $\vec x$ lies completely inside $\Omega$. If instead, for all values of $\epsilon$, some part of neighbourhood lies outside as well as inside $\Omega$, then $\vec x$ is a boundary point.   
A minimizer might lie inside or outside the boundary of $\Omega$.   
## First order necessary condition (FONC)   
FONC is a condition that helps in determining whether the minimizer is valid or not.   
If $\vec x^\*$ is a local minimizer for $f$, then for any feasible direction $\vec d$, the following is satisfied:   

$$
\bf{\vec d}^T\nabla f(\vec x^*)\geq 0
$$
If $\vec x^\*$ is an interior point of $\Omega$, then FONC reduces to:   

$$
\nabla f(\vec x^*)=0
$$
To determine if FONC is satisfied for some vector:   
1. First take the gradient of the objective function and substitute the values of the given vector inside the gradient.   
2. Then take some arbitrary values $d\_1,…,d\_n$ for the feasible direction vector and do the inner product with the gradient obtained in the previous step.   
3. Then try to find the valid values of $d\_1, d\_2, …, d\_n$ such that the given condition is respected.   
   
## Second order necessary condition (SONC)   
Let $\Omega \in \mathbb R^n$, and let $\vec x^\*$ be the local minimizer of the objective function $f$ over $\Omega$. Then SONC can be defined as:   
For any feasible direction $\vec d$, at $\vec x^\*$, if $\bf{\vec d}^T\nabla f(\vec x^\*)=0$, then, for the feasible direction to satisfy SONC, it must follow:   

$$
\bf{\vec d}^T\nabla \textbf{F}(\vec x^*)\bf{\vec d}\geq 0
$$
For interior points, we only need to check $\nabla f(\vec x^\*)=0$ for FONC, and $\textbf F(\vec x^\*)$ is positive semidefinite.   
A matrix $A$ is called positive semidefinite if all its eigenvalues are positive and $x^TAx\geq 0$ for all vectors $x\neq 0$, SONC requires Hessian matrix to be positive semidefinite matrix.   
## Second order sufficient condition (SOSC)   
If we apply SONC on $f(x)=x^3$, then the first derivative at $x^\*=0$ will be 0, and the second derivative will also be zero, which means that SONC is satisfied which requires first order derivative to be zero, and second order derivative (Hessian in the case of vectors) to be positive.   
However, in the case of $x^3$, even though SONC is satisfied at $x^\*=0$, $x^\*=0$ is not the minima, as the value of $x^3$ decreases further to the left. Therefore while it is required that SONC be satisfied for the point to be considered a minima, it is not a sufficient condition, as it is prone to inflection points.   
Hence, we need a sufficient condition for a point to be a strict minima.   
Let $f$ be a function that is twice continuously differentiable, If for a certain $x^\*$, ($\nabla f(\vec x^\*)=0$) is satisfied, then for SOSC to be satisfied it must follow:   

$$
\bf{\vec d}^T \nabla^2f(\vec x^*)\bf{\vec d}\gt 0
$$
If $\nabla^2 f(\vec x^*)$ is negative definite, then $x^*$ is a maximizer.   
Alternative way to state SOSC:   

$$
\nabla f(\vec x^*)=0\\ \textbf{F}(\vec x^*)>0
$$
   
