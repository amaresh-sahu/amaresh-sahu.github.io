---
layout: post
title:  "The Metropolis algorithm and Monte Carlo methods in statistical mechanics"
date:   2016-11-25 08:00:00
---

The following are introductory notes on how a [Monte Carlo method](https://en.wikipedia.org/wiki/Monte_Carlo_method){:target="_blank"}, namely the [Metropolis algorithm](https://en.wikipedia.org/wiki/Metropolis-Hastings_algorithm){:target="_blank"}, is used in statistical mechanics. This post is based on lectures by [Professor Carlo Carraro](https://maboudianlab.berkeley.edu/group-members/carlo-carraro/){:target="_blank"} at UC Berkeley, as well as the eight chapter of [Computational Physics](https://www.amazon.com/Computational-Physics-Fortran-Steven-Koonin/dp/0201386232){:target="_blank"} by Steven E. Koonin, which may be found online [here](http://hep.fcfm.buap.mx/cursos/2010/MNA/computational-physics.pdf){:target="_blank"}.

To start, it is important to distinguish what we mean by "the Metropolis algorithm" and "Monte Carlo methods". Monte Carlo methods describe a class of algorithms which are used to sample a probability distribution. One such algorithm was that discovered by Metropolis and others. We will start with a brief discussion of how Monte Carlo methods are useful in statistical mechanics, and then dive into the specifics of the Metropolis algorithm.


# Monte Carlo Methods

For our purposes in statistical mechanics, Monte Carlo methods may be thought of as a practical way to approximately calculate complicated integrals. For example, consider a system with the following parameters:

* $$\mathcal{N}$$ indistinguishable particles of mass $$m$$

* temperature $$1 / \beta k_B$$

* $$i^{\text{th}}$$ particle position $$\mathbf{r}_i$$

* interaction potential $$ u(r_{ij}) $$

* volume $$V$$

* [thermal wavelength](https://en.wikipedia.org/wiki/Thermal_de_Broglie_wavelength){:target="_blank"} $$\lambda = \sqrt{ 2 \pi \hbar / m k_B T}$$.

The partition function $$Z$$ is given by

$$
\displaystyle
Z = \dfrac{1}{\mathcal{N}!} \left( \dfrac{V}{3 \lambda} \right)^{\mathcal{N}} \!\!
	\int \mathrm{d}^3 r_1 \mathrm{d}^3 r_2 \dots \mathrm{d}^3 r_{\mathcal{N}}
	\exp \left\{ - \beta \sum_{i < j} u(r_{ij}) \right\} ~,
$$

which becomes impossible to calculate analytically or even using standard quadrature methods for even modest systems of only a few dozen particles.

The basic idea of Monte Carlo methods is best demonstrated with a simple one-dimensional example, to calculate the integral

$$
\displaystyle
I = \int_0^1 f(x) ~\mathrm{d}x ~.
$$

The integral $$I$$ may be thought of as the average value of $$f(x)$$ for $$x \in [0, 1]$$, and can thus be approximated as

$$
\displaystyle
I = \dfrac{1}{N} \sum_{i = 1}^N f(x_i) ~,
$$

where the $$x_i$$ are randomly chosen from the interval $$[0, 1]$$. Clearly, the accuracy of this approximation improves as the number of randomly chosen points, $$ N $$, increases. The variance in $$ I $$, $$ \sigma^2_I $$, is related to the variance in $$ f $$, $$ \sigma^2_f $$, through the relation

$$
\displaystyle
\sigma^2_I = \dfrac{1}{N} \sigma^2_f ~,
$$

elucidating the important result of standard errors scaling as $$ 1 / \sqrt{N} $$.


# The Metropolis Algorithm

As in many popular results in science and enginering, it is unclear who deserves the majority of the credit for the algorithm described here, but due to the ubiquity of the name we refer to it as the Metropolis algorithm. The Metropolis algorithm provides a way to generate points which sample a probability distribution, and thefeore calculate equilibrium quantities.

Consider a system where the space of all possible states is denoted $$ \textbf{X} $$ and for any state $$ \textbf{x} \in \textbf{X} $$, the non-normalized probability of the system being in that state is $$ w(\textbf{x}) $$. The Metropolis algorithm generates a trajectory in the state space, given by the sequence of states $$ \textbf{x}_0, \textbf{x}_1, ... $$, such that long trajectories approximately sample the space $$ \textbf{X} $$ based on the probability distribution $$ w $$. It is important to note for statistical mechanical systems, the weight $$ w(\textbf{x}) $$ is known and is generally of the form $$ \exp (- \beta \, \mathcal{H}(\textbf{x})) $$. For many systems we can integrate out the momentum contribution, as was done in our earlier calculation of the partition function $$ Z $$, in which case the weight $$ w(\textbf{x}) = \exp(- \, \beta \, \mathcal{U}(\textbf{x})) $$, where $$ \mathcal{U}(\textbf{x}) $$ is the potential energy. 

Say the system is currently in a state $$ \textbf{x}_n $$. The Metropolis algorithm determines the ext state in the trajectory, $$ \textbf{x}_{n + 1} $$, according to the following procedure:

1. make a trial step $$ \textbf{x}_t $$

2. calculate the ratio $$ r = w(\textbf{x}_t) / w(\textbf{x}_n) $$

3. generate a random number $$ p $$ uniformly from the domain $$ [0, 1] $$.

4. if $$ r > 1 $$, $$ \textbf{x}_{n + 1} = \textbf{x}_t $$

   if $$ r < 1 $$, $$ \textbf{x}_{n + 1} = \textbf{x}_t $$ with probability $$ r $$ and $$ \textbf{x}_n $$ otherwise

Before proving this algorithm works, it is important to understand it intuitively. When $$ r > 1 $$, the trial state is more favorable and the system goes there next. When $$ r < 1 $$, the trial state is less favorable and the system only goes there some of the time -- depending on how relatively unfavorable the state is. The system cannot reject all less favorable states because otherwise it would remain frozen in a local minimum, and not demonstrate equilibrium behavior.

We prove the validity of the Metropolis algorithm by considering a large number of independent, non-interacting particles. We first determine a requirement on the probability of transitions between states, and then show the Metropolis algorithm satisfies this requirement.

For a large number of independently moving particles, we denote the total number of particles in state $$ \textbf{x} $$ during step $$ n $$ as $$ N_n (\textbf{x}) $$. In a single step, the net number of particles moving from state $$ \textbf{x} $$ to another state $$ \textbf{y} $$ is denoted $$ \Delta N_n (\textbf{x} \rightarrow \textbf{y}) $$ and given by

$$
\Delta N_n (\textbf{x} \rightarrow \textbf{y})
= N_n(\textbf{x}) \, P(\textbf{x} \rightarrow \textbf{y})
- N_n(\textbf{y}) \, P(\textbf{y} \rightarrow \textbf{x})
~,
$$

where $$ P(\textbf{x} \rightarrow \textbf{y}) $$ is the probability of moving to state $$ \textbf{y} $$ from state $$ \textbf{x} $$.
At equilibrium, $$ \Delta N_e (\textbf{x} \rightarrow \textbf{y}) = 0 \,\, \forall \, \textbf{x}, \textbf{y} \in \textbf{X} $$  as there is no flow of particles from one state to another. Consequently,

$$
\dfrac{N_e(\textbf{x})}{N_e(\textbf{y})} = \dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
~.
$$

The metropolis algorithm described above provides a method for calculating $$ P(\textbf{x} \rightarrow \textbf{y}) $$ and $$ P(\textbf{y} \rightarrow \textbf{x}) $$. To demonstrate the algorithm is valid, we will show

$$
\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{w(\textbf{x})}{w_(\textbf{y})} 
~,
$$

as $$ N_e(\textbf{x}) $$ is proportional to $$ w(\textbf{x}) $$. The probability $$ P(\textbf{x} \rightarrow \textbf{y}) $$ in the Metropolis algorithm may be written

$$
P(\textbf{x} \rightarrow \textbf{y})
= T(\textbf{x} \rightarrow \textbf{y})
  \cdot A(\textbf{x} \rightarrow \textbf{y})
~,
$$

where $$ T(\textbf{x} \rightarrow \textbf{y}) $$ is the probability of choosing a trial step $$ \textbf{y} $$ given the system is currently in state $$ \textbf{x} $$, and $$ A(\textbf{x} \rightarrow \textbf{y}) $$ is the probability of accepting the trial step. We assume the Metropolis algorithm is constructed such that $$ T(\textbf{x} \rightarrow \textbf{y}) = T(\textbf{y} \rightarrow \textbf{x}) $$, and will comment on this later. With this assumption, we calculate

$$
\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{T(\textbf{y} \rightarrow \textbf{x}) \, A(\textbf{y} \rightarrow \textbf{x})}{T(\textbf{x} \rightarrow \textbf{y}) \, A(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{A(\textbf{y} \rightarrow \textbf{x})}{A(\textbf{x} \rightarrow \textbf{y})}
~.
$$

We now consider the fourth step in the Metropolis algorithm.

## (i)
Say $$ w(\textbf{x}) > w(\textbf{y}) $$, for which $$ w(\textbf{x}) / w(\textbf{y}) > 1 $$ and $$ A(\textbf{y} \rightarrow \textbf{x}) = 1 $$. Furthermore, we accept the move from $$ \textbf{x} $$ to $$ \textbf{y} $$ with probability $$ w(\textbf{y}) / w(\textbf{x}) $$. Consequently, $$ A(\textbf{x} \rightarrow \textbf{y}) = w(\textbf{y}) / w(\textbf{x}) $$ and

$$
\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{1}{w(\textbf{y}) / w(\textbf{x})}
= \dfrac{w(\textbf{x})}{w(\textbf{y})}
~.
$$


## (ii)
Say $$ w(\textbf{x}) < w(\textbf{y}) $$, for which $$ w(\textbf{y}) / w(\textbf{x}) > 1 $$ and $$ A(\textbf{x} \rightarrow \textbf{y}) = 1 $$. Furthermore, we accept the move from $$ \textbf{y} $$ to $$ \textbf{x} $$ with probability $$ w(\textbf{x}) / w(\textbf{y}) $$. Consequently, $$ A(\textbf{y} \rightarrow \textbf{x}) = w(\textbf{x}) / w(\textbf{y}) $$ and

$$
\dfrac{P(\textbf{y} \rightarrow \textbf{x})}{P(\textbf{x} \rightarrow \textbf{y})}
= \dfrac{w(\textbf{x}) / w(\textbf{y})}{1}
= \dfrac{w(\textbf{x})}{w(\textbf{y})}
~.
$$

Thus the Metropolis algorithm correctly samples the probability distribution $$ w(\textbf{x}) $$.





 




