{% include head.html %}
In the second article of our series, we saw how two independent vectors  span the whole 2-d plane,
and how three independent vectors span the whole $${R^3}$$ space.
But we merely cared what  we were referring as independence.
So in this article, let us discuss about the independence and dependence of vectors.

**Note** : For reminder $${R^3}$$ is described
as the 3-d space spanned by vectors consistings of real numbers, hence the capital letter $${R}$$)

## Linear Dependence
Let `V` be a vector space over the `Field K`. Then a set of vectors `v1, v2, v3, ………………, vn` are called linearly
dependent vectors if there exists ‘a’ set of scalar, `a1, a2, a3 , …………… an`, and not all zero,
such that `a1v1 + a2v2 + a3v3 + ………………………+ anvn = 0` .

If we take our previous example of $${R^2}$$ where we derived new vectors by the
linear combination of first two vectors `U` and `V`.
![Linear Combination of 2-d vectors]({{'static/images/LRintro/Linear-Combination-in-2d (2).png' | absolute_url}})

All the other vectors that we derived originated by some combination of `U` and `V`,
in this case all the other vectors resulting from the linear combination of `U` and `V` are linearly dependent.
Hence the `U` and `V` vectors are sufficient to span the 2-d plane in this example.

## Linear Independence:
Let us now introduce the formal definition of linear independence.

Let `V` be a vector space over the `Field K`. Then a set of vectors `v1, v2, v3, ………………, vn` are called
linearly independent vectors if there exists `a` set of scalar, `a1, a2, a3 , ……………, an`, and all zero,
such that `a1v1 + a2v2 + a3v3 + ………………………+ anvn = 0`.

You must have noticed something quite reverse than the definition of linear dependence here,
because that is the case. If two or more vectors are linearly independent and until and unless all
the constant `ai` are not zero, their linear combination can never yield the zero vector.
The other way to understand the concept of linearly independent vectors is that, some unique linear combination
of these independent vectors would yield any vector in that space. If we take the above example, two linearly independent
vectors `U` and `V` can yield any vectors in the $${R^2}$$ space. There is no vector in the  space which
the linear combination of these two vectors can not yields.
Each independent vector adds an unique directionality giving us the new space.

`X = [2 , 3, -1] , Y = [-1, 0, 0] , Z = [0, 0, -5] `

These three vectors are independent since no linear combination of the two vectors can yield the other one,
spans the 3-d space. Since they are linearly independent, some unique linear combination of
these vectors can yield every point in the space.

## More Examples Of Linear Dependence & Independence
Another way of understanding dependence and independence would be that, dependence as the word describes tells us
that  there is always some redundant vectors which can be constructed with the linear combination of independent vectors.
Hence the independent vectors however are not redundant. Linear independence is an important principle in
understanding the basis vector which we will describe later.

Let us take few examples before we head for some small proof:
```
U = (2, 4)

V = (-1, 3)
```

Let’s test the dependence of these vectors:

Just by seeing, we can’t see any scalar combination by which we can scale one of the vector and get another.
So they seems to span $${R^2}$$ . Let us explore more:

Assume `c1` and `c2` are some constant over the field of $${R}$$ (Real Numbers). And the linear combination of the
vectors `U` and `V` be:
```
c1.U + c2. V

c1(2, 4) + c2 (-1, 3)

(2c1 - c2, 4c1 + 3c2)
```

For these vectors to be linearly dependent, the following condition must hold:
1. There must be some ci which is not equal to zero (in our case it is either c1 or c2 or both)
2. The linear combination of those vectors should yield zero vectors

Hence

```
(2c1 - c2 , 4c1 + 3c2) = (0, 0)

2c1 - c2 = 0   ------------------- (i)

4c1 + 3c2 = 0 ------------------ (ii)


Let’s multiply equation (i) by 4 and (ii) by 2 and subtract (ii) from (i)

8c1 - 4c2 = 0

- (8c1 + 6c2) = 0

___________

-10c2 = 0

C2 = 0
```

Substituting the value of `c2` in the equation `(i)`, we get `c1 = 0` as well.

Seems like these two vectors didn’t meet the
condition of linear dependence, but of the linear independence.
If any set of vectors to be linearly independent, then their linear combination yields zero vector
if and only if, all the coefficients `(ci)` are zero. Which is the case of above example.

However this case doesn’t works reverse, the condition is to be read as it is, because if you multiply anything by zero,
it would always yield zero. In case of linear independence, the combination will yield zero if and only if all the
coefficient `ci` are zero. However in case of dependence having one/some/all non-zero coefficient, we can still yield the
zero vector.

