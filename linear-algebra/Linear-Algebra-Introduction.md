## Introduction

This is the first article of the Linear Algebra series, and in this article we will be discussing about the following fundamental topics of Linear Algebra: 

- Vector & Linear Combination 
- A Line, Plane & Space
- Dot Product, Vector Norm, Unit Vector
- Inequalities 
- Cross Product 


Before we move to the concepts of vector, let us start from the beautiful concept as of scalar. It is anything that is presented in terms of numbers. Mostly representing something physical as your weight, age or even the number of blood cells in your body. 
However if we go into the more theoretical definition of the scalar, it is a physical quantity that can be described by a single element of a number field such as real numbers often accompanied by the unit of measurements. A scalar has a magnitude, like if I asked you about your age, and you happened to reply, 22. It is scalar, it describes the length of time you’ve lived. `[1] `

Let us now, take a step further and add some more characteristics to the scalar, Let us assume that you’re a salesperson in the bakery shop beside the road where I usually eat in the evening. If I asked you about the price of a carrot cake, you would say Rs. 50, here 50 is a scalar, now you added few other attributes to the carrot cake and said that for the 50g carrot cake having a blue color packaging, it would cost Rs 50, but for 25g carrot cake with green color packaging it would cost about Rs 25. Then amazingly you just gave an answer in terms of a vector. Here the price is relative of the weight and the package color of the carrot cake, and forms two vectors `(50g, blue)`, `(25g, green)`. Now this has both magnitude and direction. Weird word they are, which we will discuss later.  As we added further characteristics in the carrot cake- packaging, blue and green. We can keep on adding more attributes as much as we want,
Our Vectors `V1` would be `(50g, Blue)` and `V2` would be `(25g, Green)` 
And `V3 = (50g, Green)` 


We could later change their unit measurement to make it scalar in regards to the third attributes packaging. 

# Vector A Formal Definition: 
A vector is a kind of abstract object that can be added and scaled, giving the same kind of object back, satisfying a few more properties which we shall see later. 

Arrows in 2D space with length and direction but no location are vectors
Ordered pairs of real numbers are vectors. You can add them like so: `(1,2)+(3,4)=(4,6)(1,2)+(3,4)=(4,6)`. You can scale them like this: `2(1,2)=(2,4)`
Real-valued functions of a real variable are vectors. To add two functions together, add them pointwise, for example, if `f(x)=sinx and g(x)=cosx`, then `h= f+g` satisfies `h(x)=sinx+cosx`. Scaling is similar: if `f(x)=sinx`, then `h=kf` satisfies `h(x)=ksinx`. 
`[2] `

In machine learning problems, a set of features representing some outcome is supposed to be the vector. Normally their linear combination is supposed to give us our desired outcomes but if only they are fully linear. 

Now let us move on to another beautiful concept “Linear Combination” . Let us understand through a simple example before we move towards the theoretical explanation: 

Let us again go to the bakery shop in the evening, when people are rushing towards home, and stay as you have no home to reach and order few things: 
i) 2 pieces of 50g carrot cake with blue packaging costing Rs. 50 per each.  

After the bakery, we felt the growing winter and realized that we had to buy socks. Let us go to the winter shop:  

i) a gray jacket made using 200g of original plumes, costing Rs 1200. 


So let us do something interesting, from our purchase let us form two vectors: 
`V1 = (50g, blue)`  :vector of the carrot cake
`V2 = (200g, gray)` :vector of a socks that we bought. 

Here we can form a trivial linear combination of the purchases that we made: 

Now we have the quantity which are scalar, let us form the linear combination of our evening activities: 

```2 V1 + 1 V2 
2 (50g, blue) + 1 (200g, gray)  
(100, 2 blue) + (200, gray) 
(100+200, 2blue + gray)
(300g, 2blue+gray)  ```


This is a hypothetical vector having the magnitude of our total expenses. Combining addition with scalar multiplication, we now form "linear combinations" of vectors `V1` and `V2`. We multiply `V1` by `c` and multiply `V2` by `d` then add `cV1` + `dV2`.

This is the linear combination between the vectors that we created by entering into the  bakery shop and winter shop. It is a trivial example and there could be a more scientific one to explain further. Let’s suppose your position right now is the origin of a cartesian plane, you threw a ball and it reached the coordinate point `(7, 10)`. Again you threw another ball into another direction it hit a wall and descended further through the point you were standing, finally reaching a point `(-3, -5)`. Assume they are two vectors, having a magnitude of `$\sqrt{149}$` and `6` respectively. 

If we add these two vectors, We would get another vector `(4, 5)`; a diagonal of the parallelogram formed by the original two vectors. If we choose 2, 3 as a scalar for the original vector then, their linear combination would fill a 2d plane. 
`2( 7, 10) + 3 (-3, -5) = (5, 5)`   


