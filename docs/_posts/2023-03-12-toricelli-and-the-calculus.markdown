---
layout: post
title: "Toricelli and the calculus"
date: 2023-03-12
categories: calculus toricelli
usemathjax: true
---

Have been reading an excellent <a href="https://www.wob.com/en-gb/books/assoc-prof-amir-alexander/infinitesimal/9781780746425">book on the history of the calculus and infinitesimals</a>
and there is a wonderful conclusion to a paradox for which his solution is
profound and feels to me like a first true understanding of the nature of the
continuum and it is deduced by considering the simplest situation, that of a 
rectangle.

Lets start with the paradox – consider a rectangle say of width 2 units and
height one unit and its diameter (from the
bottom left to top right). 
<p align="center">
  <img src="/assets/toricelli-rectangle.png" />
</p>
At any point on the diameter we can draw
a horizontal line for the upper triangle and a vertical for the lower triangle.
It is clear that the horizontal line is always longer than the vertical one.
Moreover all these lines cover both the upper and lower triangles. So the union
of the horizontal lines should be “larger” than those of the vertical which is
obviously contradictory as the two triangles are identical. This situation was
cause (not exclusively) for many mathemeticians to throw out the infinitesimal
as not being a valid mathemtical construct.

Toricelli came up with a solution that is both simple and yet profound.
His idea was to say that yes the two triangles are equal and this is because
the two lines have different thickness! In fact their thickness should be such
that the given "union" makes the areas equal.

Let us now rewrite this in modern syntax, we have that the line is given by
$$y=kx$$ where in our example $$k = \frac{1}{2}$$. Then the width of the horizontal
lines is given by $$\text{d}y$$ and the vertical by $$\text{d}x$$ and we see that
exactly offset so that $$x\text{d}y = y \text{d}x$$.
