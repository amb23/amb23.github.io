---
layout: post
title: "Spectral sequences 101"
date: 2023-08-29
categories: homological-algebra
usemathjax: true
---


<p>
I recently took another look at some spectral sequences and I started to understand them in a far more coherent fashion that I had initially where I thought of them as some form of brutish machine. To get to the main ideas we shall dispense with some of the additional structure which can be added back trivially later so some of the groups might look like they are missing indices etc.
</p>

<h3>Filtrations on cochains</h3>
<p>
Let \(V\) be a vector space. By a filtration on \(V\) we shall mean a decreasing filtration, so that is a family of vector subspaces \(F^p V \subset V\) such that \(F^p V \supset F^{p+1}V\). We then get a family of extensions
\[0 \rightarrow F^{p+1} V \rightarrow F^p V \rightarrow G^p V \rightarrow 0\]
where the final term in this sequence is by definition the \(p^{\text{th}}\) graded space. As we are working over the category of vector spaces this sequence will split although it doesn't in general. A nice simple example that we will come to throughout this note is that of Laurent power series taking value in some fixed vector space \(W\), so that a general element can be written as \(f = t^{-n}f_{-n} + \cdots \) where \(f_{i} \in W\). Here the filtration \(F^p\) corresponds to those terms whose leading order is \(t^p\) (note that \(p\in \mathbb{Z}\)) and the corresponding graded space is isomorphic to \(W\).
</p>

<p>
We are now going to assume that \(V\) has a differential on it, so \(d \in \text{End}(V)\) with \(d^2 = 0\). This differential is expected to act nicely with the filtration so that \(d F^p V \subset F^p V\). We can then use it to define a filtration on the cohomology of \(V\), that is we let \(F^pH(d)\) be the set of cohomology classes that can be represented by an element in \(F^pV\) (in fact we do not need the fact that the filtration behaves well with the differential to define this). In turn we can describe the graded cohomology group of \((V, F^\bullet)\) as the graded components of the cohomology. This is an invariant that may be weaker than the original cohomology but is computable (as we shall see below) and in many cases sufficient to determine the cohomology group itself.
 Our goal will be to calculate the graded cohomology groups so let us see what a class in this space is: \(\psi\in F^p V\) generates a  class if \(d \psi = 0\) and it generates the zero class if it is equal to \(\alpha + d \beta\) where \(\alpha \in Z(d) \cap F^{p+1}V\) and \(\beta \in d^{-1}F^pV\). The understanding of the roles of these two terms is important in understanding the spectral sequence proper.
</p>

<h3>Filtrations and Taylor expansions</h3>
<p>
Let us return to the condition that \(d F^p V \subset F^p V\). This in particular implies that we have the following sequences of cochain spaces:
\[ 0 \rightarrow F^{p+r} V \rightarrow F^{p} V \rightarrow G^{p, r}V \rightarrow 0\]
where the final term is a definition. This allows us to give a Taylor expansion of the differential operator \(V\). For a concrete example let us return to the case of Laurent power series, then a differential operator that respects the filtration is given by a differential that is analytic as a function of \(t\) amd we can expand it as
\[ d = d_0 + td_1 + t^2 d_2 + \cdots. \]
The Taylor expansion of this operator is given by considering it modulo \(t^{r+1}\). Such a Taylor expansion allows us to consider the subproblem of cochains that vanish up to a given level of the expansion of \(d\), namely we define \(Z^p_r \subset F^pV\) to be the set of terms that vanish up to higher orders, \(\psi \in Z^p_r\) if \(d \psi \in F^{p+r}V\). This (almost) gives us an iterative method for constructing the cocycles corresponding to the differential \(d\) for we have the natural inclusions
\[ \cdots \subset Z^{p}_{r+1} \subset Z^{p}_{r} \subset \cdots \subset Z^p_0 = F^pV. \]
The fact that some of the terms fail to lift are obstructions given by higher order terms in the differential operator. These obstructions may be interesting in their own right and in fact we shall see below that the spectral sequence tells us exactly what they are (up to boundaries).

There is another very simple but important fact to note about the Taylor expansion for a differential: it gives a deformation of the operator \(d_0\). Look at our example, in this case expanding \(d^2\) in powers of \(t\) (which corresponds to the graded part of the filtration) gives
\[ \begin{array}{rl}
d_0^2 &= 0 \\
(d_0, d_1) &= 0 \\
(d_0, d_2) + d_1^2 &= 0 \\
\cdots
\end{array} \]
and we get that \(d_0\) is a differential. As we include higher order terms we have more conditions a chain must satisfy to be a cocycle giving rise to the chain of inclusions. In general we don't get a composition in terms of linear operator \(d_i \in \text{End}(W)\) but, asuming the filtrations all split, we still get a deformation. So the filtration gives us a deformation of some differential coming from the Taylor expansion and this gives us a way of iteratively discussing cocycles.
</p>

