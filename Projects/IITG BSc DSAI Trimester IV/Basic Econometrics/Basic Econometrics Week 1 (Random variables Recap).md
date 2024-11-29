---
trimester_week: 1
course_name: basic econometrics
---

## Introduction   
Econometric (literally, measurement of financial/socioeconomic data, stocks and stuff)   
There are three types of econometric data:   
1. Cross Section (Different streams of data at the same time) For example, comparing the GDPs of different countries in a single year.
2. Time Series (Comparing the GDP of a single country over a time period)   
3. Panel Structure (Combines Cross section with Time Series) For example, Comparing data of different countries in 100 years.
   
This course is mostly about Cross Section Data, but the techniques can be extended to other forms of data as well.   
   
   
Econometrics is a systematic study of economic/social science data to establish causality.   
For example, the economic theory predicts that two events A, B cause another event C and upto what extent is the causality true.   
All tools of this course are developed from statistical analysis.   
 --- 
## Why study social sciences as random data?   
Social sciences generate a lot of data.   
- GDP of a country   
- Stock market indices   
- Number of people in a country below poverty   
- Average age of marriage   
   
The data is generated through a very complex socioeconomic process, so much so that it can be treated as **random** data.   
## Random variables   
A random variable is a function that depends on some associated probability. (for example, number of heads)   
A random variable has a distribution which tells the probability(y-axis) of a particular value of random variable(x-axis).   
All probabilities are nonnegative fractions and sum of all probabilities is zero.   
Random variables can be continuous or discrete:   
- A discrete RV is characterized by its Probability Mass Function. This calculates the probability of a particular value of the random variable.   

$$
f(x_i) = P(X=x_i)
$$

$$
 \sum_if(x_i) = 1 
$$
- A continuous RV is characterized by its Probability density function. For a continuous random variable, it is not possible to calculate the probability for a single value because the random variable can take infinite values and probability of a single value is very close to zero, rather, probabilities of intervals of random variable values are calculated.   

$$
\int_a^bf(x)dx=1
$$
   
### Binomial random variable   
Binomial Random variable is a discrete RV that is characterized by the PMF:   

$$
f(x) = {n \choose x} p^xq^{n-x}
$$
where p is the probability of success, and q is the probability of failure.   
If $p+q = 1$, then by binomial theorem:   

$$
\sum_{x=0}^n{n \choose x} p^xq^{n-x} = (p+q)^n = 1^n = 1

$$
   
### Uniform random variable   
Uniform random variable is a continuous random variable characterized by the PDF:   

$$
f(x)=\frac1{b-a}
$$
And sum of uniform random variable outside (a, b) is zero, inside (a, b) interval:   

$$
\int_a^b\frac1{b-a} = \frac{[x]}{b-a}|_a^b = 1
$$
 --- 
## Expected value of a random variable   
1. For discrete random variable:   

$$
\mu = E[X] = \sum_ix_if(x_i)
$$
   
where $f(x\_i)$ is PMF of the discrete RV.   
2.  For continuous random variable:   

$$
\mu = E[X] = \int xf(x)dx
$$
where $f(x)$ is the PDF.   
   
### Properties of expected values   
1. For any constant $c: E[c] = c$   
2. For any constants $a, b: E[aX+b] = aE[X] + b$   
3. For random variables $X\_1, X\_2,…, X\_n$ and constants $a\_1, a\_2, …, a\_n$:   

$$
E[\sum_ia_iX_i] = \sum_ia_iE[X_i]

$$
 --- 
   
## Variance   
How far is data distributed from mean/expectation. For example, how far are the incomes of people distributed from the average income.   

$$
\sigma^2 = E[(X-\mu)^2]=E[X^2]-\mu^2

$$
> mean of the square minus square of the mean.   

### Standard deviation   
Standard deviation is the square root of variance.   
### Properties of Variance   
1. $Var(c) = 0$ if $c$ is a constant   
2. For constants $a, b: Var(aX+b) = a^2Var(X)$   
3. For any constant a, b: standard deviation $sd(aX+b)=\|a\|sd(X)$   
4. For two random variables $X, Y$:   

$$
Var(aX+bY)=???????
$$
   
### Standardizing value of Random Variable   

$$
Z=\frac{X-\mu}{\sigma}
$$
Z is a standardized random variable of random variable X, $\mu, \sigma$ respectively are expectation and standard deviation of X.   

$$
E[Z] = 0, Var(Z) = 1
$$
Every standardized RV has mean zero and variance 1.   
Prove the above for practice.   
Find the mean and variance of other random variables (binomial, uniform) for practice.   
 --- 
## Higher order moments of Random variable   
### Central moment   
$n^{th}$ order central moment of a random variable is defined as:   

$$
E[(X-\mu)^n]

$$
- Variance is a second order central moment   
   
### Raw moment   
$n^{th}$ order raw moment of a random variable is defined as:   

$$
E[X^n]
$$
 --- 
## Cumulative Probability   
Cumulative probability is the probability that the value of a random variable is less than or equal to a certain number.   
For discrete:   