Now let us go further towards more theoretical definition of the linear combination. Something to note in the vector, vector can consists the component of different unitary measurement.


# Linear Combination - A formal definition: 

Let `V` be a vector space over the `FIELD K`. For any vectors `v1, v2, v3, ………, vn ∈ V `& `x1, x2, x3, ……………. , xn ∈ K,` the expression of the form `x1v1 + x2v2 + x3v3 + …………….. +` `xnvn` is called the linear combination of the vector `v1, v2, v3, …. Vn.`

Since, we haven’t yet said anything about the vector field and vector space, let’s just understand that, we scale each vector in a particular vector space with some scalar, and summing all vectors in the vector space. We will come to the significance of these linear combination as we go along the further topics in vectors eg: vector space, basis vector, linear dependence and independence. 

# A Line, Plane & Space: 
A vector is something that has both magnitude and direction, that is as plain as the point representing two coordinate axes in the Euclidean space. Say, the co-ordinate, `(2,3)` can be a vector by drawing the line from the origin. If you graph it, you will get a vector with the magnitude of `sqrt(15)` and some direction theta. 

The linear combination of a single vector as `(2,3)` which would be `cv` would form a line, similarly the linear combination of two vectors` v = (2, 3)` and` w = (4, 2) `
`cv + dw`  would form a plane. 


![Figure Reference: 4]({{'static/images/LRintro/image1.jpg' | absolute_url}})

Here, by plane we are referring to a flat, two-dimensional surface that extends infinitely far. A plane is the two-dimensional analogue of a point (zero dimensions), a line (one dimension) and three-dimensional space. Planes can arise as subspaces of some higher-dimensional space, as with a room's walls extended infinitely far, or they may enjoy an independent existence in their own right, as in the setting of Euclidean geometry. [3] A plane has no thickness and goes on forever. 

![Figure Reference: 5]({{'static/images/LRintro/image2.gif' | absolute_url}})

