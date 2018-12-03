## Introduction

This is the first article of the Linear Algebra series, and in this article we will be discussing about the following fundamental topics of Linear Algebra: 

- Vector & Linear Combination 
- A Line, Plane & Space
- Dot Product, Vector Norm, Unit Vector
- Inequalities 
- Cross Product 


Before we move to the concepts of vector, let us start from the beautiful concept as of scalar. It is anything that is presented in terms of numbers. Mostly representing something physical as your weight, age or even the number of blood cells in your body. 
However if we go into the more theoretical definition of the scalar, it is a physical quantity that can be described by a single element of a number field such as real numbers often accompanied by the unit of measurements. A scalar has a magnitude, like if I asked you about your age, and you happened to reply, 22. It is scalar, it describes the length of time you’ve lived. `[1] `

Let us now, take a step further and add some more characteristics to the scalar, Let us assume that you’re a salesperson in the bakery shop beside the road where I usually eat in the evening. If I asked you about the price of a carrot cake, you would say Rs. 40, here 40 is scalar, now you've added another attribute to the carrot cake and said that for the 50g carrot cake, it would cost Rs 50, but for 25g carrot cake it would cost Rs 25. Then amazingly you just gave an answer in terms of a vector. Here the price is relative to the weight of carrot cake, and forms two vectors `(50g, Rs 40), (25g, Rs 25)` . Now this has both magnitude and direction. Weird words they are, which we will discuss later.  Let us say, we added another further characteristics in the carrot cake, say packaging, blue and green. We can keep on adding more attributes as much as we want,
Our Vectors `V1` would be (`50g, Rs 40, Blue)`and `V2` would be (`25g`, `Rs 25`, `Green` 
And`V3 = (50g, Rs 55, Green) `

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
i) 2 pieces of carrot cake with the price Rs. `50`. 
ii) 2 cups of black coffee with the price Rs `65` per cup.
iii) 2 fruit salad with Rs `125` per plate. 

After the bakery, we felt the growing winter and realized that we had to buy a few clothes as well. We would go to the retailers and buy a few things: 

i) A pair of shocks with the price Rs `300`. 
ii) A woolen sweater with price Rs `1200`. 
iii) A muffler with price Rs `225`

So, let's do something interesting, from our purchases let's form two vectors.

`V1 = (50, 65, 125)`  :vector of price we have to pay for each unit in the bakery shop.

`V2 = (300, 1200, 225)` :vector of price we have to pay for each unit in the winter shop. 

Here we can form a trivial linear combination of the purchases that we made: 

Now we have the quantity which are scalar, lets calculate the total money we expensed in the bakery shop. 

`2 V1 + 1 V2 `

`2 (50, 65, 125) + 1 (300, 1200, 225)  = Rs 2205`

Giving us a scalar `2205`. Combining addition with scalar multiplication, we now form "linear combinations" of vectors `V1` and `V2`. We multiply `V1` by `c` and multiply `V2` by `d` then add `cV1` + `dV2`.

This is the linear combination between the vectors that we created by entering into the  bakery shop and retailer. It is a trivial example and there could be a more scientific one to explain further. For now, let us go further towards a more theoretical definition of the linear combination. 

# Linear Combination - A formal definition: 

Let `V` be a vector space over the `FIELD K`. For any vectors `v1, v2, v3, ………, vn ∈ V `& `x1, x2, x3, ……………. , xn ∈ K,` the expression of the form `x1v1 + x2v2 + x3v3 + …………….. +` `xnvn` is called the linear combination of the vector `v1, v2, v3, …. Vn.`

Since, we haven’t yet said anything about the vector field and vector space, let’s just understand that, we scale each vector in a particular vector space with some scalar, and summing all vectors in the vector space. We will come to the significance of these linear combination as we go along the further topics in vectors eg: vector space, basis vector, linear dependence and independence. 

# A Line, Plane & Space: 

A vector is something that has both magnitude and direction, that is as plain as the point representing two coordinate axes in the Euclidean space. Say, the co-ordinate, `(2,3)` can be a vector by drawing the line from the origin. If you graph it, you will get a vector with the magnitude of `sqrt(15)` and some direction theta. 



The linear combination of a single vector as `(2,3)` which would be `cv` would form a line, similarly the linear combination of two vectors` v = (2, 3)` and` w = (4, 2) `
`cv + dw`  would form a plane. 
![Figure Reference 4]({{'/static/images/LRintro/image1.jpg.jpg'}})
