---
course_name: basic econometrics
trimester_week: 1
---

### Purely random component of a random variable   
In the population, every value in the random variable has a value $x\_i$. Now, $x\_i$ can be written as a sum of mean of population $\mu$ and a purely random component $u\_i$.   

$$
x_i=\mu+u_i
$$
Finding the expected value of $x\_i$ yields the population mean.   

$$
E[x_i]=E[\mu]+E[u_i]\\\mu=\mu+E[u_i]
$$
The above equation implies that expected value over purely random component of $x\_i$ must be zero. This also makes sense intuitively because some values in the distribution are more than the mean, which sum are less than the mean, so they cancel each other out and the purely random component vanishes.   
## Estimators and Estimates   
We don't know the population's probability distribution function in advanced, hence we take samples from the population and try to estimate the characteristics by using estimators.   
Different statistical measures have different estimators. For example, sample mean is an estimator for the entire population's mean.   
Estimators yield estimates, an estimate is based on the random values of the population, since the population is based on a random variable, the estimates taken from the population are also a kind of random variable with their own mean and variance, etc.   
A good estimator is **unbiased** and **efficient.**   
### Unbiasedness   
The expected value over all estimates must be equal to the actual statistical measure.   
Here is a proof that the sample mean is an unbiased estimator of population mean:   
Let sample mean be $\bar x$   
Now, $\bar x = \sum\_{i=1}^nx\_i/n$ for some sample of size n.   
If we write $x\_i$ as mean plus its purely random component, we get:   

$$
\bar x = \frac1n\sum_{i=1}^n(\mu+u_i)\\
=\frac1n(\sum_{i=1}^n \mu+\sum_{i=1}^nu_i)\\
=\frac1n(n\mu+\sum u_i)\\
=\mu+\bar u
$$
For a single sample, this $\bar u$, which is the average of all the pure random components in the sample, will be non-zero.   
If we take the sample mean repeatedly, we will get different values, since the samples everytime are random, the value of sample mean everytime are different. Hence, sample mean itself can be treated as a random variable with an expectation and variance, etc. The nonzero $\bar u$ obtained in the analysis above is just the pure random component of one of the sample mean random variable values.   
The sample mean random variable's mean is the same as original random variable's mean, but the variance of sample mean random variable is less than the original random variable because while calculating sample mean, the pure random components of the original random variables usually cancel each other out and the distribution is more centralized towards the mean.   
If we take expected value of sample mean random variable, then:   

$$
E[\bar x]=E[\mu]+E[\bar u]\\
=\mu+0
$$
The expected values of the pure random component of all the sample means is zero because like the original random variable $X$, the sample mean random variable $\bar X$ is also distributed equally around the mean, hence if infinite samples are taken, some will have pure random component less than mean, but some will have more, effectively cancelling each other out.   
This completes the proof and shows that the expected value over all the sample means equals the expected value over the population, hence proving that the sample mean is an unbiased estimator.   
### Efficiency   
> Keynes once said, in the long run we are all dead.   

If there are two estimators, and one estimator yields estimates that are more nearer to the mean, then that estimator is more efficient.   
