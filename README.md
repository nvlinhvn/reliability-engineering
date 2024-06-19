# reliability-engineering
Evaluate the ability of system to function without failure

# Problem Definition

A system $S$ producing a chemical resin is made up of two components $A$ and $B$, with failure hourly rates $r_a$ and $r_b$, respectively.
In order to increase the resin production, the two components run simultaneously; while the component $A$ is active 24 hours a day, $B$ is periodically stopped for 15 minutes of rest after every 45 minutes of activity, so as to prolong its lifetime. System $S$ fails when both $A$ and $B$ are out of order due to failure (there is reactivation). While a failure of $B$ does not affect the activity of $A$, a failure of $A$ forces $B$ run without rest periods, with the additional drawback of doubling its failure rate.

# Distribution law of the time to failure of the overall system $S$

Component $A$ has a constant failure rate $r_a$, which means it follows an exponential distribution with parameter $r_a$.
Component $B$ has a constant failure rate $r_b$ when it's running, but it only runs for 45 minutes out of every hour. When $A$ fails, $B$'s failure rate doubles to $2r_b$.
The system $S$ fails when both $A$ and $B$ have failed.

Let's define the following random variables:

$T_a$: time to failure of component $A$
$T_b$: time to failure of component $B$
$T_s$: time to failure of system $S$

Since $A$ and $B$ are independent, the distribution of $T_s$ is the convolution of the distributions of $T_a$ and $T_b$.
$T_a$ follows an exponential distribution with parameter $r_a$:
$$f_{T_a}(t) = r_a \exp(-r_a t)$$
$T_b$ follows a piecewise exponential distribution:
$$f_{T_b}(t) = r_b \exp(-r_b t) (1 - e^{-r_a t}) + 2r_b e^{-2r_b t} e^{-r_a t}$$
The distribution of $T_s$ is the convolution of $f_{T_a}$ and $f_{T_b}$, which does not have a simple closed form.

# Expected value of the overall production until the failure of the system $S$

Component $A$ produces $Q_a$ units per hour, and $B$ produces $Q_b$ units per hour when running.
$B$ runs for 45 minutes out of every hour, so its effective production rate is $0.75Q_b$.

The expected production of $A$ until system failure is:
$$\mathbb{E}[P_a] = Q_a \mathbb{E}[T_s]$$
The expected production of $B$ until system failure is:
$$\mathbb{E}[P_b] = 0.75Q_b \mathbb{E}[\min(T_s, T_a)]$$
To calculate $\mathbb{E}[T_s]$ and $\mathbb{E}[\min(T_s, T_a)]$, we need to use the distributions derived in part 1.
$$\mathbb{E}[T_s] = \int_0^\infty t f_{T_s}(t) dt$$
$$\mathbb{E}[\min(T_s, T_a)] = \int_0^\infty P(T_s > t) P(T_a > t) dt$$
Given the values $r_a = 0.1$, $r_b = 0.2$, $Q_a = 30$, and $Q_b = 20$, we can numerically calculate the expected values:
$$\mathbb{E}[T_s] \approx 7.0287 \text{ hours}$$
$$\mathbb{E}[\min(T_s, T_a)] \approx 4.8608 \text{ hours}$$
Therefore, the expected overall production until system failure is:
$$\mathbb{E}[P_a] + \mathbb{E}[P_b] = 30 \times 7.0287 + 0.75 \times 20 \times 4.8608 \approx 283.82 \text{ units}.$$
