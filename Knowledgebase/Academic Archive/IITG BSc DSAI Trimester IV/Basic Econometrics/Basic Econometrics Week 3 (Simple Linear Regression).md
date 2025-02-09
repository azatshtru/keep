---
trimester_week: 3
course_name: basic econometrics
---
### Basics of Simple Linear Regression   
Linear regression is used to describe and analyse a potential relation between two random variables.   
The simplest linear regression model equation look like this:   

$$
y_i=\beta_0+\beta_1x_i+u_i
$$
$u\_i$ is an error term, because $x\_i$ and $y\_i$ will never be perfectly linearly correlated due to many outside factors, some deviation from the regression line is expected in $y\_i$.   
$\beta\_i$ are population regression parameters and are impossible to find, hence we try to find the sample regression parameters $\hat\beta\_i$   
To find the sample regression parameters, we assume two things:   
1. The error term $u\_i$'s random variable is independent of $x\_i$'s random variable   
2. The expected value of the error term $u\_i$'s random variable is zero.   
   
The error term for the sample population is calculated as $\hat u\_i=y\_i-\hat y\_i$ where $y\_i$ is the actual sample point and $\hat y\_i$ is the predicted sample point that is calculated using the sample regression parameters and sample x.   
There are two methods to find the sample regression parameters, the method of moments (MM1 and MM2), and the Method of Ordinary Least Sqaures (LS1 and LS2).   
### Important results from Method of Moments and Method of Ordinary Least Squares   

$$
\hat\beta_1=r_{XY}\frac{sd(y)}{sd(x)}
$$

$$
\hat\beta_0=\bar y-\hat\beta_1\bar x
$$
Therefore, to find the sample regression parameters $\hat\beta\_i$, we can first find $\hat\beta\_1$ using the correlation and standard deviations of x and y, and then using the sample means of x and y, we can find $\hat\beta\_0$ as well.   
## Analysis of Estimated (Predicted) Errors   
### Regression Line always passes through the sample mean   
In general the predicted line in Linear Regression will miss the sample points. Let the actual points corresponding to some sample point $x\_i$ be $(x\_i, y\_i)$, and the predicted point using the regression line be $(x\_i,\hat y\_i)$   
When a sample point coincides with the sample mean, i.e. $x\_i = \bar x$, the linear regression equation $\hat y\_i=\hat\beta\_0+\hat\beta\_1 x\_i$ will look like:   

$$
\hat y_i=\hat\beta_0+\hat\beta_1\bar x
$$
What does this give? From an important result in method of moments we know that $\hat\beta\_0 = \bar y - \hat\beta\_1\bar x$, substituting that in the expression above, we get:   

$$
\hat y_i =\bar y-\hat\beta_1\bar x + \hat\beta_1\bar x = \bar y
$$
Which means that if a sample point in x coincides with the sample mean $\bar x$, then that point also coincides with the sample mean of $y\_i$, i.e. $\bar y$.   
 --- 
### $\bar y = \bar{\hat y}$   
We know that $\hat u\_i=y\_i-\hat y\_i$, hence:   

$$
\sum \hat  u_i = \sum (y_i-\hat y_i)
\\ \text{ substituting } \hat y_i \text{ with } \hat\beta_0+\hat\beta_1\bar x
\\ \sum (y_i-\hat\beta_0-\hat\beta_1\bar x)
\\ \text{ futher subsitituting }\hat\beta_0 \text{ with } \bar y-\hat\beta_1\bar x
\\ \sum (y_i-\bar y+\hat\beta_1\bar x-\hat\beta_1\bar x)
\\= \sum (y_i-\bar y) = n\bar y-n\bar y= 0
$$
The above analysis indicates that, like the mean of errors in the population is zero, the mean of all estimated (predicted) errors in the sample, i.e. $\hat u\_i$ is also zero:   

$$
\bar {\hat u_i} = 0
$$
 and since $\hat u_i=y_i-\hat y_i$, we can say:   

$$
\bar y_i-\bar{\hat y_i} = 0
\\\implies
\bar y_i=\bar{\hat y_i}

$$
which means that in a sample, the mean of all the predicted y is equal to the mean of all the observed y   
 --- 
