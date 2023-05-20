---
layout: post
title: "What's the point?"
date: 2023-05-19
categories: supermaths category-theory
usemathjax: true
---


<p>
In this note we are going to look at points in supermanifolds with the goal of
finding a nice concrete representation of the category. This allows us to see
that the language used in both the physics and the mathematics literature are
describing the same objects.
It is also a nice exercise in seeing how much richer the internal hom of
a category is compared to the hom sets.
</p>


<h4>Internal Hom</h4>
<p>
Let \(\text{SMan}\) denote the category of supermanifolds. As is usual for \(X,
Y \in \text{SMan}\) we have that a map \(X \rightarrow Y\) is a pair \((f,
f^\#)\) which defines a morphism of locally ringed spaces \((X_0,
\mathcal{O}_ {X}) \rightarrow (Y_0, \mathcal{O}_ {Y})\).

For example consider the spaces \(X = \mathbb{R}^{0|1}\) and \(Y = \mathbb{R}^{1|0}\).
Any map \(X \rightarrow Y\) is given by a super morphism \(C^{\infty}(\mathbb{R}) \rightarrow
\mathbb{R}[\eta]\) and thus is determined completely by where it sends the
variable \(t \in C^{\infty}(\mathbb{R})\). For a morphism \(Y \rightarrow X\) we
see that \(\eta\) must be mapped to zero and so there is only a single morphism \(Y
\rightarrow X\). We thus have
\[\text{Hom}(\mathbb{R}^{0|1}, \mathbb{R}^{1|0}) \cong
\mathbb{R}, \hspace{8mm} \text{Hom}(\mathbb{R}^{1|0}, \mathbb{R}^{0|1}) \cong
* .\]


There is another model for the functions between two supermanifolds called the
internal Hom, and we deonote it by \(\underline{\text{Hom}}(X, Y)\). This is
the presheaf defined by the currying formula:
\[\underline{\text{Hom}}(X, Y)(S) = \text{Hom}(S \times X, Y).\]
As is usual we can think of this presheaf as some generalization of
a supermanifold.
If we return to the example of functions \(\mathbb{R}^{1|0} \rightarrow
\mathbb{R}^{0|1}\) we see that \(\text{Hom}(S,
\underline{\text{Hom}}(\mathbb{R}^{1|0}, \mathbb{R}^{0|1}))\) is isomorphic to
\(C^{\infty}(\mathbb{R}) \otimes C^\infty(S)_ \text{odd}\) where we need to complete the
tensor product. We see that the internal hom has a lot more structure than the
singleton hom set implied.
</p>


<h4>The functor of points</h4>
<p>
Let \(\mathcal{C}\) be a category and let let \(C \in \mathcal{C}\). Then \(C\)
defines a functor \(\text{Hom}(C, -)\) on the presheaves of \(\mathcal{C}\)
which we call the functor of \(C\)-points (or just functor of points). By
Yoneda we have that for \(F\) a presheaf its \(C\)-points are exactly \(F(C)\).
<br/><br/>
For example let \(X \in \mathfrak{Top}\). Then for \( * \in \mathfrak{Top}\)
a singleton we have that \(X( * ) \) is simply the underlying set of \(X\). If \(X
\in \text{SMan}\) and \( * = \mathbb{R}^{0|0}\) then \( X( * )\) is the
underlying set of the body of \(X\).
<br/><br/>
There is a small but important generalization to this notion. Let \(D \subset
\mathcal{C}\) be a diagram. Then we can define the \(D\)-points of a presheaf
\(F\) to be the colimit of the diagram in set:
\[F(D) = \varprojlim_{C \in D} F(C).\]
One can check that this extends to a functor from the presheaves on
\(\mathcal{C}\) to \(\mathfrak{Set}\). Note that if the limit of the diagram
exists in \(\mathcal{C}\) then we can replace the diagram with the limit for
all intents and purposes. By working with an ind-representable object we allow
spaces that may be too big to fit into the original category (as we will see
below).
<br/><br/>
For example consider the diagram in \(\mathfrak{Vect}\), \(A, B: V \rightrightarrows
W\). Then for a vector space \(U\) we have that the \(D\)-points are isomorphic
to \(\text{Hom}(W / \text{im}(A - B), U)\)
</p>

