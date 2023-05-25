---
layout: post
title: "Tree complexities"
date: 2023-05-25
categories: decision-trees fisher-information
usemathjax: true
---

<h3>Introduction</h3>
<p>
In this note we are going to look at using Fisher information style
regularization terms for tree constructions. A general introduction to tree
building ideas can be found in the <a
href="https://xgboost.readthedocs.io/en/stable/tutorials/model.html">
excellent XGBoost documentation</a>. Throughout we shall try to keep the
notation approximately equivalent.
<br/>
<br/>
As with all general fitting methods we are looking at finding some function
\(f(x; w)\) where \(x\) denotes features whilst the \(w\) parameterize the
space of functions we are considering. The idea is to choose the \(w\) that
best fit that data with the least complexity. To do this we have two terms: the
first a residual cost, and the second a complexity term. This gives us a vector
optimization problem and we solve it by introducing a parameter that gives
a payoff between the two terms:
 \[\text{cost}(w) = \text{cost}_ {\text{residual}}(w) + \alpha \text{cost}_
 {\text{complexity}}(w).\]
We won't spend anytime discussing the residual costs and will just use standard
functions (i.e. mean square error, log proba etc.). 
<br/><br/>
Standard complexity measures include those that simply measure the size of the
parameters, an \(\ell^p\) regularization \(||w||^p\), or, as in XGBoost, a term
that measures the total number of leaves on the tree. The Fisher complexity is
not tree specific and instead measures how rapidly the function \(f\) changes
with respect to the feature variables. In general one can think of it as
introducing a term proportional to \(||\text{d}f||^2\) although we will go into
the details below.
<br/><br/>
As with all tree based methods we want to ensure that anything we construct is
fast (usually this means a single pass over the sorted data) and easy to
interpret. The second part will hold automatically as we are going to continue
to use a decision tree as the underlying type and we shall see that the main
algorithm, the forward algorithm, satisfies the general speed conditions.
</p>


<h3>Fisher complexity</h3>
<p>
Let \(f(x;w)\) be some function which we assume to have derivatives <a
id="rfootnote-1" href="#footnote-1">[1]</a> with respect to \(x\). There are
two cases to consider - the first which we call pointwise is where the function
is just pointwise so directly outputs the target, the second is where the
function returns a distribution for the target. In the pointwise case we consider
the metric
\[ I_{ij}(x) = \frac{\partial f}{\partial x_i} \frac{\partial f}{\partial x_j},\]
where \(x = (x_1, \ldots, x_n)\). For distributions we take
\[ I_{ij}(x) = \mathbb{E} \left[
\frac{\partial \log p}{\partial x_i} \frac{\partial \log p}{\partial x_j}
\right] \]
where \(p(y|x, w)\) is the distribution and the expectation is taken with
respect to it.
<br/><br/>
So far we have constructed a metric that is a function of \(x\) and we need to
get a number from this data to use it as a complexity metric. There are two
methods we can employ here. The first is to consider
\[ C(f) = \int \text{tr}(I) \text{d}x \]
where we integrate the trace of the metric over the feature space. The second
is to introduce a vector field \(\xi(x)\) and to consider
\[ ||\xi||^2 = \int I(\xi, \xi) \text{d}x.\]
The algorithms we construct here will use the first method but the second can
be interesting in situations we will explore elsewhere.
<br/><br/>
We are dealing with a finite sample space so a few things need to be taken into
account - we are going to inetgrate by simply summing over the sample space.
One should also note from these definitions that this regularization method is
not agnostic to the scale of the features and so some form of whitening would
be useful beforehand. In fact more is true: this regularization assumes that the
feature \(x\) is described in such a way that locality should imply similarity
of the targets. If this is not the case then this regularization shouldn't be 
used on those features.
</p>


<h3>Tree based regression</h3>
<p>
We now apply these methods to the simplest scenario of a regression based
model. Most of the concepts will apply to the more general cases that we
consider later.
In trees we generally think of a decision as being determined by a feature and
a cut. At this node the decision tree then looks like a step function - this is
too nasty for us to differentiate so we need to smooth it. To do this we
consider the cut as the pair of the lower and upper sample points. Then we
linearly interpolate between these points which allows us to construct the
gradient <a id="rfootnote-2" href="#footnote-2">[2]</a>:
<p align="center">
  <img src="/assets/dtree-gradient1.png" />