### Predicted errors and predicted y values have zero covariance   

$$
\text{Cov}(x_i, \hat u_i)
\\=\sum x_i\hat u_i-\bar x\bar{\hat u}
\\=\sum x_i\hat u_i\qquad\text{because }\bar{\hat u}=0
\\=\sum[x_i(y_i-\hat\beta_0-\hat\beta_1x_i)]
$$
From MM2 and LS2, we know that $\sum[x\_i(y\_i-\hat\beta\_0-\hat\beta\_1x\_i)] = 0$, which means that:   

$$
\sum x_i\hat u_i = 0
$$
Now, using the above result, we can find the covariance between $\hat u\_i$ and $\hat y\_i$:   

$$
\sum\hat u_i\hat y_i
\\=\sum\hat u_i(\hat\beta_0+\hat\beta_1 x_i)
\\=\hat\beta_0\sum\hat u_i+\hat\beta_1 \sum\hat u_ix_i
$$
From the previous section, we know that $\sum\hat u\_i=0$, and we have also assumed that error $\hat u\_i$ and $x\_i$ are independent, which means that covariance of x and u is zero, which implies that expectation of x and u is zero which, using MM2 or LS2, further implies that $\sum\hat u\_ix\_i = 0$ as shown above.   
Therefore, $\hat\beta\_0\sum\hat u\_i+\hat\beta\_1 \sum\hat u\_ix\_i = 0$   
which means:   

$$
\sum\hat u_i\hat y_i = 0
$$
Now, putting this thing in covariance of estimated u and estimated y, we get:   

$$
\text{Cov}(\hat y_i, \hat u_i)
\\=\sum \hat y_i\hat u_i-\bar{\hat y}\bar{\hat u}
\\=\sum \hat y_i\hat u_i\qquad\text{because }\bar{\hat u}=0
\\\text{Substituting }\sum\hat u_i\hat y_i = 0 \text{ from above}
\\\text{Cov}(\hat y_i, \hat u_i) = 0

$$
### Key insights from Analysis of Estimated (Predicted) Errors   
1. The mean of estimated errors is zero.   
2. Like population errors, estimated errors are uncorrelated with x.   
3. Estimated errors are uncorrelated with estimated y.   
   
Since we have used the results obtained from Method of moments and Ordinary Least squares while analyzing estimated errors, we can say that MM and LS minimize the effect of errors since the mean of estimated errors is zero.   
## Predictive Power of Model   
We know that the difference of actual observed sample point $y\_i$ and predicted $\hat y\_i$ is the estimated error $\hat u\_i$, which can also be written as:   

$$
y_i = \hat y_i + \hat u_i
$$
If the regression model is good, then variance of $y\_i$ would be mostly explained by the variance of $\hat y\_i$ and the effect of $\hat u\_i$ would be very less.   
To quantify the goodness of the model, we define three things:   
1. Total sum of squares TSS = $\sum (y\_i-\bar y)^2$   
2. Estimated sum of squares ESS = $\sum (\hat y\_i-\bar y)^2$   
3. Residual sum of squares RSS = $\sum (y\_i-\hat y\_i)^2$   
   
Total sum of squares is the term that corresponds to the variance of observed y, dividing TSS by n-1 will give the variance of observed y. ESS is the variance term corresponding to the estimated y, dividing ESS by n-1 will give the variance of estimated y (since \bar y\_i=\bar{\hat y\_i}).   
Residual sum of squares corresponds to the remaining residual variance.   
We have:   

$$
\text{TSS = ESS + RSS}
$$
### Proof of TSS = ESS + RSS   

$$
\text{TSS} = \sum(y_i-\bar y)^2 = \sum((y_i-\hat y_i) + (\hat y_i-\bar y))^2

$$

$$
= \sum(y_i-\hat y_i)^2 + \sum(\hat y_i-\bar y)^2+2\sum((y_i-\hat y_i)(\hat y_i-\bar y))
\\= \sum(y_i-\hat y_i)^2 + \sum(\hat y_i-\bar y)^2+2\sum((\hat u_i-0)(\hat y_i-\bar y))
\\= \sum(y_i-\hat y_i)^2 + \sum(\hat y_i-\bar y)^2+2\text{Cov}(\hat u_i,\hat y_i)(n-1)
$$
Since the Covariance between u and y is zero, the last covariance term is zero:   

