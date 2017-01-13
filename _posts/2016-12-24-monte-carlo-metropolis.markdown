---
layout: post
title:  "The Metropolis algorithm and Monte Carlo methods in statistical mechanics"
date:   2016-12-24 08:00:00
---

The following are introductory notes on how a [Monte Carlo method](https://en.wikipedia.org/wiki/Monte_Carlo_method){:target="_blank"}, namely the [Metropolis algorithm](https://en.wikipedia.org/wiki/Metropolis-Hastings_algorithm){:target="_blank"}, is used in statistical mechanics. They are based on [Professor Carlo Carraro](https://maboudianlab.berkeley.edu/group-members/carlo-carraro/){:target="_blank"}'s lectures at UC Berkeley, which in turn are based on the eight chapter of [Computational Physics](https://www.amazon.com/Computational-Physics-Fortran-Steven-Koonin/dp/0201386232){:target="_blank"} by Steven E. Koonin, which may be found online [here](http://hep.fcfm.buap.mx/cursos/2010/MNA/computational-physics.pdf){:target="_blank"}.

It is important to distinguish the Metropolis algorithm from Monte Carlo methods: Monte Carlo methods describe a class of algorithms used to sample a probability distribution, and one such algorithm was discovered by Metropolis and others. We will start with a brief review of how Monte Carlo methods are useful in statistical mechanics, and then discuss the specifics of the Metropolis algorithm.


# Monte Carlo Methods

For our purposes in statistical mechanics, Monte Carlo methods are a practical way to approximately calculate complicated integrals. For example, consider the following system: 

* $$\mathcal{N}$$ indistinguishable particles of mass $$m$$

* temperature $$1 / \beta $$

* interaction potential $$ u(r_{ij}) $$

* volume $$V$$

* [thermal wavelength](https://en.wikipedia.org/wiki/Thermal_de_Broglie_wavelength){:target="_blank"} $$\lambda = 2 \pi \hbar / \sqrt{m k_B T}$$ .

The partition function $$Z$$ is given by

$$
\displaystyle
Z = \dfrac{1}{\mathcal{N}!} \left( \dfrac{V}{3 \lambda} \right)^{\mathcal{N}} \!\!
	\int \mathrm{d}^3 r_1 \dots \mathrm{d}^3 r_{\mathcal{N}}
	\exp \bigg\{ - \beta \sum_{i < j} u(r_{ij}) \bigg\} ~,
$$

which is impossible to calculate analytically or even using standard numerical methods for small systems consisting of only a few dozen particles.

The basic idea of Monte Carlo methods is best demonstrated with a simple one-dimensional example: calculate the integral

$$
\displaystyle
I = \int_0^1 f(x) ~\mathrm{d}x ~.
$$

The integral $$I$$ may be thought of as the average value of $$f(x)$$ for $$x \in [0, 1]$$, and can thus be approximated as

$$
\displaystyle
I \doteq \dfrac{1}{N} \sum_{i = 1}^N f(x_i) ~,
$$

where the $$x_i$$ are randomly chosen from the interval $$[0, 1]$$. Clearly, the accuracy of this approximation improves as the number of randomly chosen points, $$ N $$, increases. The variance in $$ I $$, $$ \sigma^2_I $$, is related to the variance in $$ f $$, $$ \sigma^2_f $$, through the relation

$$
\displaystyle
\sigma^2_I \doteq \dfrac{1}{N} \sigma^2_f ~,
$$

elucidating the important result of standard errors scaling as $$ 1 / \sqrt{N} $$.


# The Metropolis Algorithm

As in many popular results in science and enginering, it is [unclear](https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm#History){:target="_blank"} who deserves the majority of the credit for the algorithm described here, but due to the ubiquity of the name we refer to it as the Metropolis algorithm. The Metropolis algorithm provides a way to generate points which sample a probability distribution, and thefeore calculate equilibrium quantities.

### Algorithm

Consider a system for which the probability of being in state $$ \textbf{x} $$ is proportional to the (non-normalized) weight $$ w(\textbf{x}) $$. For example, after integrating the momentum degrees of freedom of a classical system, $$ w(\textbf{x}) = \exp\{- \beta \, \mathcal{U}(\textbf{x}) \} , $$ where $$ \mathcal{U} $$ is the potential energy of the system. The Metropolis algorithm generates a trajectory in state space, given by the sequence of states $$ \textbf{x}_0, \, \textbf{x}_1, \, \textbf{x}_2, \, \dots \, , $$ such that long trajectories sample the probability distrubtion defined by $$ w $$. If the system is currently in state $$ \textbf{x}_n $$, the Metropolis algorithm determines the next state in the trajectory, $$ \textbf{x}_{n + 1} $$, according to the following procedure:

1. make a trial step $$ \textbf{x}_t $$

2. calculate the ratio $$ r = w(\textbf{x}_t) / w(\textbf{x}_n) $$

3. (a) if $$ r > 1, \,\, \textbf{x}_{n + 1} = \textbf{x}_t $$

   (b) if $$ r < 1, \,\, \textbf{x}_{n + 1} = \textbf{x}_t $$ with probability $$ r $$ and $$ \textbf{x}_n $$ otherwise

Intuitively, when $$ r > 1 $$ the trial state is more favorable and the system always goes there next. When $$ r < 1 $$, the trial state is less favorable and the system only goes there some of the time -- depending on how relatively unfavorable the state is. The system cannot reject all less favorable states, however, because then it would quickly find an optimal state and never sample any other part of the distribution.

### Proof

We prove the validity of the Metropolis algorithm by considering a large number of independent, non-interacting particles. We first determine a requirement on the probability of transitions between states at equilibrium, and then show the Metropolis algorithm satisfies this requirement.

For a large number of independently moving particles, we denote the total number of particles in state $$ \textbf{x} $$ during step $$ n $$ as $$ N_n (\textbf{x}) $$. In a single step, the net number of particles moving from state $$ \textbf{x} $$ to another state $$ \textbf{y} $$ is denoted $$ \Delta N_n (\textbf{x} \rightarrow \textbf{y}) $$ and given by

$$
\Delta N_n (\textbf{x} \rightarrow \textbf{y})
= N_n(\textbf{x}) \, P(\textbf{x} \rightarrow \textbf{y})
- N_n(\textbf{y}) \, P(\textbf{y} \rightarrow \textbf{x})
~,
$$

where $$ P(\textbf{x} \rightarrow \textbf{y}) $$ is the probability of moving *to* state $$ \textbf{y} $$ *from* state $$ \textbf{x} $$.
At equilibrium, the change in particle distribution $$ \Delta N_e (\textbf{x} \rightarrow \textbf{y}) = 0 $$ for all $$ \, \textbf{x}, \, \textbf{y} \in \textbf{X} , $$ as there is no flow of particles from one state to another. Consequently,

$$
\dfrac{N_e(\textbf{x})}{N_e(\textbf{y})} = \dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
~.
$$

Because $$N_e(\textbf{x})$$ is proportional to $$ w(\textbf{x}) $$, the equilibrium condition can be written

$$
\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{w(\textbf{x})}{w(\textbf{y})} 
~.
$$


The Metropolis algorithm provides a way to calculate transition probabilities and we now show these probabilities satisfy the above equation. According to the Metropolis algorithm, the probability $$ P(\textbf{x} \rightarrow \textbf{y}) $$ may be written

$$
P(\textbf{x} \rightarrow \textbf{y})
= T(\textbf{x} \rightarrow \textbf{y})
  \cdot A(\textbf{x} \rightarrow \textbf{y})
~,
$$

where $$ T(\textbf{x} \rightarrow \textbf{y}) $$ is the probability of choosing a trial step $$ \textbf{y} $$ given the system is currently in state $$ \textbf{x} $$, and $$ A(\textbf{x} \rightarrow \textbf{y}) $$ is the probability of accepting the trial step. As we will comment on later, we assume the Metropolis algorithm is implemented such that detail balance, namely $$ T(\textbf{x} \rightarrow \textbf{y}) = T(\textbf{y} \rightarrow \textbf{x}) , $$ holds. With this assumption,

$$
\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{T(\textbf{y} \rightarrow \textbf{x}) \, A(\textbf{y} \rightarrow \textbf{x})}{T(\textbf{x} \rightarrow \textbf{y}) \, A(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{A(\textbf{y} \rightarrow \textbf{x})}{A(\textbf{x} \rightarrow \textbf{y})}
~.
$$

We now consider the Metropolis algorithm in the following two scenarios:

1. $$ w(\textbf{x}) > w(\textbf{y}) $$:

   In this case, $$ A(\textbf{y} \rightarrow \textbf{x}) = 1 $$ and $$ A(\textbf{x} \rightarrow \textbf{y}) = w(\textbf{y}) / w(\textbf{x}) $$. Consequently, 

	$$
	\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
	= \dfrac{1}{w(\textbf{y}) / w(\textbf{x})}
	= \dfrac{w(\textbf{x})}{w(\textbf{y})}
	~.
	$$

2. $$ w(\textbf{y}) > w(\textbf{x}) $$: 

   In this case, $$ A(\textbf{x} \rightarrow \textbf{y}) = 1 $$ and $$ A(\textbf{y} \rightarrow \textbf{x}) = w(\textbf{x}) / w(\textbf{y}) $$. Consequently, 

	$$
	\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
	= \dfrac{w(\textbf{x}) / w(\textbf{y})}{1}
	= \dfrac{w(\textbf{x})}{w(\textbf{y})}
	~.
	$$

Thus the Metropolis algorithm correctly samples the probability distribution $$ w(\textbf{x}) $$, and we can use it to calculate integrals as described in the Monte Carlo section.


# Considerations for Implementation

While we have examined the theoretical validity of the Metropolis algorithm, there are several practical considerations we must consider before implementation.


### Detail Balance

We mentioned earlier the condition of detail balance, 

$$
T(\textbf{x} \rightarrow \textbf{y}) = T(\textbf{y} \rightarrow \textbf{x})
~,
$$

which says if $$ \textbf{y} $$ is a possible trial move from $$ \textbf{x} $$, then $$ \textbf{x} $$ must be a possible trial move from $$ \textbf{y} $$, such that the probabilities of these two trial moves are equal. The condition of detail balance is satisfied by randomly choosing trial states within a fixed vicinity of the current state, and the size of this vicinity is important -- as demonstrated in the following example.

Consider a single particle in a one-dimensional potential, where states are labeled by the position $$ x $$. At each state, we randomly choose the trial state from the domain $$ [ x - \delta, \, x + \delta ] , $$ where $$ \delta $$ is a fixed parameter. A key consideration is the size of $$ \delta $$: if it is too large, every step could go to a far-away, unfavorable state which is always rejected, and if it is too small, the system will never leave a local vicinity where almost every move is accepted. In both cases, the system will only sample a small portion of the distribution and the algorithm will fail to capture the correct physics. It is therefore advisable to vary $$ \delta $$ until between 1/3 and 1/2 of moves are accepted, so as to avoid these two extremes. Even if this criteria is met, however, care must be taken to ensure the results are physically meaningful.


### Self-Correlation

Remember the Metropolis algorithm is used to generate a trajectory, from which we can calculate macroscopic observables. As we saw earlier, for longer trajectories our calculation becomes more precise. However, in order for the variances in our calculations to be small, our samples must be independent from one another -- and consecutive states in the trajectory are highly correlated with one another. Consequently, we will not be able to use all points in the trajectory to calculate macroscopic properties.

If we determine states in the trajectory separated by $$ k $$ steps are independent of one another, then we can calculate macroscopic properties from samples $$ k $$ steps apart. Thus for a trajectory of length $$ N $$, only $$ N / k$$ states can be used for calculations. This is one of the downsides of the Metropolis algorithm, as when $$ k $$ is large many of the points in the trajectory are not used. The value of $$ k $$ will in general vary based on what observables we are calculating. For an abstract observable $$ f $$, we calculate the auto-correlation function $$ C(k) $$ as

$$
C(k) = \dfrac{ \langle f_i \, f_{i + k} \rangle - \langle f_i \rangle^2 }{ \langle f_i^2 \rangle - \langle f_i \rangle^2 }
.
$$

By definition, $$ C(0) = 1 $$ and in general $$ C(k) $$ goes to zero for large $$ k $$. In practice, we wish to choose the smallest acceptable $$ k $$ to use as many sates from the trajectory as possible in our calculations, and pick the smallest $$ k $$ for which $$ C(k) \le 0.1 $$.


# Final Remarks

You should now have a general sense of how Monte Carlo methods and the Metropolis algorithm work. For a more detailed explanation, I recommend reading [Computational Physics](https://www.amazon.com/Computational-Physics-Fortran-Steven-Koonin/dp/0201386232){:target="_blank"} by Steven E. Koonin, as mentioned earlier. To truly understand these techniques, however, you should implement the Metropolis algorithm yourself. Start by considering a single particle in a harmonic potential, which can be solved analytically to verify any calculations you make. From there, you can keep iterating to study more and more complicated systems.

Thanks for reading, and please [contact me](mailto:amaresh.sahu@berkeley.edu) if you have any questions or comments!







 




