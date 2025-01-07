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

> Complex signals can be defined as $x(t) = x_R(t)+jx_I(t)$ where $j=\sqrt{-1}$ and $x_R$ and $x_I$ are the real and imaginary component of the signal.
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
$$E = \lim_{N\to\infty}\frac1{2N+1}\sum_{n=-N}^{N}|x[n]|^2$$
# Periodicity
> [!INFO] shifting and scaling
> Adding something to the time variable is called shifting the signal $x(t+a)$, multiplying something to the time variable is called scaling the signal $x(at)$

A signal is periodic if it repeats itself after a time interval T. The smallest value of T after which the signal repeats itself is called the period of the signal. If a signal repeats itself after T, it also repeats itself after 2T, 3T, .... kT for all $k \in \mathbb R$

**Discrete case** $x(t) = x(t+kT)$
**Continuous case** $x[n] = x[n + kT]$