**Note**: We can not multiply linearly dependent vector by 0 and say that the set of vectors are linearly independent.
Place any constant other than 0 in the above combination of two vectors U and V, you will never get the zero vector.

Now, let us introduce another 2-d vector W and check the linear dependence:

`W = (1, 2)`

Now our original linear combination becomes something like this:

```
c1.U + c2.V + c3.W

c1(2, 4) + c2 (-1, 3) + c3 (1, 2)

(2c1 - c2 + c3, 4c1 + 3c2 + 2c3)

For these vectors to be linearly dependent, some ci must be greater than
or equal to zero and their linear combination would still yields zero vectors.

(2c1 - c2 + c3, 4c1 + 3c2 + 2c3)  = (0, 0)

2c1 - c2 +c3 = 0 ---------------------(i)

4c1 + 3c2 + 2c3 = 0 ------------------(ii)

We have three coefficients to solve, let’s recall the above statement of
some ci not being euql to zero.

Assume c3 = - 2

2c1 - c2 -2 = 0 -----------------------(iii)

4c1 + 3c2 -4 = 0 -------------------- (iv)

Let’s multiply equation (iii) by 4 and (iv) by 2 and subtract (iv) from (iii)

8c1 - 4c2 - 8 = 0

8c1 + 6c2 - 8 = 0

- _____-______-____

          -10c2 = 0

          c2 = 0

Now let’s replace the value of c2 in equation (iii), we get:

2c1 - 0 -2 = 0

2c1 = 2

c1 = 1


Now back to our condition of linear dependence:

c1(2, 4) + c2 (-1, 3) + c3 (1, 2)  = (0, 0)

1 (2, 4) + 0 (-1, 3) - 2 (1, 2) = (0, 0)

(2, 4) - (2, 4) = (0, 0)

(0, 0)
```

So from the above example what we proved is that, having non-zero constant `c1` and `c3` we can still get the zero vector
from their linear combination. Hence set of vectors formed by `U, V and W` are linearly dependent because one of them is
already redundant to span the 2-d space. Hence often n independent vectors are sufficient to span the n-dimension.

**Your Go**: Write down three dimensional vectors `u, v and w` :

`u = (0, 1, 0) , v = (-3, 0, -2) , w = (0, -5, 2)`

Check their linear independence, now add another vector `z = (1, 3, 0)` and see the set of
vectors `u, v, w , z` are still linearly independent.

## Algebric Proof Of Linear Dependence:

Let us now have a small algebraic proof of linear dependence:

```
S = { v1, v2, v3, ……………………, vn} is said to be linearly dependent

Iff c1v1 + c2v2 + c3v3 + …….. Cnvn = 0 = [0, 0, 0, ……………… 0]

For some ci is not equal to zero.

c1v1 + c2v2 + c3v3 + …….. cnvn = 0

V1 = c2v2 + c3v3 _ …………. + cnvn


If this is linearly dependent then:

0 = -1v1 + c2v2 + c3v3 + ……………….. + cnvn

Assuming that c1 is non-zero (-1)

c1v1/c1 + c2v2/c1 + c3v3/c1 + ………………….cnvn/c1 = 0

Then:

c2v2 /c1 + c3v3/c1 + ………. + cnvn/c1 = -v1

V1 = -c2v2/c1 - c3v2/c1 - …………… - cnvn/c1
```

As you just saw, how the v1 was formed from the linear combination of other vectors, when the set of vectors are
linearly dependent.

## Key Points:

1. The linear combination of n independent vectors will always yield another coplanar vector which lays on the same n-space.
2. No combination of set of linearly independent set of vectors can result any one/other independent vector in that set.
3. N independent vectors fully spans the N-dimensional space.
4. Linearly dependent vectors are also coplanar, means they are in the same plane/space.
5. When a unique vector adds a new directionality, (for which the vector must have a different plane) or say should cross the existing plane, the span of the original set of vectors is increased.
6. Linearly combination of n independent vectors can create infinitely many vectors in the n-dimensional space.
7. These concepts are key for understanding the linear subspace and basis vectors.

## Final Thoughts

**Finally Leaving you with an example**:
Say, you bought two pieces of carrot cake, a piece costing you Rs 50, so your vector of (quantity, price) would look like:

`(1, 50)`
Say since you bought `2` pieces of carrot cake, you want to construct another vector:
`(2, 100)`

In this case the second resultant vector is already redundant, it is not adding any new information to our
original vector of carrot cake, hence this set of vector is linearly dependent.
If we however only took the first vector, it’s giving us the unique information of the price of carrot cake,
relative to its quantity and is independent in itself.