</p>

The calculation of the regularization for the full tree is involved and we do
note enter it here. Instead we look at greedy algorithms that allow us to
approximate the solution by iteratively building the tree. If we consider the
cost of the regression at a given cut we see that we get the terms
\[ \sum_{l = 1, 2} \left( G_l w_l + \frac{1}{2}H_l w_l^2 \right) + \frac{1}{2}
\alpha \frac{(w_2 - w_1)^2}{x_2 - x_1}\]
where the terms \(G_l\) and \(H_l\) are linear and quadratic terms coming from
the regression. Note that we divide the regularization term by the distance
between the two samples that define the cut. This means that cuts that are
close together are punished more than those where there is a larger interval of
ignorance. For most of the pointwise algorithms we will actually drop this division although it can be easily reinstated.
</p>

<h3>The forward algorithm</h3>
<p>
The forward algorithm is an iterative tree building algorithm that naturally
allows us to build in a reasonable approximation of the complexity cost. To
define the algorithm consider the following image:
<p align="center">
  <img src="/assets/treebuilding-forward.png" />
</p>
Think of the final unlabelled node as being underconstruction, i.e. we are
determinig whether to split it or not, and we have built the tree from left to
right. Looking up from this node we see that some of the features are bounded
to get to this node (in this example we have that the 1st and 4th features are
bounded whilst the 2nd feature is bounded from above). Moreover on all the
finite bounds we have a good approximation for the regression parameter (and
more generally the model we are fitting) normal to this face which is the other
parameter we defined when iteratively building the tree to this point (this
part is approximate, to get the true value we would need to know what happened
to that branch). When we look at the complexity of a given feature we can then
have to consider the gradient of the function that may include the bounds. The
fully bounded case looks like:
<p align="center">
  <img src="/assets/feature-variation.png" />
</p>
where \(w_L\) and \(w_U\) are the values of the parameters on the bounding
normal planes. In this case we have there terms that come into play and for
a regression problem these are simply \((w_L - w_1)^2\), \((w_1 - w_2)^2\), and
\((w_2 - w_U)^2\).
<br/>
It follows for the forward algorithm we get four choices for the cost function,
depending on where the function is bounded which has the general form (for
regression):
\[\text{cost}(w_1, w_2) = 
(G_1 - \alpha w_L) w_1 + \frac{1}{2}(H_1 + 2\alpha)w_1^2 
+ (G_2 - \alpha w_U) w_2 + \frac{1}{2}(H_2 + 2\alpha)w_2^2 - \alpha w_1w_2,\]
where we set \(w_L\) equal to \(w_1\) if the feature is not bounded below and
\(w_U\) equal to \(w_2\) if we are not bounded from above. In all cases we get
a closed form and so can get the optimal weights with one pass as in the normal
iterative case. 
<br/>
<br/>
<h4>The generic forward algorithm</h4>
We can extend the forward algorithm to a generic case. Here the \(w\) are now
higher dimensional parameterisations of the functions. We expand the cost
function to second order in the \(w_i\) which gives us a quadratic problem to
solve. This can be solved again by one pass although for the generic case we
will need to solve a linear problem at each cut. This is identical to the
generic case for the iterative algorithm with \(\ell^2\) or leaf number
regularization except that we get off diagonal terms in the quadratic
components (we can see this for the regression above). 
<br/><br/>
There is one major difference between the forward and the iterative algorithm.
Namely the forward algorithm will always find a solution at least as good as
the current choice of the regularization term (by simply setting \(w_1 = w_2
= w\)) as opposed to the case where we use leaf number for complexity. We
address this in the next section.
</p>

