---
layout: post
title:  "The Metropolis algorithm and Monte Carlo methods in statistical mechanics"
date:   2016-11-25 08:00:00
---

The following are some introductory notes on how a [Monte Carlo method](https://en.wikipedia.org/wiki/Monte_Carlo_method){:target="_blank"}, namely the [Metropolis algorithm](https://en.wikipedia.org/wiki/Metropolis-Hastings_algorithm){:target="_blank"}, is used in statistical mechanics. This post is based on lectures by [Professor Carlo Carraro](https://maboudianlab.berkeley.edu/group-members/carlo-carraro/){:target="_blank"} at UC Berkeley, as well as the eight chapter of [Computational Physics](https://www.amazon.com/Computational-Physics-Fortran-Steven-Koonin/dp/0201386232){:target="_blank"} by Steven E. Koonin, which may be found online [here](http://hep.fcfm.buap.mx/cursos/2010/MNA/computational-physics.pdf){:target="_blank"}.

To start, it is important to distinguish what we mean by "the Metropolis algorithm" and "Monte Carlo methods". Monte Carlo methods describe a class of algorithms which are used to sample a probability distribution. One such algorithm was that discovered by Metropolis and others. We will start with a brief discussion of how Monte Carlo methods are useful in statistical mechanics, and then dive into the specifics of the Metropolis algorithm.


# Monte Carlo Methods

For our purposes in statistical mechanics, Monte Carlo methods may be thought of as a practical way to approximately calculate complicated integrals. Such integrals often come up in statistical mechanics, due to the tremendous number of degrees of freedom in a macroscopic system. For example, the partition function $$ Z $$ for a classical system with $$ \mathcal{N} $$ indistinguishable particles of mass $$ m $$ at temperature $$ 1/ \beta k_B $$ and pairwise potential $$ v (r_{ij}) $$ is given by

$$
\displaystyle
Z = \dfrac{1}{\mathcal{N}!} \left( \dfrac{V}{3 \lambda} \right)^{\mathcal{N}}
	\int \mathrm{d}^3 r_1 \mathrm{d}^3 r_2 \dots \mathrm{d}^3 r_{\mathcal{N}}
	\exp \left\{ - \beta \sum_{i < j} v(r_{ij}) \right\},
$$

where $$ V $$ is the volume of the system, $$ \lambda = \sqrt{(2 \pi \hbar / m k_B T)} $$ is the [thermal wavelength](https://en.wikipedia.org/wiki/Thermal_de_Broglie_wavelength){:target="_blank"} of a particle, and $$ r_{ij} $$ is the distance between particles $$ i $$ and $$ j $$. We know the number of particles and the thermal wavelength, so we are interested in calculating the integral above. The value of the integrand depends on the relative position of all the particles, and this integral quickly becomes untractable using standard techniques.