<h4>Concrete categories and points</h4>
<p>
Let \(\mathcal{C}\) be a category and suppose it has a forgetful functor to set
(we shall normally not indicate the functor and subscript the Hom sets to make
it clear what category we are in). Obvious examples come from topological
spaces, groups, etc.
We say that the category \(\mathcal{C}\) is concrete if the forgetful functor
is faithful. This roughly means that elements in a concrete category are sets
enriched with some additional structure. This is a useful tool as we all know
what sets are and so can easily get intuition about such objects <a 
id="rfootnote-1" href="#footnote-1">[1]</a>.
<br/><br/>
Let us take a step back and think about what relationship this has to the
functor of points as described above which is a ind-representable
forgetful functor. We start by looking at some examples: For topological spaces we have a
terminal object, the singleton \( * \), and we can describe the forgetful 
functor as the \( * \)-points of a space. For groups we do have a terminal 
object, namely the singleton considered as a group but this doesn't represent 
the forgetful functor. We still can represent a concrete forgetful functor by
consider the functor of \(F_1\)-points where \(F_1\) is the free group on
a single generator.
<br/><br/>
What these examples show us is that to get a good notion of the underlying set
of an object in a category it is not (necessarily) useful to consider what we
may think of as "natural" forgetful functors (e.g. the body of a supermanifold)
but instead we should look for representable functors that are concrete (and in
fact ind versions). To formalize this we shall say that a diagram \(T\) is
a <i>point</i> of \(\mathcal{C}\) if the functor of \(T\)-points is concrete
<a id="rfootnote-2" href="#footnote-2">[2]</a>:
\[\text{Hom}_ \mathfrak{C}(X, Y) \subset \text{Hom}_ \mathfrak{Set}(X(T),Y(T)).\]
As alluded to above this allows us to think of elements of \(\mathcal{C}\) as
sets with some enriched structure (continuity, smoothness, algebraic properites
etc).
<br/><br/>
Let \(T\) be a point in \(\mathcal{C}\). It is not true that \(\text{Hom}(X, F)
\subset \text{Hom}_ {\mathfrak{Set}}(X(T), F(T))\) for \(F\) an arbitrary
presheaf. However we do get a result relating to the internal hom:
<br/><br/>

<b>Claim: </b><i>Let \(\mathcal{C}\) be a category where we can define products and
thus the internal hom presheaf as we did for supermanifolds. Then if \(T\) is a point
of \(\mathcal{C}\) we do have a natural embedding:</i>
\[\text{Hom}(S, \underline{\text{Hom}}(X, Y)) \subset \text{Hom}_
{\mathfrak{Set}}(S(T), \underline{\text{Hom}}(X, Y)(T)).\]
<br/>
<div style="border-left-style:solid;padding-left:5px;">
Let \(f: S \times X \rightarrow Y \in \text{Hom}(S, \underline{\text{Hom}}(X, Y))\).
Then define the function \(f_ * \) by \(f_ * (s:P \rightarrow S)_ {P \in T} = (f \circ (s \times 1_X): P \times X \rightarrow Y)_ {P \in T}.\)
We claim that this is injective. Given \(f \neq g \in \text{Hom}(S,
\underline{\text{Hom}}(X, Y))\) as \(T\) is a point we can find some
\((s \times x: P \rightarrow S \times X)_ {P \in T} \in (S \times X)(T)\) that seperates the two
functions. Then \(f_ * (s)_ {P \in T} \neq g_ * (s)_ {P \in T}\) which we can see by composing them
with \( (1_S \times x)_ {P \in T}\) where we have used that \((S \times X)(T) \cong S(T)
\times X(T)\).
</div>
<br/><br/>
Note that we clearly have an embedding
\[\text{Hom}_ {\mathcal{C}}(S, \underline{\text{Hom}}(X, Y))
\subset \text{Hom}_ {\mathfrak{Set}}(S(T), \text{Hom}_ {\mathfrak{Set}} (X(T),Y(T)))\]
by using the defining property and then currying back in \(\mathfrak{Set}\).
The above result is stronger saying that we only need to consider the
\(T\)-points of the internal hom.

</p>

<h4>Super points</h4>
<p>
Note that the class of superspaces with body a point has a set of isomorphism
classes given by \(\{ \mathbb{R}^{0|n}\}_ {n\in\mathbb{N}}\). This is a natural
starting point for us to consider in constructing a point for
\(\text{SMan}\). We would rather have a connected point so we define \(T\) by
the diagram
\[T := \hspace{5mm} \{ \mathbb{R}^{0|0} \rightarrow \cdots \rightarrow \mathbb{R}^{0|n} \rightarrow
\mathbb{R}^{0|n+1} \rightarrow \cdots  \}\]
where the maps are given by the natural surjections \(\mathbb{R}[\eta_1,
\ldots, \eta_{n}] \rightarrow \mathbb{R}[\eta_1, \ldots, \eta_ {n-1}]\). 