$$
\sum(y_i-\bar y)^2 = \sum(y_i-\hat y_i)^2 + \sum(\hat y_i-\bar y)^2
$$
Since $\bar y = \bar{\hat y}$,   

$$
\sum(y_i-\bar y)^2 = \sum(y_i-\hat y_i)^2 + \sum(\hat y_i-\bar{\hat y})^2
$$
Or:   

$$
\text{TSS = ESS + RSS}
$$
## A Goodness of Fit in context of Regression, but like not THE Goodness of Fit   
**Goodness of fit in context of regression** is defined as:   

$$
R^2=\frac{ESS}{TSS}

$$
This number is between 0 and 1. The better the predicted coefficients, the closer this number is to 1.   
The term "goodness of fit" is used in other places in statistics. Hence this $R^2$ is called the "goodness of fit in context of regression".   
It can be shown and derived that $R^2$ is equal to the correlation coefficient between predicted y of the sample ($\hat y\_i$) and actual observed y of the sample ($y\_i$)   

$$
R^2 = r^2_{y,\hat y}
$$
In most natural sciences, one looks for a very high $R^2$, above 0.8, but in social sciences, due to data being more random, the value can drop down to 0.3 or 0.4, however a value this lower for social sciences does not necessarily mean that the regression model is useless.   
## Assumptions, Assumptions and Assumptions for simple linear regression model   
### The dependent variable y, the independent variable x and the error term u are linearly related by the equation $y_i=\beta_0+\beta_1x_i+u_i$ for the population   
### For a sample, a $\hat y\_i$ is estimated, $\hat y\_i=\hat\beta\_0+\hat\beta\_1x\_i$, where $\hat\beta\_i$ are the estimated regression parameters different from $\beta\_i$   
The difference between the estimated y and observed y is called the estimated error, the estimated error $\hat u\_i$ is different from observed error $u\_i$ as observed error is obtained from socioeconomic randomness, while the estimated error is the result of differences of y between estimated linear regression line and the population regression line.   
### The sample points have different values: Cov(x, y) is never zero and Var(x) is never zero, otherwise the sample $\beta\_1$ would be undefined.   
### x is independent of u, which means only x affects y, x is causal of y, not u.   
### The variance of error u is same given any sample or any subset of the population.   
### Covariance(u\_i, u\_j) and $E[u\_i, u\_j]$ of two different errors in two different sample points i and j is zero, errors of two different sample points are always uncorrelated.   
## Gauss Markov Theorem   
The parameters $\hat\beta\_i$ obtained using Method of Moments or Ordinary Least Square Method are **BLUE, or **Best Linear Unbiased Estimators.   
defining $s\_i$   

$$
s_i = \frac{x_i-\bar x}{\sum(x_i-\bar x)^2}
$$
### Proof of linearity of Ordinary Least Square estimators   
To prove that $\hat\beta\_1$ obtained by MM and LS is Linear, we write:   

$$
\hat\beta_1=\frac{\sum_i(x_i-\bar x)(y_i-\bar y)}{\sum_i(x_i-\bar x)^2}
$$
The above equation is derived using MM or LS in the previous week.   
It was also derived that $\sum\_i(x\_i-\bar x)(y\_i-\bar y) = \sum\_i(x\_i-\bar x)y\_i$   
Hence $\hat\beta\_1=\frac{\sum\_i(x\_i-\bar x)(y\_i-\bar y)}{\sum\_i(x\_i-\bar x)^2}$ can also be written as:   

$$
\hat\beta_1=\frac{\sum_i(x_i-\bar x)y_i}{\sum_i(x_i-\bar x)^2}=\sum s_iy_i
$$
which means that $\hat\beta\_1$ is a linear combination of $y\_i$   
$s\_i$ is a function of $x$   
Linearity reduced computation costs.   
### Proof of Unbiasedness of Ordinary Least Square estimator   
We know that:   

