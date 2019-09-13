{% include head.html %}
## Introduction To Vectors

Before we move to the concepts of vector, let us start from the beautiful concept as of scalar.
It is anything that is presented in terms of numbers. Mostly representing something physical as your weight, age or even the number of blood cells in your body.


However if we go into the more theoretical definition of the scalar, it is a physical quantity that can be described by
a single element of a number field such as real numbers often accompanied by the unit of measurements.
A scalar has a magnitude, like if I asked you about your age, and you happened to reply me, 22. It is scalar, it describes the length of time you’ve lived. [1]


A vector can be considered as the scalars representing different properties coming together to describe something, it
could be something real or abstract. Vector has both magnitude and direction. A directed line segment in 2D space with
length and direction but no location are vectors. Ordered pairs of real numbers are vectors.
You can add them like this:

`(1, 2)+(3, 4) = (4, 6) (1, 2)+(3, 4)= (4, 6)`

You can scale them like this:

`2(1, 2)=(2, 4)`

Let’s say a bus is travelling at the speed of 3m/second, here this quantity of the speed is a scalar.
Let’s add another property, the destination towards which the bus is heading.
Let’s just assume that our arbitrary bus is heading towards north. Here we have both magnitude and direction giving us the velocity vector of the bus.

As another example, let us consider the movement of the bus that is easy to visualize.
Let’s assume that our bus was moving on cartesian coordinate system (X, Y plane).
At first it started its engine at the origin `(0, 0)`, then it travelled horizontal `2km` and vertical `4km`.
Here this can be visualized as a vector in the cartesian plane.

![A line segment as vector]({{'static/images/LRintro/geogebra-export.png' | absolute_url}})

The magnitude of a vector often denoted by the length of the line, is something that is represented by the distance between starting and the ending points.
In our example, the magnitude of the displacement vector given by the travel of a bus is $$\sqrt{20}$$,
given by the formula:

$$\sqrt{(v1^2 + v2^2)}$$
                         `v1` and `v2` being two elements of a 2-d vector.

$$\sqrt{(2^2 + 4^2)}$$ .

And the direction of the line denoting where the bus is headed in the plane, visually it's moving some direction horizontally and some direction vertically to get to that point.
Often Zero vector or let’s just say the origin of cartesian plane `(0, 0)` is taken as the initiating point of vector in $$R^2$$ (2d space) for
the sake of simplicity. But a vector can initiate from any point. Let’s draw the same vector, however taking `(2, -2)` as the origin of the vector.

![Same vector from different origin]({{'static/images/LRintro/Vector-Introduction-1.png' | absolute_url}})

If you see in the above image, the vector `a` and `a’` are the same vector having a different origins. They still have the same magnitude and the direction,
hence they are the equivalent of each other.

**Note**: That’s what we meant by ‘no location’ when we mentioned no physical location in the vector’s definition. But this is an abstract concept.

## Addition & Subtraction:
Considering the above example let us say, that our bus further ascended from the point `(2, 4)` and travelled `3 km` horizontal and descended `3 km` vertical,
this can be represented as a vector `(3, -3)` . `(-3)` here because the bus is descending on the vertical side of the plane,
as it’s not going up rather inclined towards the horizontal direction. Since the bus travelled further from the point `(2, 4)` in our plane,
let’s visualize how it looks like:
![Vector addition]({{'static/images/LRintro/Vector-Introduction-1 (1).png' | absolute_url}})

Let’s see where we would have ended up, if we had done some arithmetic instead of plotting the new travel vector in the plane.

`(2, 4 ) + (3, -3) = (5, 1)`

And see, that’s where we have ended up in the graph as well. So the intuition here is that by adding the vector representing the same properties of
different scale, we can reach to the different point in the plane and for that we necessarily don’t have to do the actual addition at
all (which is ofcourse more easy). We can actually assume the first vector as an origin and then plot the coordinates
accordingly in the plane to reach the point represented by the addition of two vectors.

**Note**: By the analogy **plane**, we’re just referring to the 2-d space. Nothing fancy. If you’re too curious, how would it look like, then cartesian plane is what is it.