$$
F(a) = P(X\leq a) = \sum_{i=0}^nf(x_i), where\space x_n=a

$$
For continuous:   

$$
F(a) = P(X\leq a) = \int_{a_0}^af(x)dx
$$
### Medians and Quartiles   
Median is a number $m$ such that $F(m)=0.5$   
First quartile is a number $q\_1$ such that $F(q\_1)=0.25$   
   
### Median for Uniform Random Variable   

$$
\int_a^m\frac{dx}{b-a}=\frac{m-a}{b-a}=\frac12\implies m=\frac{a+b}2
$$
The median of Uniform random variable is equal to the expectation of uniform random variable.   
If two random variables exhibit the property that their expectation and median are equal, then those random variable distributions are called ***symmetric distributions**.*   
   
 --- 
## Joint Distribution   
Two variables can be dependent on each other.   
  For example:   
  X = hours studied, Y = grade received   
  X and Y are two random variables   
  Some questions can be asked based on this:   
  1. **P(grade A \| 4 hours studied) = ?** which means what is the probability of receiving grade A if someone studied for 8 hours.   
  2. **P(4 hours studied \| grade A) = ?** which means what is the probability that someone studied for 4 hours given that they received grade A.   
For discrete, f(x, y) is the joint PMF, such that:   

$$
f(x, y) = P(X=x, Y=y)
$$

$$
\sum_x\sum_yf(x, y) = 1
$$
For continuous, f(x, y) is the joint PDF, such that:   

$$
\int_x\int_yf(x, y)dxdy = 1
$$
### Marginal distribution   

$$
f(x)=\sum_yf(x,y)=\int_yf(x,y)dy
$$
Remember: for marginal distribution of x, sum over y and for marginal distribution of y, sum over x. Always sum over the "irrelevant" variable.   
### Mean of Product of two Random Variables   
It is important to sum over all the x values as well as all the y values to compute complete expectation.   

$$
E[XY] = \begin{cases}\sum_x\sum_yxyf(x,y):discrete\\\int_x\int_yxyf(x,y)dx:continuous\end{cases}
$$
### Conditional probability   
Conditional probability of X, given some value of Y is:   

$$
f(x|y)=P(X=x|Y=y)=\frac{P(X=x, Y=y)}{P(Y=y)}=\frac{f(x,y)}{f(y)}
$$
i.e. conditional probability is obtained by dividing the joint PDF by marginal PDF.   
### Independent Random variables   
If two variables are independent, then their conditional probabilities are equal to their respective probabilities, i.e.:   

$$
f(x|y) = f(x) \implies \frac{f(x,y)}{f(y)}=f(x)\implies f(x,y)=f(x)*f(y)
$$
Which means if the marginal probabilities can be multiplied to get the joint probability, then the two random variables are independent.   
### Unconditional Mean and Variance   
Unconditional mean and variances are just the regular mean and variances of Random variables, but in the Joint context.   
For example, the unconditional mean of a Random variable X in a jointly distributed context would be:   

$$
\mu_x=\sum xf(x):discrete=\int xf(x)dx: continuous
$$
Similar for variance:   

$$
\sigma_x^2=E[(X-\mu_x)^2]
$$
### Conditional Mean and Variance   
Conditional Mean for X can be defined as:   

$$
E[X|Y=y]=\sum_xxf(x|Y=y):discrete=\int xf(x|Y=y)dx:continuous
$$
Conditional Variance for X can be defined as:   

$$
Var(X|Y=y)=E[(X-E(X|Y=y))^2|Y=y]
$$
### Properties of conditional mean and variance   
1. Let $X$ be an RV and $f(x)$ be a function of $X$, then $E[f(X)\|X=x] = f(x)$   
2. Let $X$ and $Y$ be two RVs, and $f(x)$ and $g(x)$ be two functions of $X$, then $E[f(X)Y+g(X)|X] = f(X)*E[Y|X] + g(X)$   
   
   
### Law of Iterated Expectations   
$E[Y\|X]$ is a function of $X$. Since $X$ is random, $E[Y\|X]$ is also a random variable and $E[Y\|X]$ has its own distribution and expected value.   
The law of iterated expectations implies that:   

$$
E[E[Y|X]] = E[Y]
$$
### Expectation of Product of Independent Random Variables   
Since, $f(x, y) = f(x)*f(y)$ for independent variables,   

$$
E[XY]=\sum_x\sum_yxyf(x, y)\\=\sum_x\sum_yxyf(x)f(y)\\=\sum_xxf(x)\sum_yyf(y)\\=E[X]E[Y]
$$
### Covariance   
Let $X$ and $Y$ be two random variables with expectations $\mu\_x$ and $\mu\_y$ respectively.   
The covariance is defined as:   

