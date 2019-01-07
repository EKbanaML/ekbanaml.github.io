{% include head.html %}

## Linear Combination
In the previous article, we saw how scaling a vector would scale the vector up or down,
however without changing the direction. When we scale a vector by some constant,
it always spans over the line formed by the original vector. Let us see the example by scaling our
vector `(2, 4)` by few constant.

![Scalar Combination of vector]({{'static/images/LRintro/Scalar Combination .png' | absolute_url}})

Here `u` vector ending at the point `A` when scaled by `2`, we get the vector `v` ending at the point `D`,
when we scale it with `3` it gave us the vector `w` ending at point `B`. When we scale the vector `u`
with `-1`, it gave us the negative vector `u’`. There is a pattern here, no matter with what number
do we scale the vector `u`, it will always end up in the line span by the original vector `u`.
It will never go away from the line, hence preserving its original directionality.

We say that the scalar combination of this vector spanning the line in $${R^2}$$.
Let `V` be a vector space over the `FIELD K`. For any vectors `v1, v2, v3, ………, vn ∈ V & x1, x2, x3, ……………. , xn ∈ K`,
the expression of the form `x1v1 + x2v2 + x3v3 + …………….. + xnvn` is called the
linear combination of the vector `v1, v2, v3, …. vn`.
We haven’t yet gone through the field and vector space,
however we can just pick the concept of linear combination and move further.

Let us go further and see what happens when we form the linear combination of two vectors.

```

U = (2, 4) , V = (-1, 3)

Let’s pick c1 = 2 and c2 = -1

c1. U + c2 . V  = 2 (2, 4) + (-1) (-1, 3)

= (4, 8) - (-1, 3)

= (5, 5)

```

![Scalar Combination of vector]({{'static/images/LRintro/Linear-Combination-in-2d.png' | absolute_url}})

Let’s change our constant `c1` and `c2` and get another resultant.

```
c3 . U + c4. V

= -0.5(U) + 3 (V)

= (1, 2) + (-3, 9)

= (-2, 11)

```
Let’s get a combination consisting totally negative scalars:

```
c5 = -1, c6 = -2

c5.U + c6.V = -1(2, 4) + (-2) (-1, 3)

= (-2, -4) + (2, -6)

= (0, -10)

```
Let’s plot this together:
![Scalar Combination of vector]({{'static/images/LRintro/Linear-Combination-in-2d (1).png' | absolute_url}})

Adding the last linear combination, the above graph would look like this:

![Linear Combination of 2-d vector]({{'static/images/LRintro/Linear-Combination-in-2d (2).png' | absolute_url}})

There is something to notice here, and that is:

The linear combination of the two independent 2-d vectors are spanning a plane in $${R^2}$$.
No matter what constants we choose to derive the resultant from these two vectors, they would always
lie in the $${R^2}$$ spanned by these vectors. If we add a new 2-d vector, it too would lie in the
plane spanned by these two vectors. Isn’t that interesting?

Similar thing happens in 3-d and higher dimension as well.

Here by plane we are referring to a flat, two-dimensional surface that extends infinitely far.
A plane is the two-dimensional analogue of a point (zero dimensions), a line (one dimension) and three-dimensional space.
Planes can arise as subspaces of some higher-dimensional space, as with a room's walls extended infinitely far,
or they may enjoy an independent existence in their own right, as in the setting of Euclidean geometry. [1]
A plane has no thickness and goes on forever.
![a plane]({{'static/images/LRintro/image2.gif' | absolute_url}})
Figure reference [2]

A plane is spanned by two independent vectors. (Mind this word independent for it’s significance because we will come
in the detail about this in the later articles.  However if we have more than two independent vectors, would
give us the space. Say, `x, y and z` were three independent vectors, then
their linear combination `cu + dv + ew` would fill the whole 3-d space. In the above example figure, there wouldn’t
be any vector in 3-d space that couldn’t be resulted by the linear combination of `x, y and z vectors`.
When the directionality keeps on increasing which is the additional vector that we’re going to add isn’t  coplanar with other vectors, we keep going towards the higher dimension.


### References:

1. [https://en.wikipedia.org/wiki/Plane_(geometry)](https://en.wikipedia.org/wiki/Plane_(geometry))
2. [http://mathworld.wolfram.com/Plane.html](http://mathworld.wolfram.com/Plane.html)
3. [Vector images drawn using Geogebra](https://www.geogebra.org/?lang=en)