$$
\hat\beta_1=\sum s_iy_i=\sum s_i(\beta_0+\beta_1x_i+u_i)
\\=\beta_0\sum s_i+\beta_1\sum s_ix_i+\sum s_iu_i
$$
To further simplify this, we need to find out three things:   
1. $\sum s\_i$   
2. $\sum s\_ix\_i$   
3. $\sum s\_iu\_i$   
   
   
- Finding $\sum s\_i$:   

$$
\sum s_i =  \frac{\sum(x_i-\bar x)}{\sum(x_i-\bar x)^2}
$$
   
We know that $\sum(x\_i-\bar x)=n\bar x -n\bar x = 0$, therefore:   

$$
\sum s_i = 0
$$
   
- Finding $\sum s\_ix\_i$:   
   
From the derivations in previous week, we know that $\sum\_i(x\_i-\bar x)^2 = \sum\_i(x\_i-\bar x)x\_i$, using this we can write:   

$$
s_i = \frac{x_i-\bar x}{\sum(x_i-\bar x)^2}\\\implies
s_ix_i = \frac{(x_i-\bar x)x_i}{\sum(x_i-\bar x)^2}=\frac{(x_i-\bar x)^2}{\sum(x_i-\bar x)^2}

$$
This further implies that summing over $s\_ix\_i$ will give:   

$$
\sum s_ix_i =\frac{\sum(x_i-\bar x)^2}{\sum(x_i-\bar x)^2} = 1
$$
   
Using these two findings above, we can write the first equation of the proof as:   

$$
\hat\beta_1=\beta_1+\sum s_iu_i
$$
Since $x$ and $u$ are independent, any function of $x$ is independent with $u$, since $s\_i$ is also a function of $x$, it is also independent from $u$, we get:   

$$
E[\hat\beta_1]= \beta_1+\sum E[s_i]E[u_i]
$$
We know that $E[u\_i] = 0$, hence:   

$$
E[\hat\beta_1]= \beta_1
$$
Which means that sample parameter $\hat\beta\_1$ is an unbiased estimator of the population parameter $\beta\_1$   
### Unbiasedness of the intercept term   
We know that at the points where mean coincides with the sample points, hence we have:   

$$
\hat\beta_0 = \bar y -\hat\beta_1\bar x
$$
Taking $\bar y$ as a point near the population regression line, but not on top of it, we can say that at $x\_i=\bar x$, the value of population regression line is $\bar y+\bar u$   

$$
\hat\beta_0 = \beta_0+\beta_1\bar x+\bar u -\hat\beta_1\bar x
\\=\beta_0+\bar u+\bar x(\beta_1 -\hat\beta_1)
$$
On taking expectation both sides we get:   

$$
E[\hat\beta_0] = \beta_0+E[\bar u]+\bar xE[\beta_1 -\hat\beta_1]
\\=\beta_0+E[\bar u]+\bar x(\beta_1 -E[\hat\beta_1])
\\=\beta_0
$$
Hence, sample intercept is also an unbiased estimator of population intercept in simple linear regression.   
## Variance of $\hat\beta\_1$   
We know that:   

$$
\hat\beta_1=\beta_1+\sum s_iu_i
\\\implies \text{Var}(\hat\beta_1)=\text{Var}(\sum s_iu_i)
$$
From the equation of variance of sums of n random variables which is $Var(\sum a\_iX\_i)=\sum a\_i^2Var(X\_i)+2\sum\_{i\neq j} a\_ia\_jCov(X\_i,X\_j)$, we can write:   

$$
Var(\sum s_iu_i)=\sum Var(s_iu_i)+2\sum_{i\neq j}Cov(s_iu_i,s_iu_j)
$$
Now, handling the covariance part   

$$
Cov(s_iu_i, s_ju_j) = E[s_iu_is_ju_j]-E[s_iu_i]E[s_ju_j]

$$
Since $s\_i$ is a function of x and u and x are independent, therefore s and u are independent, so we can write:   

$$
Cov(s_iu_i, s_ju_j) =E[s_is_j]E[u_iu_j] - E[s_i]E[s_j]E[u_i]E[u_j] = 0
$$
Because $E[u\_iu\_j] = E[u\_i] = 0$   
Therefore,    

$$
\text{Var}(\hat\beta_1)=Var(\sum s_iu_i)=\sum Var(s_iu_i)
$$
Analyzing $Var(s\_iu\_i)$:   

