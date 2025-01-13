---
trimester_week: 2
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
   
### Sample parameters as Random Variables   
Since, the sample statistics are calculated from a subset of a random variable, their values will also be random for different samples, hence the sample statistics can themselves be treated as random variables with their own mean and variance.   
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
[[Introduction to Econometrics (Estimators)]]
## Unbiased estimator   
Suppose we want to estimate the population parameter $\theta$, (theta could be population mean $\mu$, etc.) from the sample statistic $W$ (W could be sample mean $\bar x$).   
If one takes the sample again and again, the value of $W$ would sometimes be greater than, less than or equal to $\theta$ depending on the sample. But one can be sure that the average value of all the sample statistic $W$ would be very close (sometimes equal) to the population parameter $\theta$.   
$W$ is called an unbiased estimator if $E[W]=\theta$, i.e. mean of $W$ is equal to $\theta$.   
The bias of an estimator is calculated as $Bias(W) = E[W] - \theta$   
The bias in case of an unbiased estimator as described above is zero.   
### Sample mean's mean and variance   

$$
\bar X = \sum \frac{X_i}{n}\\\implies E[\bar X]=\frac1n\sum E[X_i]\\\implies E[\bar X] = \frac1n[E[X_1]+\dots+E[X_n]]\\\implies E[\bar X] = \frac1nn\mu=\mu
$$

$$
Var(\bar X) = Var(\frac{\sum X_i}{n})\\ = \frac1{n^2}[Var(X_1)+\dots+Var(X_n)]\\=\frac1{n^2}n\sigma^2=\frac{\sigma^2}{n}
$$
From this we understand that $\bar X$ itself is also a random variable with mean $\mu$ and variance $\sigma^2/n$   
### Biasedness of sample variance   
Sample variance is:   