<br/><br/>
<b>Claim: </b><i>\(T\) is a point for \(\text{SMan}\).</i>
<div style="border-left-style:solid;padding-left:5px;">
Let \(f, g:X \rightarrow Y\) be maps in \(\text{SMan}\) and assume that \(f
\neq g\). If \(f_0, g_0: X_0 \rightarrow Y_0\) are not equal then we can find
some point \(x: \mathbb{R}^{0|0} \rightarrow X\) on which they disagree.
Conversely assume that \(f\) and \(g\) induce the same map on the topological
spaces \(X_0 \rightarrow Y_0\). Then as they are different we can find an \(x_0
\in X_0\) such that the maps of local algebras \(C^\infty_{Y, f_0(x_0)}
\rightarrow C^\infty_{X, x_0}\), \(f^{\#}_ {x_0}, g^{\#}_ {x_0}\) are
different. We now take local coordinates of \(X\) and $Y$ about \(x_0\) and
\(f_0(x_0)\) so that we can describe the maps as functions on the local
algebras: \(C^\infty_{\mathbb{R}^{n|m}, 0} \rightarrow
C^\infty_{\mathbb{R}^{p|q}, 0}\). Now we Taylor expand the odd coefficients and
we find:
<p align="center">
\[ f^ * (y^i) = f^i(x) + f^i_{a_1 a_2}(x)\eta^{a_1}\eta^{a_2} + \cdots,
\hspace{4mm} f^ * (\xi^m) = f^m_a(x) \eta^a + \cdots.\]
</p>
Now one of the \(f^i_ {a_1 \cdots a_{2l}}\) or \(f^m_{a_1 \cdots a_{2l + 1}}\)
must be different to the corresponding term in the expansion of \(g\). Thus we 
can find some \(t_0\) (not necessarily equal to 0) such that the values of 
these functions are different at \(t_0\). Then the point
\(t: \mathbb{R}^{0|q} \rightarrow X\) which is induced by the map
\(\mathbb{R}^{0|q} \rightarrow \mathbb{R}^{p|q}\) which is the identity on the
odd coordinates and sends the singleton to \(t_0\) on the bodies. We have that \(f\circ x \neq g \circ x\). It follows we
can find a point \(\mathbb{R}^{0|p} \rightarrow X\) that separates \(f\) and \(g\).

This point induces an element of \(X(T)\) as all the inclusions in \(T\) have
natural cosections and the result follows.
</div>
<br/><br/>

By the last part of the previous section we can now think of the function
spaces in \(\text{SMan}\) as a set and so we will use terminology like
\(f \in \underline{\text{Hom}}(X, Y)\) in the future. This is important for helping us write
down functions between superspaces, for expressing differential equations etc.
</p>


<h4>The ring of odd constants</h4>
<p>
To complete this note we want to make reference to the ring of odd constants.
Note that \(T\) doesn't define an object in \(\text{SMan}\) but it does define
an ind-object. It is useful to look at the algebra of functions of this space,
namely it is the projective limit of \(\varprojlim \mathbb{R}[\eta_1, \ldots,
\eta_n]\). We call this ring \(\Lambda\) which can be though of as a ring
containing free odd constants of arbitrary dimension (so we can always choose
some odd constants from the ring that do not satisfy any finite number of
equations except for the defining ones). Then when we define any functions 
between spaces we can just take constants from this ring rather than
\(\mathbb{R}\), or equivalently we can consider the category \(\text{SMan}
/ \text{Sp}(\Lambda)\) <a id="rfootnote-3" href="#footnote-3">[3]</a>.
<br/><br/>
For example let us consider functions \(\mathbb{R}^{1|0} \rightarrow
\mathbb{R}^{0|1}\). Using the above we can consider such a function by \(f^ * (\eta) = u(t) \lambda\) for some
odd constant \(\lambda \in \Lambda_ \text{odd}\) and \(u(t) \in C^\infty(\mathbb{R})\).
Similarly we can look at maps \(\mathbb{R}^{0|1} \rightarrow
\mathbb{R}^{1|0}\), these are then defined by \(f^ * (t) = c + \lambda \eta\)
for \(c \in \Lambda_ \text{even}\) and \(\lambda \in \Lambda_ \text{odd}\).
</p>

<p id="footnote-1"><a href="#rfootnote-1">[1]</a> This is a form of humour</p>

<p id="footnote-2"><a href="#rfootnote-2">[2]</a> We actually want a
generalized point to be some terminal object in the category of diagrams that 
represent concrete forgetful functors. I won't go into this here but is 
potentially interesting</p>

<p id="footnote=3"><a href="rfootnote-3">[3]</a>The space
\(\text{Sp}(\Lambda)\) is sometimes called a "fat point".