$$
Var(s_iu_i) = E[s_i^2u_i^2]-E[s_iu_i]^2

$$
Since $s\_i$ and $u\_i$ are independent, $E[s\_i^2u\_i^2] = E[s\_i^2]E[u\_i^2]$   
And because $E[u\_i] = 0$, therefore, $E[s\_iu\_i] = E[s\_i]E[u\_i] = 0$, hence the second term is zero. We can write:   

$$
Var(s_iu_i) = E[s_i^2]E[u_i^2]
$$
Now, $Var(u\_i)=E[u\_i^2]-E[u\_i]^2=E[u\_i^2]$, i.e. the variance of error is same as the expectation of square of error.    
Since we assumed that the variance of error is same in each sample, therefore we can treat the variance of $u\_i$ as a constant $\sigma\_{u}^2$   
Based on this, we can write the above equation as:   

$$
Var(s_iu_i) =E[s_i^2]\sigma_{u}^2
\\\implies\text{Var}(\hat\beta_1)=Var(\sum s_iu_i)=\sum E[s_i^2]\sigma_{u}^2
\\=\sigma_{u}^2\sum E[s_i^2]=\sigma_{u}^2 E[\sum s_i^2]
$$
Till now, we are at:   

$$
Var(s_iu_i) =\sigma_{u}^2 E[\sum s_i^2]
$$
To solve further, we need to find $E[\sum s\_i^2]$, we know that:   

$$
s_i = \frac{x_i-\bar x}{\sum(x_i-\bar x)^2}
$$
which implies that:   

$$
E[\sum s_i^2] = E\left[\frac{\sum(x_i-\bar x)^2}{(\sum(x_i-\bar x)^2)^2}\right]
\\=E\left[\frac{1}{\sum(x_i-\bar x)^2}\right]
$$
Since, $\sum(x\_i-\bar x)^2$ is just the sample variance of $x\_i$ multiplied with sample size, i.e. $\sum(x\_i-\bar x)^2=ns\_x^2$, which is a constant, hence we can write:   

$$
E[\sum s_i^2] = E\left[\frac{1}{\sum(x_i-\bar x)^2}\right]
\\=\frac1{\sum(x_i-\bar x)^2}E[1]
\\=\frac1{\sum(x_i-\bar x)^2}
$$
Substituting this value of $E[\sum s\_i^2]$ into $Var(s\_i)$   

$$
Var(s_iu_i) =\sigma_{u}^2 E[\sum s_i^2]
\\=\frac{\sigma_{u}^2}{\sum(x_i-\bar x)^2}
$$
Therefore, after all of this adventure we can say that:   

$$
\text{Var}(\hat\beta_1)=\frac{\sigma_{u}^2}{\sum(x_i-\bar x)^2}=\frac{\sigma_{u}^2}{ns_x^2}
$$
Variance of $\hat\beta\_1$ is directly proportional to the variance of error. If error has higher variance, then $\hat\beta\_1$ also has higher variance from the original $\beta\_1$.   
Similarly, $\hat\beta\_1$ is inversely proportional to the sample variance of $x\_i$, hence if the variance of $x\_i$ is larger in the population, then it will be larger in its samples, which will result in higher sample variance of $x\_i$ and lower variance of $\hat\beta\_1$, more variance in the sample, more precise the slope estimator.   
This coincides with the socioeconomic reason, that we need to take samples from a varied population to reduce biases.   
### Variance of $\hat\beta\_0$   

$$
\text{Var}(\hat{\beta}_0) = \frac{\sigma^2_u\sum x_i^2}{n\sum (x_i-\bar x)^2}
$$
Where is the proof? I don't know :(   
## The minimum variance property   
The minimum variance property says that the estimators obtained from OLS have the least variance among all the possible unbiased estimators.   
Given any other unbiased estimator, one can show that its variance is higher than OLS estimators.   
The OLS estimator $\hat\beta\_0$ in terms of $s\_i$ is $\sum s\_iy\_i$   
Assume a general unbiased estimator for $\beta\_0$   

$$
\tilde\beta = \sum c_iy_i
$$
   