Similar thing happens with the subtraction as well. It’s more intuitive to show these addition and subtraction in cartesian plane dealing with 2
dimensional vector, however same thing is very hard and sometimes even impossible in the larger dimension.

Let us see one example of the subtraction. Let's say at the point `(2, 4)`, the bus driver realized that he had to add the
fuel in the bus to run for the whole day, however he had already missed the nearest fuel station. Now he had to return the
bus to reach the nearest fuel station for that he descends `2 km` horizontally and `2 km` vertically, forming a vector `(-2, -2)` from the point `(2, 4)`.
But the same vector if we were to take in terms of the origin `(0, 0)`, it would be `(2, 2)`, then we would have to subtract `(2, 2)`
from `(2, 4)` to get to our arbitrary fuel station. Since our bus is already at a point `(2, 4)`, it has to descend reducing its original magnitude.


Now if we do some arithmetic, our new point would look like this:
`(2, 4) - (2, 2)  = (0, 2) `

![Vector addition]({{'static/images/LRintro/Vector-Introduction-1 (2).png' | absolute_url}})

The point `A (0, 2)` is where the fuel station is, the vector `a’`  origin is the `(0, 0)`. Since our bus is at `(4, 2)`,
to descend from the point to the fuel station. We can understand it in the following way:

1. Descend `2` point horizontally, `2` vertically, that would be like `(-2, -2)` from the origin `(2, 4)`
2. In terms of the origin `(0, 0)`, it would be `(2, 2)`, then we would have to subtract `(2, 2)` from `(2, 4)`
to get to our arbitrary fuel station, we would end up then at the point `(0, 2)` .


Let’s plot that in the figure:
![Vector addition]({{'static/images/LRintro/Vector-Introduction-1 (3).png' | absolute_url}})

Which gave us the same point `A (0, 2)` .

Vector elements can operate in multiple measurement systems. Say we want to form a vector to
define property of a car. Say one can put together the weight of the car and number of
people it is carrying, the age of the car and it’s speed forming a vector which looks something like this:

`(10ton, 100 people, 2 years, 5mph/s)`

This is the four dimensional vector representing the various properly of the car.

## Parallelogram law of vector addition:

There is an interesting phenomenon in vector addition,
let us have the two vectors formed by our travelling bus, one is `(2, 4)` and another is `(5, 2)`.
Let’s add them together, this should give us the net displacement of the bus in both horizontal and the vertical direction.

`(2, 4) + (5, 2) = (7, 6)`

Now let’s visualize that:
![Parallelogram law of Vector addition]({{'static/images/LRintro/Vector-Introduction-1 (5).png' | absolute_url}})

Here the vector r is the resultant of the addition between vectors a and b.
If we connect the points `A, B & C`, we will get a nice parallelogram.

![Parallelogram law of Vector addition]({{'static/images/LRintro/Vector-Introduction-Parrallogram-law.png' | absolute_url}})

This is what is known as the parallelogram law of vector addition.
If two vectors acting simultaneously at a point can be represented both in magnitude and
direction by the adjacent sides of a parallelogram drawn from a point, then the resultant vector
is represented both in magnitude and direction by the diagonal of the parallelogram passing
through that point. [2] If you want to see, how that works, you may refer to the same referenced link.


## Scalar Multiplication :

When we want to scale the vector up or down, we perform the scalar multiplication, just
like multiplying anything with `1` would yield us the same result, even in the case of scaling a
vector with 1 doesn’t affect its magnitude. Remember, when we scale a single vector,
it’s direction remains the same, just the magnitude changes.
Let’s scale our original vector `(2, 4)` by some constant `3` and visualize the result.

`3 (2, 4) = (6, 12)`

![Parallelogram law of Vector addition]({{'static/images/LRintro/Scalar Multipication .png' | absolute_url}})

As you can see in the figure, the direction of the vector u didn’t change, however the magnitude just go scaled up.
### References:
1. https://en.wikipedia.org/wiki/Scalar_(physics)
2. https://www.mathstopia.net/vectors/parallelogram-law-vector-addition
3. [Vector images drawn using Geogebra](https://www.geogebra.org/?lang=en)