$$
\sigma_{XY}=Cov(XY)=E[(X-\mu_x)(Y-\mu_y)]\\=E[XY]-\mu_x\mu_y
$$
### Properties of Covariance   
1. If two RVs are independent then, covariance is zero.   
2. If $E[Y\|X]$ is zero, then $E[Y]$ is zero and $X, Y$ are uncorrelated.   
3. $Cov(a\_1X+b\_1, a\_2Y+b\_2)=a\_1a\_2Cov(X,Y)$   
4. $\|Cov(X, Y)\| \leq \sigma\_X\sigma\_Y$ (Cauchy-Schwartz Inequality) where $\sigma\_X$ and $\sigma\_Y$ are standard deviations.    
   
### Linear Correlation   
Pearson Correlation is defined as:   

$$
\rho = \frac{\sigma_{XY}}{\sigma_X\sigma_Y}
$$
Correlation is the measure of linear association between X and Y. If two variables are highly nonlinearly associated (eg. parabolic, cubic), then the correlation would be close to zero.   
There can be cases sometimes, where out of coincidence or other hidden factors, two variables can show a high value of $\rho$ indicating that they are linearly correlated, for example, rain fall in New York might increase at the same time and rate as Divorce Rates in India, but the two might not be necessarily correlated. Such type of correlation is called spurious correlation.   
It is always between -1 and 1, i.e. $-1 \leq \rho \leq 1$.   
Covariance can be positive or negative.   
For every linear function of the Random Variables, covariance remains same, only the sign changes:   

$$
Corr(a_1X+b_1, a_2Y+b_2)=\begin{cases}Corr(X,Y) & if\space a_1*a_2>0\\-Corr(X,Y) & if\space a_1*a_2<0\end{cases}
$$
### Variance of Sums of Random Variables   
Derive this for practice:   

$$
Var(aX\pm bY) = a^2Var(X)+b^2Var(Y)\pm 2abCov(X,Y)
$$

$$
Var(\sum a_iX_i)=\sum a_i^2Var(X_i)+2\sum_{i\neq j} a_ia_jCov(X_i,X_j)
$$
## Some Important Distributions   
### Gaussian (Normal) Distribution   
Define a random variable $Z = (X-\mu)/\sigma$   
A normal distribution function is then defined as:   

$$
f(x) = \frac1{\sqrt{2\pi\sigma^2}}e^{-z^2/2}
$$
where $z = (x-\mu)/\sigma$   
The normal distribution is denoted by $X \sim N(\mu, \sigma)$   
Properties of Normal distribution:   
1. If $X \sim N(\mu, \sigma^2)$ and $Y=aX+b$, then: $Y \sim N(a\mu+b, a^2\sigma^2)$   
2. For normal distribution, zero correlation implies independence, i.e. if two jointly distributed RVs have zero correlation, then the product of their PMFs/PDFs is equal to their Joint PMFs/PDFs.   
3. Any linear combination of Independent and Identically Distributed (same mean and same variance) RVs is also normal.   
   
### Standard Normal Distribution (Z Distribution)   
A **Standard Normal Distribution** is $Z \sim N(0, 1)$, the Area under Standard Normal Distribution is 1.   
Cumulative Distribution Function of Standard Normal Distribution is:   

$$
F(z) = \Phi(z) = P(Z \leq z) = \int_{-\infty}^z\frac1{\sqrt{2\pi}}e^{\frac{-z^2}{2}}
$$
Properties of Standard Normal Distribution:   
1. The area under standard normal distribution is 1.   
2. $P(Z > z) = 1 - \Phi(z)$   
3. $P(Z > z) = P(Z < -z)$   
4. $(a \leq Z \leq b) = \Phi(b)-\Phi(a)$   
   
If the value of $z$ is less than -3 or greater than 3, then that value is called an outlier value as the probability of that value is very less.   
To calculate the area under any normal distribution, standardize the given values in normal distribution by subtracting by mean and dividing by standard deviation and look at the corresponding values in the area table for Z-distribution.   
### $\chi^2$ Distribution   
Chi square distribution of n degrees is the sum of squares of n Z-Distribution functions.   

$$
\chi_n^2 = \sum_{i=1}^n(\frac{Z_i-\mu_i}{\sigma_i})^2
$$
A random variable following Chi distribution is always positive.   
$\chi$ distribution is positively skewed, especially for lower degrees.   
### t-Distribution   
If there is a z-Distribution $Z$ and a $\chi$ distribution of degree k, then the t-Distribution is defined as:   

$$
t_k = \frac z{\sqrt{\frac{\chi_k^2}{k}}}
$$
Shape of t-Distribution is almost the same as z-Distribution, as k increases, t-Distribution approaches z.   
### F Distribution   
Fischer/Snedecor's distribution of degree of freedom n, m is defined as:   

$$
F_{n,m}=\frac{\chi_n^2/n}{\chi_m^2/m}
$$
   
Online Applets for visualizing various distributions:   
- homepage.divms.uiowa.edu/\_mbognar/applets/normal.html   
- homepage.divms.uiowa.edu/\_mbognar/applets/t.html   
- homepage.divms.uiowa.edu/\_mbognar/applets/f.html   
- homepage.divms.uiowa.edu/\_mbognar/applets/chisq.html   
   
   