<h3>Forward variations</h3>
<p>
In this section we look at two simple variations of the forward algorithm. As
always we still have the single pass requirement.
<h4>Depth complexity</h4>
One simple way of adding depth complexity to the problem is to simply the add
leaf complexity term. The depth complexity variant is a different way of doing
this - namely we increase \(\alpha\) as we go down the tree. This variant
satisfies that we may stop building earlier due to failing to improve the cost.
Such a tree will allow for large differences between early branches in the tree
but later splits will be more and more constrained. This requires no update to
the generic algorithm except that \(\alpha\) is a function of depth so we need
to check whether the optimal split cost is better than that for the current
leaf.
<br/><br/>
<h4>Proportionality</h4>
Another issue with the way the algorithm is constructed is that there is
ambiguity in the cutting. For example in the image for the bounded problem the
cost doesn't vary if we move the location of the cut along the axis as long as
the choice of \(w_1\) and \(w_2\) remain constant. 
It makes sense that all other things being equal we should cut in the middle of
the interval rather than at the end. We can alter the cost by smoothing the
interpolation even more till we get the following view of \(f\):
<p align="center">
  <img src="/assets/proportional-variation.png" />
</p>
This removes the ambiguity and does what is expected for large \(\alpha\).
</p>


<h3>Additional problems</h3>
<p>
<h4>Gaussian regression</h4>
In a Gaussian regression problem we fit a Gaussian distribution with a negative
log likelihood function for the residual cost. The residual cost thus has the
from
\[ \sum_i \left(
    \frac{1}{2}\log(2\pi\sigma^2) + \frac{(y_i - \mu)^2}{2\sigma^2}
\right)\]
The residual cost is minimized at the expected solution: \(\mu = \overline{y}\)
and \(\sigma^2 = \sum_i(y_i - \overline{y})^2/N\). There are two methods for
calculating the Fisher metric for trees corresponding by how we interpolate the
Guassian distributions across the interval \([x_1, x_2]\): we can either
linearly interpolate or mix the distributions.
<br/><br/>
Linear interpolation gives a normal distribution of the form
\[\text{N}\left( \frac{x - x_1}{x_2 - x_1} \mu_1 + \frac{x_2 - x}{x_2
- x_1}\mu_2, \frac{x - x_1}{x_2 - x_1}\sigma_1^2 + \frac{x_2 - x}{x_2
  - x_1}\sigma_2^2 \right)\]
over the interval \([x_1, x_2]\). Then calculating the Fisher information over
this interval we get
\[I(x) = \frac{(\mu_2 - \mu_1)^2}{v(x)} 
+ \frac{1}{2}\frac{(v_2 - v_1)^2}{v(x)^2}.\]
Minimizing the complete cost function is an arduous task so we expand to 2nd
order about the minimum of the residual cost. We let \(\mu \mapsto \mu
+ \lambda\) and \(v \mapsto v\exp(\xi)\) where we now set \(\mu\) and \(v\) to
be the solutions as above we get the following regularization term:
\[
G = 
\left(
\begin{array}{c} G_{\mu_1} \\ G_{v_1} \\ G_{\mu_2} \\ G_{v_2} \end{array}
\right) =
\frac{1}{2} \left(
\begin{array}{c}
2 \Delta \mu (v_1^{-1} + v_2^{-1}) \\
(\Delta \mu)^2 v_1^{-1} + \Delta v (v_1 v_2^{-2} + v_1^{-2} v_2) \\
-2 \Delta \mu (v_1^{-1} + v_2^{-1}) \\
(\Delta \mu)^2 v_1^{-1} - \Delta v (v_1 v_2^{-2} + v_1^{-2} v_2)
\end{array}
\right)
\]
\[
\tiny
H = \left( \begin{array}{cccc}
v_1^{-1} + v_2^{-1} & \Delta \mu v_1^{-1} & -v_1^{-1} - v_2^{-1} & 2\Delta v_2^{-1} \\
\Delta \mu v_1^{-1} &
     \frac{1}{2}(\Delta \mu)^2v_1^{-1} + v_1^{-2}v_2^2 + v_1^2v_2^{-2} - \frac{1}{2}(v_1v_2^{-2} + v_1^{-1}v_2) &
    -\Delta \mu v_1^{-1} &
    -v_1^{-2}v_2^2 -v_1^2v_2^{-2} + \frac{1}{2}(v_1v_2^{-1} + v_1^{-1}v_2) \\
-v_1^{-1} - v_2^{-1} & -\Delta \mu v_1^{-1} & v_1^{-1} + v_2^{-1} & -\Delta \mu v_2^{-1} \\
\frac{1}{2}(\Delta \mu)^2v_2^{-2} &
    v_1^{-2}v_2^2 -v_1^2v_2^{-2} + \frac{1}{2}(v_1v_2^{-1} + v_1^{-1}v_2) &
    -\Delta \mu v_2^{-1} &
    \frac{1}{2}(\Delta \mu)^2v_2^{-1} + v_1^{-2}v_2^2 + v_1^2v_2^{-2} - \frac{1}{2}(v_1v_2^{-2} + v_1^{-1}v_2)
\end{array} \right)
\]
This is now set up as required for the generic forward algorithm as we can
iteratively update the coefficients \(\mu_i\) and \(v_i\).
<br/><br/>
The other option is mixing. This leads to a more complicated set of equations
so we shan't discuss it in detail here. It does however lead to a nice
alternative. Namely when we consider the mixed distribution the Fisher
information becomes an incredibly lengthy manual calculation. To get around
this we consider the Gini information as a regularization term, namely:
\[ I(x) = \mathbb{E}\left[ \left( \frac{\partial\, p(y|x)}{\partial x} \right)^2\right]\]
The exact same process leads us to a second order expansion.

