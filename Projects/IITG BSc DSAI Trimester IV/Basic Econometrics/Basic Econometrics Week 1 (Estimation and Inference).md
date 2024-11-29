---
trimester_week: 1
course_name: basic econometrics
---

## Population   
Population is the collection (or universe) of all possible data points. Probability distribution relates to the population, the characteristics like mean and variance of a distribution are related to the population. These numbers (mean, variance, etc.) are known as parameters.   
## Sample   
In reality, it is near impossible to count all possible data points (known as census).   
We usually take samples through a survey of subset of population (or universe).   
Since we are not observing all the population at all times, we will infer the parameters (mean, variance) through the survey of the subset of population.   
### Sample statistics   
Suppose we collect a sample ${x\_1, x\_2,\dots , x\_n}$   
For this sample one can calculate:   
1. Sample mean $\bar{x}=\sum\_i x\_i/n$   
2. Sample variance $\sigma^2=\sum\_i(x\_i-\bar{x})^2/n$   
3. Sample covariance $\sigma\_{XY}=1/n\sum\_i(x\_i-\bar x)(y\_i-\bar y)$   
4. Sample correlation using Sample standard deviation and standard covariance.   
   
### Dual Life of a sample (Sample points as random variables)   
Once the sample is taken we have specific values of the sample statistics.   
Although, before the sample is taken, the potential values can be thought of as a random variable.   
If population has M members and we are taking a sample of size n, then the first sample point $x\_1$ can be any of the M members.   
The first sample point $x\_1$ can be thought of as a single value of random variable $X\_1$   
$X\_1$ can take any value from the sample with some probability.   
Prior to taking the sample, each potential sample point then is an RV with a mean, variance etc.   
If X follows a population distribution $f()$ with mean $\mu$ and variance $\sigma^2$, each potential sample member will also follow the same.   
### (Simple) Random Sampling   
Simple random sampling is a method of forming a sample from the population.   
Assume that each sample point has the same probability of getting chosen and each sample point is independent and identically distributed.   
Then one can start choosing n sample members from the population of size M.   
## Estimator and Estimates   
Using the sample statistics, we try to get a measure of population parameters.   
An **estimator** is a function for getting the measure.   
An **estimate** is a numerical value we get from the sample.   
Estimates can change from sample to sample.   
Estimators are random since the potential values are also random.   
### Unbiased estimator   
Suppose we want to estimate the population parameter $\theta$, (theta could be population mean $\mu$, etc.) from the sample statistic $W$ (W could be sample mean $\bar x$).   
If one takes the sample again and again, the value of $W$ would sometimes be greater than, less than or equal to $\theta$ depending on the sample. But one can be sure that the average value of all the sample statistic $W$ would be very close (almost equal) to the population parameter $\theta$.   
$W$ is called an unbiased estimator if $E[W]=\theta$, i.e. mean of $W$ is equal to $\theta$.   
The bias of an estimator is calculated as $Bias(W) = E[W] - \theta$   
The bias in case of an unbiased estimator as described above is zero.   
### Calculating Sample mean's mean and variance   

$$
\bar X = \sum \frac{X_i}{n}\\\implies E[\bar X]=\frac1n\sum E[X_i]\\\implies E[\bar X] = \frac1n[E[X_1]+\dots+E[X_n]]\\\implies E[\bar X] = \frac1nn\mu=\mu
$$

$$
Var(\bar X) = Var(\frac{\sum X_i}{n})\\ = \frac1{n^2}[Var(X_1)+\dots+Var(X_n)]\\=\frac1{n^2}n\sigma^2=\frac{\sigma^2}{n}
$$
From this we understand that $\bar X$ itself is also a random variable with mean $\mu$ and variance $\sigma^2/n$   
   
