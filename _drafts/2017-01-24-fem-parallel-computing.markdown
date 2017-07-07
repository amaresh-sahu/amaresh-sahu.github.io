---
layout: post
title:  "Lipid Membranes, Finite Elements, and Parallel Computing"
date:   2017-01-24 08:00:00
---

As part of a class I'm taking this semester, [Applications of Parallel Computing](https://sites.google.com/a/lbl.gov/cs267-spring-2017/){:target="_blank"}, I was asked to write a short bio about myself and my research interests, and to examine an application for which parallel computing has been useful.

I'm a first year chemical engineering PhD student at UC Berkeley, advised by [Prof. Kranthi Mandadapu](http://www.cchem.berkeley.edu/kranthi/){:target="_blank"}. I study [lipid membranes](https://en.wikipedia.org/wiki/Lipid_bilayer){:target="_blank"}, which make up the boundary of the cell as well as the boundaries of internal organelles such as the [nucleus](https://en.wikipedia.org/wiki/Cell_nucleus){:target="_blank"}, [endoplasmic reticulum](https://en.wikipedia.org/wiki/Endoplasmic_reticulum){:target="_blank"}, and [Golgi complex](https://en.wikipedia.org/wiki/Golgi_apparatus){:target="_blank"}. My [first research project](https://arxiv.org/abs/1701.06495){:target="_blank"} focused on developing a theoretical [continuum model](https://en.wikipedia.org/wiki/Continuum_mechanics){:target="_blank"} of arbitrarily curved lipid membranes, and I am now interested in using [finite element methods](https://en.wikipedia.org/wiki/Finite_element_method){:target="_blank"} to simulate the dynamics of lipid membranes according to this theory and comparing these results to experiments. For more about my background, see my [About](/about/) page.

Finite element methods allow us to numerically solve partial differential equations (PDEs) for which analytical solutions cannot be obtained. The spatial domain over which the PDE is to be solved is divided into many smaller pieces, called elements. By performing calculations on these much simpler elements, an approximate solution to the overall problem can be pieced together. Finite element methods have an incredible variety of applications, including fluid and solid mechanics, heat transfer, and structural mechanics. Furthermore, because finite elements break up a problem into many smaller sub-problems, they are very naturally parallelizable. Yet although finite element methods can be applied to many different types of problems, the algorithms used differ based on the types of PDE to be solved and consequently parallelization developments for one problem may not be applicable to another. As a result, there has been significant effort in parallelizing the solutions to many well-studied PDEs.

Lipid membranes pose a unique modeling challenge in that they behave as a two-dimensional fluid in the plane of the membrane, yet respond as an elastic shell to out-of-plane bending. There have been many theoretical advances in understanding lipid membranes over the last 20 years, and recently a theoretical foundation suitable for finite element methods [was developed](dx.doi.org/10.1177/1081286515594656){:target="_blank"}. Based on this foundation, a new finite element formulation [was created and used](https://arxiv.org/abs/1601.03907){:target="_blank"} to study a biologically relevant example: how lipid membranes dynamically change shape in response to external protein complexes which induce curvature. I am now interested in extending such finite element methods to my [more general membrane model](http://arxiv.org/abs/1701.06495){:target="_blank}, which accounts for in-plane viscosity, multi-component diffusion, and protein binding, and parallelize these algorithms in order to study more complicated membrane behavior over longer times. My goal is to compare these simulations to experimental results, and eventually to be able to simulate and predict how lipid membranes will respond to different physiological situations.














 




