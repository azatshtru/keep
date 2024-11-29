---
course_name: optimization
trimester_week: 3
---

Unconstrained optimizer deals with finding the local maximizer $\vec x^\*$ for an objective function $f:\mathbb R^n → \mathbb R^n$ **without any imposed restrictions**.   
At the local minimizer, the function always has a value that is less than any other values of the function in the vicinity of the minimizer.   
### Review of FONC, SONC and SOSC   
For an internal point, the first order necessary condition dictates that the gradient of the function with respect to the minimizer must be 0   

$$
\nabla f(\vec x^*) = 0
$$
SONC dictates that the Hessian matrix of of the function with respect to minimizer must be positive semi-definite.   

$$
\vec x^{*T}\nabla^2f(\vec x^*) \vec x^* \geq 0
$$
SOSC requires the points to be strictly positive definite to avoid saddle points:   

$$
\vec x^{*T}\nabla^2f(\vec x^*) \vec x^* \gt 0
$$
## Using Taylor's expansion of multivariables for deriving a generalized system for unconditional optimization   
### Review of taylor series of two variable   
The Taylor expression for two variables is as follows:   

$$
f(x, y) = f(x_0, y_0) + \left[ \left( \frac{\partial f}{\partial x} \right)_{(x_0, y_0)} (x - x_0) + \left( \frac{\partial f}{\partial y} \right)_{(x_0, y_0)} (y - y_0)\right]
+ \frac{1}{2!} \left[ \frac{\partial^2 f}{\partial x^2} (x - x_0)^2 + 2 \frac{\partial^2 f}{\partial x \partial y} (x - x_0)(y - y_0) + \frac{\partial^2 f}{\partial y^2} (y - y_0)^2 \right] + \cdots

$$
If we write,   
1. $\partial f/\partial x$ as $f\_x$   
2. $\partial^2 f/\partial x^2$ as $f\_{xx}$   
3. $\partial^2 f/\partial x\partial y$ as $f\_{xy}$   
   
Then we can write the tay~~l~~or expression as:   

$$
f(x, y) = f(x_0, y_0) + [f_x(x_0, y_0)(x - x_0) + f_y(x_0, y_0)(y - y_0)]
+ \frac{1}{2!} \left[ f_{xx}(x_0, y_0)(x - x_0)^2 + 2 f_{xy}(x_0, y_0)(x - x_0)(y - y_0) + f_{yy}(x_0, y_0)(y - y_0)^2 \right] + \cdots

$$
If we further write $x-x\_0$ as $\delta x$ and $y-y\_0$ as $\delta y$, then we can write this as:   

$$
f(x, y) = f(x_0, y_0) + [f_x(x_0, y_0)\, \delta x + f_y(x_0, y_0)\, \delta y]
+ \frac{1}{2!} \left[ f_{xx}(x_0, y_0)\, (\delta x)^2 + 2 f_{xy}(x_0, y_0)\, \delta x \delta y + f_{yy}(x_0, y_0)\, (\delta y)^2 \right] + \cdots

$$
This can be further written in matrix form by considering that the first order derivative term is just gradient of [x, y] multiplied by [$\delta x$, $\delta y$]   
And the second order term is just the hessian matrix written as quadratic form.   

$$
f(x, y) = f(x_0, y_0)+\begin{bmatrix}
f_x(x_0, y_0) \\
f_y(x_0, y_0)
\end{bmatrix}^T
\begin{bmatrix}
\delta x \\
\delta y
\end{bmatrix}
+ \frac{1}{2!} 
\begin{bmatrix}
\delta x & \delta y
\end{bmatrix}
\begin{bmatrix}
f_{xx}(x_0, y_0) & f_{xy}(x_0, y_0) \\
f_{xy}(x_0, y_0) & f_{yy}(x_0, y_0)
\end{bmatrix}
\begin{bmatrix}
\delta x \\
\delta y
\end{bmatrix}
$$
### What about more than two variables?   
For taylor expansion of n variables that is just two degrees deep, this generalizes to:   

$$
f(\vec x)=f(\vec x_0)+\nabla f(\vec x_0)^T\vec {\delta x}+\frac12\vec {\delta x}^T\nabla^2 f(\vec x_0)\vec {\delta x}+\cdots
$$
where $\vec x\_0$ is the point around which we are calculating the taylor polynomial.   
And we wouldn't for the most part need higher degree terms for our use.   
### Ok, how do we apply this to get the Unconstrained optimization   
Assume we are calculating the Taylor expression around a supposed minimizer:   

$$
f(\vec x)=f(\vec x^*)+\nabla f(\vec x^*)^T\vec {\delta x}+\frac12\vec {\delta x}^T\nabla^2 f(\vec x^*)\vec {\delta x}+\cdots
$$
For a point to be a minimizer, the gradient around it should be zero, i.e., $\nabla f(\vec x^\*) = 0$, therefore the taylor expansion loses the first degree term.   

$$
f(\vec x)=f(\vec x^*)+\frac12\vec {\delta x}^T\nabla^2 f(\vec x^*)\vec {\delta x}
$$
Since only the hessian in the second degree term dictates the sign of second degree term, if the hessian is positive definite, then the second degree term is positive, adding a positive number to $f(\vec x^\*)$ will give a bigger number than $f(\vec x^\*)$. Then the neighbouring points $\vec x$ will have higher values of $f$ than the given point $\vec x^\*$. Hence $\vec x^\*$ is a minima if hessian is positive definite.   
If the second degree term is negative, then by the same logic above, neighbouring points $\vec x$ will have lower values of $f$ than the given point $\vec x^\*$. Hence $\vec x^\*$ is a maxima if hessian is negative definite.   
### Finding minimizers   
For a two variable function,   

$$
\begin{bmatrix}
f_x(x_0, y_0) \\
f_y(x_0, y_0)
\end{bmatrix} = 0
$$
We get two equations from the matrix above:   

$$
f_x(x_0, y_0) = 0 \\
f_y(x_0, y_0) = 0
$$
Two equations and two unknowns $\vec x\_0, \vec y\_0$   
Then we can find the hessian and substitute $\vec x\_0, \vec y\_0$ to check if the following condition is true   

$$
\frac{1}{2} 
\begin{bmatrix}
\delta x & \delta y
\end{bmatrix}
\begin{bmatrix}
f_{xx}(x_0, y_0) & f_{xy}(x_0, y_0) \\
f_{xy}(x_0, y_0) & f_{yy}(x_0, y_0)
\end{bmatrix}
\begin{bmatrix}
\delta x \\
\delta y
\end{bmatrix} \gt 0
$$
If the above condition is true then $\vec x\_0, \vec y\_0$ are the points of a local minimizer.   
   
