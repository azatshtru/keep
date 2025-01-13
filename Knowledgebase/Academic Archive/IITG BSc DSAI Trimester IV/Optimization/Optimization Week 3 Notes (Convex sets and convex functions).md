---
course_name: optimization
trimester_week: 3
---

"points" and "vectors" are used interchangeably in the following weekly note.   
## Line segment   
The line segment between two vectors $\vec x$ and $\vec y$ in $\mathbb R^n$ is a set of vectors on the "straight line" joining $\vec x$ and $\vec y$.   
If a point $\vec z$ lies on the line segment between $\vec x$ and $\vec y$, then:   

$$
\vec z-\vec y = a(\vec x-\vec y)\qquad\qquad \text{where }a\in[0, 1]
$$
This equation basically means that subtracting the vector z from vector y will give some vector, which will be equal to the vector obtained by subtracting the vector x from vector y and then scaling it down with some scalar a.   
Rearranging the equation above, we get:   

$$
\vec z= a\vec x+(1-a)\vec y 
$$
Hence any point on the line segment between x and y can be represented as a linear combination of its two endpoint vectors where the linear coefficients add up to one, this type of linear combination is called a **convex combination**.   
The line segment between two points $\vec x$ and $\vec y$ can be defined as the set of all the convex combinations between $\vec x$ and $\vec y$:   

$$
L=\{\vec w\in \mathbb R^n:\vec w= a\vec x+(1-a)\vec y, a\in[0, 1]\}
$$
## Convex sets   
Any subset $S$ of $\mathbb R^n$ is called a **convex set** if for all vectors $\vec u, \vec v$ in $S$, the line segment between u and v is also in $S$.   

```tikz
$$
\documentclass[border=20mm]{standalone}
%\documentclass{article}
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}
\tikzset{dot/.style={circle,fill,inner sep=1.5pt}}

\begin{scope}[local bounding box=L]  % the left
\def\a{1.5}
\draw[fill=red!50,line width=1.5pt] 
(-\a,0) .. controls +(75:3) and +(105:3).. (\a,0) --cycle;
\path (.6*\a,0) node[dot]{} (-.6*\a,0) node[dot]{};
\end{scope}

\begin{scope}[local bounding box=C,shift={(3,1)}]     % the center
\draw[fill=orange!50,line width=1.5pt] 
(0,-1.5) .. controls +(170:.5) and +(180:2) ..
(0.5,2) .. controls +(0:1.2) and +(0:.5) .. cycle;
\draw (0,0) node[dot]{} -- (.3,1.5) node[dot]{};
\end{scope}

\begin{scope}[local bounding box=R,shift={(6,1)}]   % the right
\def\b{1}
\draw[fill=violet!40,rounded corners=2mm,line width=1.5pt] 
(\b,2.5) .. controls +(-95:5) and +(-85:5) ..
(-\b,2.5) .. controls +(-60:2) and +(-120:2) .. 
cycle        % I like that! Symmetry is not necessary here
%(\b,2.5)    % I like that too
;
\draw (.75,1.6) node[dot]{} -- (-.75,1.6) node[dot]{};
\end{scope}

% for legends
\path 
(C.south)+(0,-.7) node (M) {strictly convex}
(L.south|-M) node{convex}
(R.south|-M) node{non-convex};
\end{tikzpicture}

\end{document}

$$

```
### Properties of Convex sets   
1. If some $\Theta$ is a convex set, then for some constant k, the set $k\Theta = \{\vec v:\vec v=k\vec u, \vec u \in \Theta\}$ is also a convex set.   
2. If $\Theta\_1, \Theta\_2$ are convex sets, then the set $\Theta\_1+\Theta\_2 = \{\vec v:\vec v=\vec u\_1+\vec u\_2, \vec u\_1 \in \Theta\_1,\vec u\_2 \in \Theta\_2\}$ is also a convex set.   
3. The intersection of any collection of convex sets is also convex.   
   
## Convex functions   
A function $f:\Theta→\mathbb R^n$ ($\Theta$ is a convex set) is called a convex function if:   

$$
f(a\vec x+(1-a)\vec y)\leq af(\vec x)+(1-a)f(\vec y)
$$
Geometrically, one can imagine that the line segment between any two points on the function's graph always stays above the graph.   
![Screenshot 2024-09-20 at 2.18.15 PM.png](files/screenshot-2024-09-20-at-2-18-15-pm.png)    
A convex function   
![Screenshot 2024-09-20 at 2.20.03 PM.png](files/screenshot-2024-09-20-at-2-20-03-pm.png)    
A non-convex function   
Some other examples of convex functions are:   
1. $e^{ax} \quad\forall\space a\in \mathbb R$   
2. $\|x\|^p \quad\forall\space p \geq 1$   
   
### The local minimizer of a convex function is also a global minimizer.   
### Strict convexity: $f(a\vec x+(1-a)\vec y)\lt af(\vec x)+(1-a)f(\vec y)$   
### Concave functions   
A function $f$ is said to be strictly concave, if $-f$ is strictly convex. A concave function's graph always lies above the line segment connecting any of its two points.   
### Quadratic form and convexity   
Quadratic form of a vector $\vec x$ is given by the function $f(\vec x)=\vec x^T Q\vec x$, where $Q=Q^T$, then f(x) is convex **if and only if **for all the points $\vec x, \vec y$ in the domain of $f$:   

$$
(\vec x-\vec y)^T Q(\vec x-\vec y) \geq 0
$$
    
   
