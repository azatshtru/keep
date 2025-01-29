---
course_name: signals and systems
trimester_week: 1
---
A signal is a function of one or more variable, in this course, that variable is time.

Financial data like stock market trends that move up and down with time are signals.

Biomedical data like ECG/EKG and human speech that brain hears all change with time.

Signals are analyzed on the basis of four factors:
1. Continuity
2. Energy
3. Periodicity
4. Determinism
# Continuity
A function is continuous at a point if the left hand limit equals the right hand limit at that point.

In terms of signals, if the independent time variable is continuous, in which case the signal is written as $x(t)$, or the independent time variable can be discrete, which is written as $x[t]$.

The dependent signal variable can be continuous or discrete as well, when its codomain is discrete or continuous respectively, which leads us to four cases:

1. discrete time discrete signal (DTDV)
2. discrete time continuous signal (DTCV)
3. continuous time continuous value (CTCV)
4. continuous time discrete value (CTDV)
## Analog and Digital signals
Analog signals are CTCV signals
Digital signals are DTDV signals specifically from the codomain set of ${0, 1}$, i.e. the binary set.

> [!INFO] Complex signals
> Complex signals can be defined as $x(t) = x_R(t)+jx_I(t)$ where $j=\sqrt{-1}$ and $x_R$ and $x_I$ are the real and imaginary components of the signal.
# Energy and Power
Energy between a time interval T can be calculated as follows:

**Continuous case**
$$ E = \int_{-T/2}^{T/2}|x(t)|^2dt$$
**Discrete case**
$$E = \sum_{n=-N}^{N}|x[n]|^2$$
Usually, we take $T\to \infty$ and $N\to \infty$, and we get the energy over the entire time period.
1. $E = \int_{-\infty}^{\infty}|x(t)|^2dt$
2. $E = \lim_{N\to\infty}\sum_{n=-N}^{N}|x[n]|^2$

Instantaneous power is calculated by $x(t)^2$
Average Power is calculated by dividing the total energy by the total time.

**Continuous case**
$$P = \lim_{T\to\infty}\frac1T\int_{-T/2}^{T/2}|x(t)|^2dt$$
**Discrete case**
$$P = \lim_{N\to\infty}\frac1{2N+1}\sum_{n=-N}^{N}|x[n]|^2$$
> There can be cases where there is finite energy, in which case they are called **energy signals**, or infinite energy with finite average power, in which case they are called **power signals**.
# Periodicity
> [!INFO] shifting and scaling
> Adding something to the time variable is called shifting the signal $x(t+a)$, multiplying something to the time variable is called scaling the signal $x(at)$

A signal is periodic if it repeats itself after a time interval T. The smallest value of T after which the signal repeats itself is called the period of the signal. If a signal repeats itself after T, it also repeats itself after 2T, 3T, .... kT for all $k \in \mathbb R$

**Continuous case** $x(t) = x(t+kT)$
**Discrete case** $x[n] = x[n + kT]$

## caveats in periodicity of discrete signals
continuous signals of the form $cos(2\pi t)$ have the periodicity t = 1 and discrete signals of the form $cos[2\pi n]$ have periodicity n = 1.

Similarily, signals of the form $cos(2t)$ have periodicity $t = \pi$, but discrete signals of the form $cos[2t]$ cannot have periodicity $n = \pi$ because n should always be an integer, therefore signals of the form $cos[2t]$ cannot exist.

# Odd/Even signals
## time reversed signals
The operation of scaling a signal by -1 results in a time reversed signal corresponding to the original signal.
## odd signals
time reversed version of the signal is the negative of the original signal
$$x(-t) = -x(t)$$
$$x[-n] = -x[n]$$
A time reversed signal can be subtracted from its original signal to get an odd signal.
$$x_o(t) = \frac{x(t)-x(-t)}{2}$$
## even signals
time reversed version of the signal equal to the original signal
$$x(-t) = x(t)$$
$$x[-n] = x[n]$$
A time reversed signal can be added to its original signal to get an even signal.

$$x_e(t) = \frac{x(t)+x(-t)}{2}$$
## every signal can be expressed as a sum of odd and even signals
From the above equations, it can be shown that:
$$x_o(t) + x_e(t) = x(t)$$
# Deterministic vs Random signals
If signal values can be predicted using well defined mathematical functions, then the signals are called deterministic, otherwise they are random.

The course specifically deals with deterministic signals.

