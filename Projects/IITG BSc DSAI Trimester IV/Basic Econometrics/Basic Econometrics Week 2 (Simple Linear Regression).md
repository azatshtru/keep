---
trimester_week: 2
course_name: basic econometrics
---

## Nature of Econometrics   
Some socioeconomic factors $X\_1,\dots,X\_n$ affect some other socioeconomic factor $Y$, for example if X increases Y decreases, etc.   
People come up with theories regarding the directions of the effects but not the exact magnitude.    
Our aim is to figure out that the actual data supports the theory or not and if it support what is the magnitude of the changes and if the data does not support the theory, then we go back to the drawing board and come up with a new theory.   
## An example econometric model   
Estimating work productivity as being directly proportional to the wages of the employees. A possible theory is that the wage depends on three things: the education, years of experience and weeks spend in job training. $\text{wage}=f(\text{education}, \text{experience}, \text{job  training})$   
The simplest econometric for this can be that the wage is a linear combination of education, experience and job training.   
$\text{wage} = \beta\_0+\beta\_1\text{education}+\beta\_2\text{experience}+\beta\_3\text{job training}+u$   
where $u$ denotes unknown factors, the data about which are hard to obtain. No matter what factors we add (the innate skill, bargaining ability of the employee, etc.) the $u$ will always be there in the equation.   
## Analysis on the simplest linear model   

$$
y=\beta_0+\beta_1x+u
$$
### cetaris paribus clause   
This is an assumption that the other things remain equal in the linear model, i.e. $\Delta u = 0$.   
For some part of the population, $u$ will be large and will add significant amounts to the model $y$, for other part of the population, $u$ will be small and will add little to nothing, for some other part of the population, $u$ would be negative and will reduce the model $y$, since we assume a normal distribution, we have:   
$E[u] = 0$   
Both $u$ and $x$ are random variables.   
### Assumption of mean independence   
It is assumed that $u$ and $x$ are independent. $E[u\|x] = E[u]$, $\text{Cov}(x, u) = 0$   
Since, $E[u]=0$, therefore $E[u\|x]=0$   
### these assumptions better be helpful if not somewhat realistic   
Yes they are, writing the original equation   

$$
y=\beta_0+\beta_1x+u
\\\implies E[y|x_1]=\beta_0+\beta_1E[x|x_1]
\\\implies E[y|x_1]=\beta_0+\beta_1x_1
$$
Which means the expected value of $y$ given some value of $x$, say $x\_1$.   
For some change in $x$, say $dx$, let the corresponding change in $E(y\|x)$ be $dE[y\|x]$   
The linearity of the equation above implies that:   

$$
\beta_1=\frac{dE[y|x]}{dx}
$$
### Sample estimates of $\beta\_0$ and $\beta\_1$   
If we take a sample of size $n$, then the sample parameters for $\beta\_0$ and $\beta\_1$ are $\hat\beta\_0$ and $\hat\beta\_1$   
The estimation methods to obtain $\hat\beta\_i$ are: Method of moments, Method of least squares (ordinary least square) and maximum likelihood.   
## Method of Moments   
We know that $E[u]=0$ and $y=\beta\_0+\beta\_1x+u$ which implies $u=y-\beta\_0-\beta\_1x$   
Which further implies:   

$$
E[u] = E[y-\beta_0+\beta_1x]=0 \tag1
$$
Since $x$ and $u$ are independent, and $E[u]=0$:    

$$
\text{Cov}(x, u) = E[xu]-E[x]E[u] = 0\\\implies E[xu]=0

$$
This thing above can be written as:   

$$
E[x(y-\beta_0-\beta_1x)]=0 \tag2
$$
Based on the equations $(1)$ and $(2)$ above, we can write the two sample moment methods as follows:   

$$
\text{MM1} = \frac1n\sum_i(y_i-\hat\beta_0-\hat\beta_1x_i)=0
$$