<br/><br/>
<h4>Classification</h4>
The final example to consider is a classification problem. Conceptually this is
no more complex than the previous examples. Our choice of interpolation however
is restricted to just mixing rather than interpolation. We thus find that the
sample Gini cost is given by
\[ \frac{1}{2}\sum_i (p_{2i} + p_{1i})(\Delta p_i)^2. \]
As usual we expand this around the minimum of the unregularized solutions where
\(p_i = P_i\) are the sample proportions. So we let \(p_i = P_i + \xi_i\) and
we expand to the second order in the \(\xi\) noting that \(\sum \xi_i = 0\).
The residual cost in \(\xi\) becomes
\[ -\frac{1}{2}\sum_i P_i \xi_i^2. \]
The linear and quadratic terms from the regularization are given by:
\[
    G_{1i} = -\Delta P_i (3 P_{1i} + P_{2i}), \hspace{5mm} G_{1i} = \Delta P_i (P_{1i} + 3P_{2i})
\]
\[
\frac{1}{2} \left(\begin{array}{cc} H_{1i,1i} & H_{1i,2i} \\ H_{2i, 1i} & H_{2i, 2i}
\end{array} \right) = 
\left(\begin{array}{cc} 3P_{1i} - 2P_{2i} & -P_{1i}P_{2i} \\ -P_{1i}P_{2i}
& -2P_{1i} + 3P_{2i}\end{array}\right).
\]
We also got cross terms by applying the condition \(\xi_n = -\xi_1 - \ldots
- \xi_{n-1}\). Putting these together give the generic forward algorithm for classification.
</p>


<h4>Summary</h4>
<p>
This note describes an alternative method to regularization. The algorithm
defined here allows for a simple one pass for a generic tree and is essentially
the same linear problem although it will normally lead to harder problems (e.g.
off diagonal terms). Moreover the way we have described the problem requires
some manual massaging of the terms although this can be avoided by using
auto-differentiation once an interpolation method has been chosen.
<br/><br/>
The methods lead to a few interesting extensions. If we think about the
interpolation this generalisations to the notion of a soft tree. The simplest
form of a soft tree is a probabilistic decision tree where each node gives
a probability of moving to the lower or upper branch. This leads to the mixing
method as described for Gaussians. Even for decision trees some of the ideas
here can make sense, for example in the interval where the cut occurs we could
apply a probabilistic cut. These trees require slightly more advanced
techniques and we shall look at them in another note.
<br/><br/>
Code and examples for these ideas is beign semi-actively worked on and is 
available at <a href="https://github.com/amb23/softee">softee</a>.
</p>



<p id="footnote-1"><a href="#rfootnote-1">[1]</a> We do not actually require
that it is differential just that we can define some suitable approximation to
a derivative. This normally will be something of the form the average of the
upper and lower derivatives (these will normally exist).</p>

<p id="footnote-2"><a href="rfootnote-2">[2]</a> Note that this looks like
a sharpe ReLU activation. We explore this more when we look at soft trees in
the future.</p>