<h3>The spectral sequence</h3>
<p>
So far we have seen how the Taylor expansion of the differential gives us a way of almost iteratively building the cocycles but it doesn't tell us about the obstructions and we haven't found a way of including the coboundaries. The reason lies in that the subspace that generates the coboundaries, \(d^{-1}F^pV\) is not generally a simple thing to get our hands on without working with the whole differential and we want to work iteratively. The idea lies in the fact that we are already computing the some part of this space \(d^{-1}F^p V\) in our iterative cocycle calculation - namely \(d Z^n_{p - n} \subset Z^p_\infty\). Thus we can use previously calculated cocycles to generate the coboundaries.

Turning back to the equivalence relation on the full cocycles \(\psi \in Z^p(d)\) we have that 
\[ \psi \sim \psi + \alpha + d \beta \]
where \(\alpha \in Z^{p+1}(d)\) and \(\beta \in d^{-1}F^pV\). Using the idea above we see that we have a natural choice now for both these spaces, namely we can select \(\alpha \in Z^{p+1}_{r-1} \subset Z^p_r\) and \(\beta \in Z^{p + 1 -r}_{r-1}\). Finally we define the spaces
\[ E^p_r = \frac{Z^p_r}{Z^{p+1}_{r-1} + dZ^{p +1 - r}_{r-1}}. \]
These are the vector spaces that we will use to approximate the graded cohomology. We see that as we let \(r\) tend to \(\infty\) that we will get the corresponding grading group if we have some suitable level of covering  by the filtration, e.g. \(\cap F^{p} V = 0\) and \(\cup F^p V = V\). For a fixed \(r\) we shall call the \(E^\bullet_r\) a page of the spectral sequence.
</p>
<p>
Before we complete the construction let us take a look at what this looks for the Laurent polynomials with a analytic differential \(d\). In this case we have that \(\psi \in Z^p_r\) implies that \(\psi = t^p f\) where \(f\) is analytic and \(df = 0 \text{ mod } (t^{r})\). By quotienting out by \(Z^{p+1}_{r-1}\) we see that \(t^p f \sim t^pg\) if \(f(0) = g(0)\) (this enforces the grading structure). Moreover the terms here where we apply the differential are given by those \(t^{p+1-r}b\) where \(b\) is analytic and \(d b = 0 \text{ mod }t^{r-1}\).
</p>
<p>
Now we come to the true magic of the spectral sequence: the differential \(d\) induces an operator \(\delta_r:E^p_r \rightarrow E^{p+r}_r\). This operator is a differential operator that measures the obstruction of solving the equation \(d\psi = 0\) to the next order, in fact the cohomology of this differential is the next page of the spectral sequence. Let us show this, firstly we have to check that \(\delta_r\) is well defined. This follows for \(d (\alpha + d \beta) = d\alpha \in d F^{p+1}_{r-1}\) and hence is zero in \(E^{p+r}_r\). Now assume that some class \([\psi]\) is a cocycle for this operator, then \(d \psi = \alpha + d \beta\) where \(\alpha \in Z^{p+r+1}_{r-1} \subset F^{p+r+1} V\) and \(\beta \in F^{p+1}_{r-1}\). This tells us that \(d(\psi - \beta) \in F^{p + r + 1}V\) and so the class \([\psi]\) can be extended without altering its zeroth order term, i.e. there is no obstruction at the next level. It follows as \(Z^p_{r} \subset Z^{p}_{r+1}\) that we get a surjective map from \(H^p(\delta_r)\) to \(E^p_{r+1}\). To show that it is injective let \([\psi] \mapsto 0\) so that \(\psi = \alpha + d \beta\) for \(\alpha \in Z^{p+1}_r\) and \(\beta \in Z^{p-r}_r\). But then \(\psi\) is in the image of \(\delta_r\), namely \([\psi] = \delta_r[\beta]\), and so it is zero in \(H^p(\delta_r)\).
</p>

<p>
Let us again return to our basic example to decode this information. Here we have that an element lying in \(E^p_r\) is given by a analytic function \(f\) with \(df = 0 \text{ mod }(t^r)\). The boundaries in this space are those analytic functions of the form \(tg + t^{1-p}dh\) where \(dh = O(t^{p-1})\). If we assume a simpler space, i.e. analytic rather than Laurent power series, we then get that there are no negative terms and the space \(E^0_r\) is simpler as the boundary terms \(d Z^{1-r}_{r-1} = d V = dW[[t]]\). In this case we get that the \(E^0_r\) is the zeroth graded part of the (now well defined) differential \(d \text{ mod } (t^r)\). Hence the full structure, both allowing for more complicated filtrations that aren't bounded below, and richer modules are a generalization of the simple fact that we are building the cohomology groups inductively on the order
</p>

<p>
That completes the basic structure: we see that the spectral sequence gives us a tool for itertively building the cohomology groups as we deform a differential on the space. The corresponding differnetial on each page describes the obstruction to a lift. I think this gives a nice way to think about a spectral sequence and links it closely to the idea of a Taylor expansion that I always considered to be there. The above description trivially generalizes to the case where \(V\) is replaced by a cochain complex or has additional structure, although as usual getting the filtration to play nicely with the additional structure, e.g. a ring structure, is an interesting problem.  
</p>