$$
\text{MM2}=\frac1n\sum_i[x_i(y_i-\hat\beta_0-\hat\beta_1x_i)]=0
$$
We have two equations and two unknowns $\hat\beta\_0$ and $\hat\beta\_1$   
Simplifying MM1:   

$$
\text{MM1}=0\\\implies \frac1n(\sum_i^n y_i-\sum_i^n\hat\beta_0-\hat\beta_1\sum_i^n x_i) = 0 \\\implies \bar y-\hat\beta_0-\hat\beta_1\bar x = 0
\\\implies \bar y=\hat\beta_0+\hat\beta_1\bar x
$$
Using the above equation to derive $\hat\beta\_0$:   

$$
\hat\beta_0=\bar y-\hat\beta_1\bar x
$$
Substituting $\hat\beta\_0$ into MM2:   

$$
\text{MM2}=0
\\\implies\frac1n\sum_i[x_i(y_i-\hat\beta_0-\hat\beta_1x_i)]
\\=\frac1n\sum_i[x_i(y_i-\bar y+\hat\beta_1\bar x-\hat\beta_1x_i)]
\\=\frac1n\sum_i[x_i(y_i-\bar y)-\hat\beta_1x_i(x_i-\bar x)]=0
\\\implies \sum_ix_i(y_i-\bar y)=\hat\beta_1\sum_ix_i(x_i-\bar x)
$$
Keep the last step in the back of your mind and bear with me now.   
Now, we know that $\sum(x\_i-\bar x) = \sum x\_i - n\bar x = n\bar x - n\bar x = 0$, therefore, taking $\sum\_i(x\_i-\bar x)(y\_i-\bar y)$:   

$$
\sum_i(x_i-\bar x)(y_i-\bar y) \\= \sum(x_iy_i-x_i\bar y-y_i\bar x+\bar x\bar y) \\= \sum y_i(x_i-\bar x) - \sum \bar y(x_i-\bar x)
\\= \sum (y_ix_i-y_i\bar x) - \bar y\sum (x_i-\bar x)
\\= \sum y_ix_i-\sum y_i\bar x - 0
\\=\sum x_iy_i -n\bar x\bar y
$$
Now, taking $\sum\_ix\_i(y\_i-\bar y)$:   

$$
\sum_ix_i(y_i-\bar y)
\\=\sum x_iy_i-\sum x_i\bar y
\\=\sum x_iy_i -n\bar x\bar y
$$
From the above two things, we see that:   

$$
\sum_ix_i(y_i-\bar y)=\sum_i(x_i-\bar x)(y_i-\bar y)=\sum x_iy_i -n\bar x\bar y
$$
Therefore, we can substitute $\sum\_ix\_i(y\_i-\bar y)$ with $\sum x\_iy\_i -n\bar x\bar y$ in the equation for MM2.   
Similarly we can say $\sum\_ix\_i(x\_i-\bar x) = \sum(x\_i-\bar x)^2$   
If you are wondering why you just wrote "Similarly we can say" and then something incomprehensible by your tiny little smooth brain, try proving that equation written after "Similarly we can say" in the same way as you proved the one before it.   
Again, going back to the last step in the MM2 simplification above, and substituting the insights we made in between:   

$$
\sum_ix_i(y_i-\bar y)=\hat\beta_1\sum_ix_i(x_i-\bar x)
\\\implies \sum_i(x_i-\bar x)(y_i-\bar y)=\hat\beta_1\sum_i(x_i-\bar x)^2
$$
Rearranging,   

$$
\hat\beta_1=\frac{\sum_i(x_i-\bar x)(y_i-\bar y)}{\sum_i(x_i-\bar x)^2}
$$
The term in the numerator is the covariance of x and y and the denominator is the variance of x. Hence we can write,   

$$
\hat\beta_1=\frac{\text{Cov}(x, y)}{\text{Var}(x)}=\frac{\text{Cov}(x, y)}{sd(x)\cdot sd(x)}

$$
Multiplying and dividing the thing above by $sd(y)$.   

$$
\hat\beta_1=\frac{\text{Cov}(x, y)}{sd(x)\cdot sd(y)}\frac{sd(y)}{sd(x)}
$$
OR:   