A plane is spanned by two independent vectors. (Mind this word, independent for it’s significance because we will come in the detail about this in the later articles. However if we have more than two independent vectors, or right here lets just understand with more than 2 non-collinear cartesian  points, would give us the space. Say, `u`, `v` and `w` were three independent vectors, then their linear combination `cu` + `dv` + `ew` would give us the space. Space is probably used here to denote the higher dimensional vectors. 

Before we move further, let us see an interesting phenomenon we can get from vector addition. If two vectors acting simultaneously at a point can be represented both in magnitude and direction by the adjacent sides of a parallelogram drawn from a point, then the resultant vector is represented both in magnitude and direction by the diagonal of the parallelogram passing through that point. 

This is called parallelogram law of vector addition. `[6] `


the sum of the vectors `u = (3, 4)` and `v = (4, 1)` in the plane

![Figure Reference: 7]({{'static/images/LRintro/image3.gif' | absolute_url}})
When we add two vectors, their resulting vector from their sum would also lie in the same plane, and this is true for in higher dimension as well.  


# Dot Products, Norm & Unit Vectors:
There are subtle reasons, why we’re putting dot products, vector norm and unit vector together; It is for the sake of simplicity and probably brings coherent clarity. 
Consider two vectors `u` and `v`, the resulting value of `u.v` is known as dot product, it is a scalar value. 
If the dot product between two vectors `u` and `v` is actually `0`, then both vectors are perpendicular to each other. The dot product gives us a scalar. 
For example `u = (1, 4)` and `v = (2, 1)` then the dot product of these two vectors `u.v` would be: 

	`(1, 4) . (2, 1) = 1 x 2 + 4 x 1 = 6`

`|| v ||` Length or Norm or magnitude of a vector is the square root of dot product with itself. e.g 

	`|| v || = $\sqrt{(v. v)}$`
	
From the above vector v, it would be: 

	|| v ||  = $\sqrt{($2^2$ + $1^2$)}$ 

A unit vector is any vector whose length or magnitude is actually `1`. For example:

	```v = (1, 0) 
	$\sqrt{(1 + 0)}$ = 1. ```
	 
	
`u = v/ll v II` is a unit vector in the same direction as `v`. 
From the above vector `v`, the unit vector would be 

	(2, 1)/ $\sqrt{5}$ = (0.89443, 0.44721) 
Calculating the norm of this resulting unit vector, we would derive `1`. 
 
When `v` and `w` are perpendicular, they form two sides of a right triangle. The third side is `v - w` (the hypotenuse going across). 

The Pythagoras Law for the sides of a right triangle is

	$a^2$ + $b^2$ = $c^2$ 

Perpendicular vectors `|| $v^2$ ||  + || $w^2$ || = $(|| v - w||)^2$`

The angle is less than `90°`when `v . w` is positive. The angle is above` 90°` when `v . w` is negative.


If `u` and `v` are non-zero vectors then `cos(theta) = (u.v/ ||u|| ||v|| ) `


Start with unit vectors `u` and `U`. The sign of `u • U` tells whether `theta < 90°` or `theta > 90°`. Because the vectors have length `1`, we learn more than that. The dot product `u • U` is the `cosine` of `d`. This is true in any number of dimensions.  
Unit vector  `u . U` at angle `theta` we have `u· U = cos(theta)`. Remember that `cos(d)` is never greater than `1`. It is never less than `-1`. The dot product of unit vectors is between `-1 and 1` 



![Figure Reference: 8]({{'static/images/LRintro/image4.png' | absolute_url}})

From the above backgrounds, now we can move onto another interesting concept. 
# Inequalities: 

Whatever the angle, this dot product of `v / || v ||` with `w / || w ||` never exceeds `1`. That is __"Schwarz inequality"__ `|v. w| =< ||v|| ||w||`
Similarly the cosine formula gives us yet another interesting phenomenon, triangle inequality. `|| v + w || < || v || + || w ||`

The __Schwarz__ inequality tells us that the magnitude of dot product of two vectors is always less than or equals to the multiplication of their norms. Extending this concept we get another inequality which is triangle inequality. 
Geometrically, the right-hand part of the triangle inequality states that the sum of the lengths of any two sides of a triangle is greater than the length of the remaining side. `[9] `
      
![Figure Reference: 10]({{'static/images/LRintro/image5.png' | absolute_url}})

Depicted as in the figure, the triangle inequality tells that, the magnitude of sum of two sides of the triangle would be less than or equal to the summation of their individual magnitude. Since mostly in machine learning problems we would work with Euclidean space, in which these inequalities are already the part of the space. For example the triangle inequality is used to accelerate the K-means clustering `[11]`. Citing a cross Validation: 

“The optimized algorithm is based on the fact that most distance calculations in standard K-means are redundant. If a point is far away from a center, it is not necessary to calculate the exact distance between the point and the center in order to know that the point should not be assigned to this center. Conversely, if a point is much closer to one center than to any other, calculating exact distances is not necessary to know that the point should be assigned to the first center.”  `[12]`

# Cross Product: 
Finally, we’re on the final topic of this article, cross product of vectors. The cross product between two vectors u and v gives us the new vector w which is perpendicular to the both vectors u and v. There are some nice properties that the cross product can give us, which are listed below: 
The cross product is defined by the formula

	||a x b || = ||a|| ||b|| sin$\theta$ n
where `θ` is the angle between`a` and `b` in the plane containing them (hence, it is between `0°` and `180°`), `‖a‖` and `‖b‖` are the magnitudes of vectors `a` and `b`, and n is a unit vector perpendicular to the plane containing `a` and `b` in the direction given by the right-hand rule. 

The magnitude of cross product between two vectors would give us the whole area of a parallelogram. 
As we had seen previously the sum would yield the diagonal. 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Cross_product_parallelogram.svg/480px-Cross_product_parallelogram.svg.png "https://en.wikipedia.org/wiki/File:Cross_product_parallelogram.svg")


Now let us be clear about the characteristics of cross product and dot product: 

The cross product can be thought as the measure of perpendicularity and the dot product is the measure of parallelism. Given two unit vectors, their cross product has a magnitude of `1`, if they are perpendicular and magnitude of `0` if they are parallel. 
The dot product of two unit vectors behave exactly opposite, it is `0`, when they are perpendicular and `1` if they are parallel. 
The dot product of two unit vectors yields the `cosine` (which can be both positive and negative) of the angle between two unit vectors. The magnitude of the cross product of the two unit vectors yields the sine, which is always positive. 
The self cross product of a vector is `0` vector. `a x a = 0`
the cross product is defined to be the vector which is perpendicular to both vectors



For next article, we will discuss about Vector Space, Subspace, Basis Vectors and the linear dependence and independence. 

At last leaving you with this Image: 

![Image](https://www.grc.nasa.gov/www/BGH/Images/vectpart.gif)


#### References: 

3. <https://en.wikipedia.org/wiki/Plane_(geometry)>

4. Strang Gilbert Introduction to Linear Algebra p. 6

5. <http://mathworld.wolfram.com/Plane.html> 

6. <https://www.mathstopia.net/vectors/parallelogram-law-vector-addition> 

7. <https://www.sparknotes.com/physics/vectors/vectoraddition/section2/> 

8. Strang Gilbert Introduction to Linear Algebra p. 14

9. <http://mathworld.wolfram.com/TriangleInequality.html> 

10. <https://commons.wikimedia.org/wiki/File:Vector_triangle_inequality_vw.PNG> 

11. <https://www.aaai.org/Papers/ICML/2003/ICML03-022.pdf> 

12. <https://stats.stackexchange.com/questions/132736/k-means-triangle-inequality>