$$
s^2 = \frac1n\sum (x_i-\bar x)^2\\=\frac1n[\sum(x_i-\mu+\mu-\bar x)^2]
\\=\frac1n[\sum((x_i-\mu)-(\bar x-\mu))^2\\
=\frac1n[\sum((x_i-\mu)^2+(\bar x-\mu)^2-2(\bar x-\mu)(x_i-\mu))\\
=\frac1n[\sum(x_i-\mu)^2+n(\bar x-\mu)^2-2(\bar x-\mu)\sum(x_i-\mu)]\\
=\frac1n[\sum(x_i-\mu)^2+n(\bar x-\mu)^2-2(\bar x-\mu)(\sum x_i-\sum\mu)]
\\=\frac1n[\sum(x_i-\mu)^2+n(\bar x-\mu)^2-2(\bar x-\mu)(n\bar x-n\mu)]
\\=\frac1n[\sum(x_i-\mu)^2+n(\bar x-\mu)^2-2n(\bar x-\mu)^2]
\\=\frac1n[\sum(x_i-\mu)^2-n(\bar x-\mu)^2]
$$
Taking expectation of sample variance:   

$$
E[s^2]=\frac1n\sum E[(x_i-\mu)^2]-E[(\bar x-\mu)^2]\\
=\frac1n\sum \sigma^2-\frac{\sigma^2}{n}\\
=\frac1nn\sigma^2-\frac{\sigma^2}{n}
\\=\frac{n-1}{n}\sigma^2
$$
The term inside the summation was the variance of population and the second term in the equation was the expectation of the sample mean random variable, hence the proof.   
Since expected value of Sample variance is not equal to the population variance, therefore, the sample variance is not an unbiased estimator.   
### Basel correction   
As seen above, the sample variance is a biases estimate. To correct the biasness, Basel correction is used.   
We know that sample variance:   

$$
s^2=\frac1n\sum(x_i-\bar x)^2

$$
Then the expectation of sample variance (as calculated above):   

$$
E[s^2]=E[\frac1n\sum(x_i-\bar x)^2]=\frac{n-1}n\sigma^2\\
\implies \frac1nE[\sum(x_i-\bar x)^2]=\frac{n-1}n\sigma^2\\
\implies E[\sum(x_i-\bar x)^2]=(n-1)\sigma^2\\
\implies\sigma^2=\frac1{n-1}E[\sum(x_i-\bar x)^2]=E[\frac1{n-1}\sum(x_i-\bar x)^2]

$$
We get that expectation of $\frac1{n-1}\sum(x\_i-\bar x)^2$ is equal to the population variance, hence $\frac1{n-1}\sum(x\_i-\bar x)^2$ is an unbiased estimator of population variance.   
Therefore, instead of taking $s^2=1/n\sum(x\_i-\bar x)^2$ as an estimator for variance, we can instead take $\frac1{n-1}\sum(x\_i-\bar x)^2$.   

$$
s^2=\frac1{n-1}\sum(x_i-\bar x)^2
$$
This is called Sample variance with basel correction.   
However, for larger sets of data, i.e. very large $n$, the value of sample variance without basel correction also approaches unbiasedness. For example, if $n=10000$, it doesn't matter if we are dividing by $10000$ or $9999$.   
## Efficient estimator   
Other than unbiasedness, the other desirable properties of estimators is Efficiency.   
The probability density of the estimates should be closer to the population parameter, this is also called reliability.   
If there are two estimates of a population parameter $\theta$, say $W\_1,W\_2$, such that $EW\_i=\theta$, then we should choose the one with lower variance.   
### Estimators for two sample points   
Let the two sample points be $x\_1$ and $x\_2$, such that they have a population mean $\mu$ and population variance $\sigma^2$.   
Let the estimator be any linear combination of the values of the two sample points, the estimator is denoted by $\psi$:   

$$
\psi = \lambda_1x_1+\lambda_2 x_2
$$
This generates a family of estimators that depends on the values of $\lambda\_1$ and $\lambda\_2$.   
Calculating mean of $\psi$:   

$$
E\psi = E[\lambda_1x_1+\lambda_2 x_2]=\lambda_1Ex_1+\lambda_2 Ex_2\\
=(\lambda_1+\lambda_2)\mu
$$
From the dual nature of sample points, we know that prior to taking sample, each sample point itself is also a random variable that can take any value from the population. Hence, each sample point random variable has the same distribution, mean and variance as the original population random variable.   
For $\psi$ to be an unbiased estimator, its expectation must be equal to $\mu$.   

$$
E\psi = (\lambda_1+\lambda_2)\mu=\mu\\
\implies \lambda_1+\lambda_2=1
$$
Hence, infinite number of unbiased estimators are possible as infinite number of real numbers can sum upto one. i.e., any $\psi = (\lambda+(1-\lambda))\mu$ is an unbiased estimator.   
To pick the most efficient estimator from the family of unbiased estimators described above, we need to pick the one with the least variance.   
Calculating variance of $x\_1$ and $x\_2$   
Assuming that $X\_1$ and $X\_2$ are drawn from the population independently, we can say that $\text{Cov}(X\_1,X\_2)=0$   
Based on the above assumption, using the formula for variance of sums of random variables, we have:   

$$
Var(\psi)=Var(\lambda_1X_1+\lambda_2X_2)
\\=\lambda_1^2\space Var(X_1)+\lambda_2^2\space Var(X_2)+2\lambda_1\lambda_2\text{Cov}(X_1, X_2)
\\=\lambda_1^2\space Var(X_1)+(1-\lambda_1)^2\space Var(X_2)
$$
We know that the variance of both the Sample point random variables equals the variance of the population, hence:   

$$
Var(\psi)=\lambda_1^2\space Var(X_1)+(1-\lambda_1)^2\space Var(X_2)
\\=\lambda_1^2\sigma^2+(1-\lambda_1)^2\sigma^2=(\lambda_1^2+(1-\lambda_1)^2)\sigma^2
$$
$(\lambda\_1^2+(1-\lambda)^2)$ is minimum if $\lambda=0.5$. (Calculated by using derivatives and minima)   
Hence, from the family of unbiased estimators for a sample of size two, the most efficient one is the one with $\lambda=0.5$.   
## Mean Squared Error   
Sometimes it is not possible to have an unbiased estimator. Estimators may have different variance. One has to pick one of the unbiased estimators. If there are different estimates with different biases, then we pick one with the least mean squared error.   

$$
MSE = E[(W-\theta)^2]
$$
Where $W$ is the estimate and $\theta$ is the true population value.   
It can be shown that $MSE = Var(W)+(\text{Bias}\space W)^2$   
## Sample Covariance and Sample Correlation   
Sample Covariance:   

$$
\text{cov}(X, Y)=\frac{\sum(x_i-\bar x)(y_i-\bar y)}{n}
$$
Sample Correlation:   

$$
r_{XY}=\frac{\text{cov}(X,Y)}{s_X*s_Y}
$$
For sample covariance, if we divide by $n-1$ instead of $n$, then it is an unbiased estimate of population covariance.   
Sample correlation is a biased estimator of population correlation and no unbiased estimator for population correlation exists.   
## Consistent estimator   
Let $W\_n$ be an estimate of population parameter $\theta$ for a sample of size $n$, then $W\_n$ is said to be consistent estimator of $\theta$ if for all $\epsilon>0$:   

$$
\lim_{n\to\infty}P(|W_n-\theta|>\epsilon)=0
$$
This means that an estimator is a consistent estimator, if as the sample size increases, the probability that the difference between sample statistic and population statistic is greater than some epsilon reaches zero.   
When sample statistic is taken multiple times, the different values of sample statistic will sometimes be closer to the population statistic, and sometimes farther. If the sample size is smaller, than the probability that the sample statistic is closer to population statistic is less, but as the sample size increases, then the sample statistic will approach the population statistic if it is consistent, effectively making them equal at infinity, hence at larger sample sizes the probability that the difference between population statistic and sample statistic is greater than any positive value will reduce because they will become equal.   
A consistent estimator an be biased, as well as an inconsistent estimator can be unbiased. Aim for consistency more than unbiasedness.   
### Example 1   
Consider an estimator: $\Lambda\_n=\frac1{n+1}\sum x\_i$.   
Now, $E[\Lambda\_n]=\frac{n}{n+1}\mu$, since the expected value of the estimator is not equal to the population mean $\mu$, this is a biased estimator.   
But, $\lim\_{n\to\infty}P\|\Lambda\_n-\mu\|=0$, because as n increases, the value of $\Lambda\_n$ will become very close to $\mu$, and at infinity they will subtract to give zero.   
Hence, $\Lambda\_n$ is biased yet consistent.   
### Example 2   
Let the sample taken from a population be $X\_1, X\_2,\dots,X\_n$.   
If we take the estimator of mean as the first random variable among these, i.e. $X\_1$, then:   
Expectation of estimator = $E[X\_1]$ = $\mu$ which means that since estimator's expectation is $\mu$, it is an unbiased estimator.   
However, $P(\|W\_n-\theta\|<\epsilon)$, i.e.:   

$$
P(|X_1-\mu|<\epsilon)\\=P(\mu-\epsilon<X_1<\mu+\epsilon)\\=F(\mu+\epsilon)-F(\mu-\epsilon)\neq 0
$$
where F is the CDF of the distribution.   
The probability that the first sample point is the mean is always random for any size of the sample, no matter how large.   
Hence, taking the first sample point as the estimator for population mean surely is unbiased, but it inconsistent.   
### Example 3   
Variance of sample mean is $\sigma^2/n$. As n increases, the variance in sample mean becomes zero. Which means that the error made in predicting population mean from sample mean also goes to zero indicating that the sample mean is a consistent estimator of population mean.   
## Asymptotic Normality   
As the sample size becomes large, let the estimates be $X\_1, X\_2,\dots X\_n$. The probability that the sample estimate is less than some value $x$ approaches the CDF of a standard normal distribution as the sample size grows.   

$$
\lim_{n\to\infty}P(X_n\leq x) = \Phi(x)
$$
## Central Limit Theorem   
Distribution of Sample mean becomes a normal distribution $\bar X \sim N(\mu,\sigma^2/n)$ as sample size become large.   
## Hypothesis testing: Normal Case   
By reading scientific literature or combining pieces of other knowledge, if we come up with a **hypothesis** that the mean temperature of the sea is 20 degree celsius (population mean). Then we can go ahead and compute the sample mean. Here 20 degree is the initial belief, also known as **The Null Hypothesis ($H\_0$)**.   
### The Alternate Hypothesis   
If the Null Hypothesis is false based on the sample calculations, we may conclude that something else is correct, this is called the Alternate hypothesis $H\_1$.   
For the sake of example, let $H\_1$ be $\mu\neq 20$. This we gathered because our sample mean is either too large or too small from 20.   
In order to reject to reject $\mu=20$, 20 must be really far away from $\mu$ in the distribution graph of $\bar x$.   
### Quantifying "really far away"   
In social sciences, really far away generally account to one of the levels of significance. Either 1%, 5% or 10%. For a standard normal variable:   
|                           Computed z | Probability of Observing |
|:-------------------------------------|:-------------------------|
| Greater than 1.65 or less than -1.65 |                 10% (.1) |
| Greater than 1.96 or less than -1.96 |                 5% (.05) |
| Greater than 2.58 or less than -2.58 |                 1% (.01) |

This means that only 10% of values lie after 1.65 in a standard normal distribution and only 1% of values lie after 2.58.   
Another method of saying this: The sum area under the standard normal distribution graph after 1.96 and before -1.96 is $0.05$.    
Jargon method for saying this: The critical value of z (for a two tailed test) at 5% level of significance is 1.96.   
### Okay, but how to test null hypothesis? (spoiler, it's z-test)   
To test null hypothesis, one can first calculate the sample mean of size n. The sample of size n is one of the many samples of size n one can choose. The sample means of all the possible samples of size n lie in a normal distribution. The normal distribution for sample mean random variable has mean $\mu$ which is equal to the population mean and variance $\sigma^2/n$.   
Now, standardize the sample mean by subtracting $\mu$ from it and dividing it by $\sigma^2/n$.   
This standarization will give a value from a standard normal distribution. If the value is larger than 2.58 or less than -2.58, then this means that the value of sample mean is only possible about 1% of the time given the null hypothesis which is very unlikely, this means that the null hypothesis has a very large probability of being wrong.   
If values of z are larger than 1.96 or smaller than -1.96, then that is possible only 5% of the time and so on.   
To find what values of sample mean would not lead to rejection of null hypothesis at a certain level of significance, we can do $(\bar x-\mu)/(\sigma/\sqrt{n}) \leq z\_\alpha$   
where $z\_\alpha$ is the z value at a certain $\alpha$ percent level of significance.   
### Errors in standardization   
$\text{Type I}$ error is when the null hypothesis is rejected when it is true.   
$\text{Type II}$ error is when null hypothesis is accepted when it is false.   
We define two parameters:   
$\alpha = P(\text{Type I error is true})$   
$\beta = P(\text{Type II error is true})$   
We can write alpha and beta as:   

$$
\alpha = P(\text{accept}\space H_1|H_0\space\text{is true})\\\beta = P(\text{accept}\space H_0|H_1\space\text{is true})
$$
In old statistics, it is near impossible to minimize both. So a general rule is to minimize alpha.   
### p value   
p value of a hypothesis is the probability at which we can just reject the null hypothesis.   
For example, if computed z value of a sample mean is 1, then the probability of the sample mean being larger than 1 and less than -1 is 0.3174, which is the p-value.   
Now, p value represents probability, but it dictates which level of significance to use.   
If the probability of just rejecting hypothesis (p-value) is less than 0.01, then we should use 1% LOS. Note that this one percent is the value of the random variable, not the probability, while p-value is a probability. In essence, the probability is dictating which values of random variable to use.   
If p-value is between 0.05 and 0.01, then it is advisable to reject at 5%.   
If p-value is between 0.1 and 0.05, then it is advisable to reject at 10%.   
### t-test   
Until now, we have assumed that the value of population variance is known, i.e. $\sigma^2$ is known, which can be used to calculate the variance of sample mean random variable $\sigma^2/n$.   
But this is too tall of a claim because no way on god's green earth can we know that stuff.   
Therefore, instead of using the population variance $\sigma^2$, we will use basel corrected sample variance $(s')^2$ that can be calculated in reality.   
The expression $(\bar x-\mu)/(s'/\sqrt{n})$ that represents the standardization of the sample mean using sample standard deviation instead of population standard deviation follows a t-distribution of n-1 degrees of freedom instead of a z-distribution.   
The t-distribution follows a separate table for 1%, 5%, 10% for different degrees of freedom, which can be looked up to analyze and reject the null hypothesis using the sample variance instead of population variance.   
### One-sided z test   
If we reject the null hypothesis, we now have to find out whether the actual value of mean is larger than or smaller than the one hypothesized in the null hypothesis.   
One-sided z test helps in finding whether the value is larger or smaller than the null hypothesis.   
We only look at one side tail of the z-distribution.   
| Level of significance |         One sided z reject limits |
|:----------------------|:----------------------------------|
|                   10% | More than 1.28 or less than -1.28 |
|                    5% | More than 1.65 or less than -1.65 |
|                    1% | More than 2.33 or less than -2.33 |

### One-sided t test   
To calculate if the population mean is larger or smaller than the sample mean, while using the sample variance instead of the population variance, one can do a one sided t-test instead of a one sided z test. The one sided critical value at 5% LOS is 1.753   
### Confidence Interval   
Given the sample mean and population variance, confidence interval is the interval in which the existence of population mean is certain with some probability.   
For example the confidence interval for 5% LOS is inside the interval $(-1.96, 1.96)$ for a standard z-distribution.   
For any normal distribution, it can be calculated by de-standardizing the sample mean random variable:   

$$
-1.96\leq \frac{\bar x-\mu}{(\sigma/\sqrt n)}\leq 1.96
$$
The above is for 5% LOS, for any LOS $\alpha$ with the associated z value $z\_\alpha$, we have:   

$$
-z_\alpha\leq \frac{\bar x-\mu}{(\sigma/\sqrt n)}\leq z_\alpha
$$
Rearranging the above inequality and solving for $\mu$ will give the interval.   
If LOS is 5%, then the Confidence Interval is called a 95% percent confidence interval because we have calculated the confidence interval to be inside the 5% critical cutoff points, similarly for other intervals.   
Some Jargons: The sample mean is called the **point estimate**, the point estimate has very low probability to be equal to the population parameter. The Confidence interval is called the **interval estimate**, of which we have a confidence value that the population parameter lies in between the interval.    
Again, if instead of population variance, which is unrealistic to know, we use the sample variance, then instead of calculating the confidence interval using the z-test critical cutoff points, we will use the t-test critical cutoff points.   