Examples,
- **sinusoids** $$x(t) = Asin(2\pi f_o t)$$
- **exponentials** $$x(t) = ce^{-\alpha t}$$
## properties of sinusoids
- period of sinusoid = $1/f_o$ = $2\pi/\omega$ 
- using DeMoivre's theorem, sinusoids can be written in complex exponential forms:
	- $$A\cos(\omega t + \phi) = \frac A2(e^{j\omega t + \phi} + e^{-j\omega t - \phi})$$
	- $$A\sin(\omega t + \phi) = \frac{A}{2j}(e^{j\omega t + \phi} - e^{-j\omega t - \phi})$$
> [!INFO] De Moivre's theorem
> $e^{j\theta} = \cos\theta + j\sin\theta$
> or, $e^{j\omega t} = \cos\omega t + j\sin\omega t$
 
- energy of a sinusoid for a any period T is constant for any starting and ending point in the period, i.e. energy of period -T/2 to T/2 is same as energy for the period 0 to T.
- Energy of a sinusoid of form $A\cos(\omega t)$ is $$E = \int_0^TA^2\cos^2(\omega t)dt = \frac{A^2T}{2}$$
- Power of a sinusoid of form $A\cos(\omega t)$ is $$P = \frac1T\int_0^TA^2\cos^2(\omega t)dt = \frac{A^2}{2}$$
## complex exponential
complex exponential is a combination of sinusoids and exponentials.
Let $C = Ae^{j\theta}$ and $\alpha = \sigma + j\omega$
then the continuous complex exponential is defined as:
$$Ce^{\alpha t}$$
It can be shown that $Ce^{\alpha t} = Ae^{\sigma t}\cos(\omega t+\theta) + jAe^{\sigma t}\sin(\omega t+\theta)$

The graph of real part of complex exponential gives a decaying or growing sinusoid.
### discrete complex exponential
$$Ce^{\alpha n} = Ae^{\sigma n}\cos[\omega n+\theta] + jAe^{\sigma n}\sin[\omega t+\theta]$$
## discrete unit impulse (kroneckar delta)
this is a discrete signal defined as:
$$\delta[n] = \begin{cases}0&& n\ne0\\1&&n=0\end{cases}$$
shifting the discrete unit impulse by some $n_0$ gives:
$$\delta[n - n_0] = \begin{cases}0&& n\ne n_0\\1&&n=n_0\end{cases}$$
### properties of kronecker delta
1. $x[n]\delta[n] = x[0]\delta[n]$
2. $x[n]\delta[n-m_0] = x[n_0]\delta[n-m_0]$
## discrete unit step
the unit step function is the accumulation of many iteratively shifted kroneckar deltas

$$u[n] = \delta[n] + \delta[n-1] + \delta[n-2] + \dots$$
$$u[n] = \sum_{i = 0}^\infty\delta[n-i]$$
This gives:
$$u[n] = \begin{cases}0&& n < 0\\1&&n\ge 0\end{cases}$$
$$u[n - n_0] = \begin{cases}0&& n < n_0\\1&&n\ge n_0\end{cases}$$
Further,
$$\delta[n] = u[n]-u[n-1]$$
## continuous unit step
$$u(t) = \begin{cases}0&& t < 0\\1&&t\ge 0\end{cases}$$
## gate/rect function
$$\text{rect}(\frac t T) = u(t+T/2)+u(t-T/2)$$
## continuous time impulse (Dirac delta)
Similar to discrete case the continuous unit step is an accumulation of some continuous time impules.
$$u[n] = \int_{0}^t\delta(t)dt$$
which means that dirac delta can be calculated by taking the derivative of the continuous unit step function
$$\delta(t) = \frac{du(t)}{dt}=\lim_{\Delta\to\infty}\frac{u(t+\Delta)-u(t)}{\Delta}=\lim_{\Delta\to\infty}\frac{\text{rect}(t/\Delta)}{\Delta}$$
like the discrete case, dirac delta is zero for all t != 0, but unlike the discrete case, dirac delta is **undefined** at zero.

but the integral of dirac delta from negative infinity to infinity is 1.

### properties of dirac delta
1. $u(t) = \int_0^t\delta(t-\sigma)d\sigma$
2. $x(t)\delta(t-t_0) = x(t_0)\delta(t-t_0)$
3. **Sifting property** $\int_{-\infty}^{\infty}x(t)\delta(t-t_0)dt = x(t_0)$