$$
\hat\beta_1=r_{XY}\frac{sd(y)}{sd(x)}
$$
where $r\_{XY}$ is the pearson coefficient of correlation.   
## (Ordinary) Least Squares Method   
There are n equations $y\_i=\beta\_0+\beta\_1x\_i$   
And only two unknowns $\beta\_0, \beta\_1$   
Still, every observation doesn't lie on an exact straight line, hence every value of $\hat\beta\_i$ has some degree of error.   
Error is just the difference between the predicted value at some $x\_i$ that lies on the straight line with slope $\hat\beta\_1$ and the actual observed value of $y\_i$.   

$$
\hat u_i=y_i-\hat y_i
$$
This error is there for purely statistical reasons. But other than the statistical reason, there is another way to look at this situation. The reasons for these errors in a socioeconomic sense is that they are generated because of unaccounted microfactors.   
The errors generated due to statistical analysis coincide with those generated due to the complex nature of socioeconomic factors.   
No matter, if we see errors as being from statistical analysis or unaccounted socioeconomic factors, our goal shall be to minimize them.   
Errors are written as:   

$$
\hat u_i=(y_i-\hat\beta_0-\hat\beta_1x_i)

$$
If we take the square of the error values in each observation and sum them:   

$$
\sum\hat u_i^2=\sum(y_i-\hat\beta_0-\hat\beta_1x_i)^2
$$
We have to minimize this error term $\sum\hat u\_i^2$   
To minimize it, we take derivatives at zero with respect to our control parameters $\hat\beta\_0$ and $\hat\beta\_1$:   

$$
\frac{d\sum\hat u_i^2}{d\hat\beta_0}=0
\\\implies -2\sum(y_i-\hat\beta_0-\hat\beta_1x_i) = 0

$$
This gives LS1:   

$$
\text{LS1}: \sum(y_i-\hat\beta_0-\hat\beta_1x_i) = 0
$$
To calculate LS2, we take derivative with respect to $\hat\beta\_1$   

$$
\frac{d\sum\hat u_i^2}{d\hat\beta_1}=0
\\\implies 2\sum x_i(y_i-\hat\beta_0-\hat\beta_1x_i) = 0
$$

$$
\text{LS2}: \sum x_i(y_i-\hat\beta_0-\hat\beta_1x_i) = 0
$$
These LS1 and LS2 are similar to MM1 and MM2. There are two equations and we have two unknowns $\hat\beta\_0$ and $\hat\beta\_1$. Using LS1 and LS2, we can calculate the sample control parameters $\hat\beta\_i$, in the exact same manner as Method of Moments.   
### Take home from Method of Moments and Method of Ordinary Least squares   
To calculate the sample regression control parameters $\hat\beta\_i$, first find the correlation coefficient $r\_{XY}$, then find the standard deviations of x and y, then multiply the correlation with the ratio of standard deviations to get $\hat\beta\_1$, like so:   

$$
\hat\beta_1=r_{XY}\frac{sd(y)}{sd(x)}


$$
After calculating $\hat\beta\_1$, calculate $\hat\beta\_0$ like so:   

$$
\hat\beta_0=\bar y-\hat\beta_1\bar x
$$
### Properties of Estimated Coefficients   
1. If a constant k is added to each $y\_i$, then $\hat\beta\_1$ doesn't change, however $\hat\beta\_0$ is increased by k.   
2. If a constant k is added to each $x\_i$, then $\hat\beta\_1$ doesn't change, however $\hat\beta\_0$ is decreased by k.   
3. If a constant k is multiplied to each $y\_i$, then, cov(x,y) is multiplied by k, but var(x) remains same, hence $\hat\beta\_1$ is multiplied by k.   
4. If a constant k is multiplied to each $x\_i$, then, cov(x,y) is multiplied by k, var(x) is multiplied by $k^2$, hence $\hat\beta\_1$ is multiplied by $1/k$.   
   
   
