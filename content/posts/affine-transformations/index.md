---
title: The geometry of linear transformations (old)
katex: true
draft: true
date: "2020-02-21"
markup: "pandoc"
---


If your experience with linear algebra was anything like mine, the thought of applying linear transformations on vectors quickly brings back boring memories of robotically crunching numbers and reviewing repetitive proofs drenched in complex notation. In this post, I plan to leave all of that out and remind ourselves that although many linear algebra problems have a mechanical feel, there are underlying geometric intuitions that are powerful. I wish I knew of a good way to include graphics of vectors and matrices which I could use to display these geometric intuitions (Who doesn't like a few pretty pictures?). Unfortunately, I haven't figured out how to do that yet, so we'll need to resort to some imagination.

## Linear transformations

Let's start with a vector in $\mathbb{R}^2$.

$$
\vec{x} = \begin{bmatrix} x \\ y \end{bmatrix}
$$

Linear algebra is a branch of math that focuses on the ways that we can manipulate our vectors using **linear transformations**. What makes linear transformations different from other mathematical operations is that we can represent them as a matrix. Here is a general template for how these transformations look - it’s an equation describing **matrix-vector multiplication** in two dimensions.

$$
A \cdot \vec{x} = \vec{b}
$$

$$
\underbrace{\begin{bmatrix}
a_{11} & a_{12} \\ a_{21} & a_{22}
\end{bmatrix}}_{A\text{ = map to apply}}
\begin{bmatrix} x \\ y \end{bmatrix}
=
\underbrace{\begin{bmatrix}
a_{11} x + a_{12} y \\ a_{21} x + a_{22} y
\end{bmatrix}}_{b \text{ = resulting point}}
$$

Here we are transforming the $\vec{x}$ with the linear transformation $A$ to produce a new vector in $\mathbb{R}$ called $\vec{b}$.

At first glance, I don’t think it is clear how to interpret what the matrix $A$ is doing to our vector. What is the relationship between vector $\vec{x}$ and $\vec{b}$? Grant Sanderson (3blue1brown on youtube) has a [linear algebra series](https://www.youtube.com/watch?v=fNk_zzaMoSs&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab) with some great animations that breaks down how matrix multiplication. The video series got me thinking of vectors as geometric quantities. There are a few types of linear transformations on vectors that stood out to me for their geometric impacts:

1. Linear transformations that **scale** our vector
2. Linear transformations that **sheer** our vector
3. Linear transformations that **rotate** our vector
4. Linear transformations that **translate** our vector

Let's take a look at the matrix that defines each one.

## Scaling vectors

Scaling is by far the easiest and least interesting of theseoperations. If we have a matrix with only entries in the diagonals (aka a diagonal matrix), then each row of the $\vec{x}$ is scaled by the value in the corresponding value in the diagonals.

$$
\begin{bmatrix}
a & 0 \\ 0 & b
\end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
=
\begin{bmatrix}
a \cdot x \\
b \cdot y
\end{bmatrix}
$$

## Sheering Vectors

If the diagonal entries scale our vector, what do the non-diagonal entries do? Consider the following transformation.

$$
\begin{bmatrix}
a & b \\ 0 & b
\end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
=
\begin{bmatrix}
a \cdot x + b \cdot y \\
b \cdot y
\end{bmatrix}
$$

This transformation is similar to the one that translates the matrix with one small difference: The first coordinate is no longer independent of the second coordinate. **The shear moves the coordinate towards one of the other coordinates.** When you use a pair of scissors for cutting, the top blade moving closer and closer to the bottom blade. This type of behavior is exactly in line with what is happening to our axis.

## Rotating Vectors

To rotate a vector by some angle $\theta$, we need to be able represent our angle movement as a sum of products. First, we should start by reminding ourselves that vectors themselves can be represented in terms of the angle that they form with the $x$ axis. This representation is referred to as the **polar coordinate system**. For example, with some basic trigonometry, we can represent the vector that we saw before $\vec{x}$ as follows.

$$
\vec{x} =
\begin{bmatrix} x \\ y \end{bmatrix}
=
\begin{bmatrix}
\|
\vec{x} \| \cdot \cos \phi
\\ \|
\vec{x} \| \cdot \sin \phi
\end{bmatrix}
$$

So if we rotate the matrix by $\theta$ degrees, we are effectivly increasing the angle between the vector and the x-axis:

$$
\vec{x}_{\text{rotated}} =
\begin{bmatrix}
\| \vec{x} \| \cdot \cos( \theta + \phi) \\
\| \vec{x} \| \cdot \sin( \theta + \phi)
\end{bmatrix}
$$

And now we can make use of the [trigonometric formulas for the sum of angles](http://mathworld.wolfram.com/TrigonometricAdditionFormulas.html)!

$$
\begin{bmatrix}
  \| \vec{x} \| \cdot \cos( \theta + \phi)
  \\
  \| \vec{x} \| \cdot \sin( \theta + \phi)
  \end{bmatrix}
  =
  \begin{bmatrix}
  \| \vec{x} \| (\cos\phi \cos\theta - \sin\phi \sin\theta)
  \\
  \| \vec{x} \| (\sin\phi \cos\theta + \cos\phi \sin\theta)
  \end{bmatrix}
$$

$$
\begin{bmatrix}
  \| \vec{x} \| (\cos\phi \cos\theta - \sin\phi \sin\theta)
  \\
  \| \vec{x} \| (\sin\phi \cos\theta + \cos\phi \sin\theta)
  \end{bmatrix}
  =
  \begin{bmatrix}
  \| \vec{x} \| \cos\phi \cdot \cos\theta -
  \| \vec{x} \| \sin\phi \cdot \sin\theta
  \\
  \| \vec{x} \| \sin\phi \cdot \cos\theta +
  \| \vec{x}\| \cos\phi \cdot \sin\theta
  \end{bmatrix}
$$

$$
\begin{bmatrix}
  \| \vec{x} \| \cos\phi \cdot \cos\theta -
  \| \vec{x} \| \sin\phi \cdot \sin\theta
  \\
  \| \vec{x} \| \sin\phi \cdot \cos\theta +
  \| \vec{x}\| \cos\phi \cdot \sin\theta
  \end{bmatrix}
  =
  \begin{bmatrix}
  x\cdot \cos\theta
  -y \cdot \sin\theta
  \\
  y \cdot \cos\theta +
  x \cdot \sin\theta
  \end{bmatrix}
$$

Finally, by separating out our original vector, we end up with the following representation:

$$
\begin{bmatrix}
x \cdot \cos \theta - y \cdot \sin \theta \\
x \cdot \sin \theta + y \cdot \cos \theta
\end{bmatrix}
=
\begin{bmatrix}
\cos \theta & - \sin \theta \\ \sin \theta & \cos \theta
\end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
$$

## Translating Vectors

Understanding each of these transformations involves progressively building up more and more knowledge about our coordinate grid. Scaling involved looking at a few common conditions. Sheering involved figuring out how each element of the matrix was contributing to the matrix multiplication operation. And rotations involved us using these parts in unison with a different view of our grid system. Although this last transformation is geometrically simple, it forces us to consider the limitations denoted our grid system.

If we have a translation matrix, then we should be able to use it to translate any vector $\vec{x}$. This includes the zero vector, deonted by $\vec{0}$.

$$
\underbrace{\begin{bmatrix} 0 \\ 0 \end{bmatrix}}_{\vec{0}}
$$

The problem is, there are no values that we can throw into our matrix that will transform our zero vector. That's engraved into the definition of vector spaces.

$$ A \cdot \vec{0} = \vec{0} $$

$$
\begin{bmatrix}
a_{11} & a_{12} \\ a_{21} & a_{22}
\end{bmatrix}
\begin{bmatrix} 0 \\ 0 \end{bmatrix}
=
\begin{bmatrix}
a_{11} \cdot 0 + a_{21} \cdot 0 \\ a_{12} \cdot + a_{22} \cdot 0
\end{bmatrix}
= \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

So there is no two-dimensional matrix which we can use to translate our point. Interestingly enough, however if we add another dimension, we can get our desired effect using the following matrix.

$$
\begin{bmatrix}
a & 0 & t_{x} \\
0 & b & t_{y} \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
=
\begin{bmatrix} a \cdot x + t_{x} \\ b \cdot y + t_{y} \\ 1 \end{bmatrix}
$$

The first time I saw this matrix, I thought it was complete magic! However, I recently discovered that this matrix can be thought of **two-dimensional shear of three-dimensional space**! For example, if I fan a deck of cards, I am essentially sheering the deck along its depth axis, all while the card simply moves along the top slice. This is the same thing the equation above is doing. It's sheering the third axis and displaying the result in two dimension which intern looks identical to a translation.

In my computer graphics course, these intuitions come up over and over again so it was worth taking some time to review them. After writing this post, I saw this other lovely exposition which I highly recommend. It even has the diagrams that this post lacks! [A Geometrical Understanding of Matrices](http://gregorygundersen.com/blog/2018/10/24/matrices/)
