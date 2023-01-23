---
layout: post
title:  "Just what is a group? Can I see it somewhere?"
---

<span style="color:red">P.S. I started this post with the intention to simplify the description of what groups in math are. Evidently, there is some tension between simple description and long description. Very difficult to have a simple description that is not long.</span>

Yes, you can see it everywhere.

![Circle]({{site.url}}/assets/images/circlesmall.png)

Here is a circle. Let us point out the obvious. There are six equal arcs inside it, and each of them is colored a different color.

Here are all the ways this circle may show up.

![Circle]({{site.url}}/assets/images/circlesmall.png)
![Circle]({{site.url}}/assets/images/circlesmall60.png)
![Circle]({{site.url}}/assets/images/circlesmall120.png) \
![Circle]({{site.url}}/assets/images/circlesmall180.png)
![Circle]({{site.url}}/assets/images/circlesmall240.png)
![Circle]({{site.url}}/assets/images/circlesmall300.png)

So, all these six circles are the same one as you can start with any one and simply rotating it generate any of the six circles. In addition, there is no other way this circle may show up. We can definitely have a circle that has its colors arranged differently and that would make it a different circle. But these six I showed in a row are all the same circle appearing in different situations.

I mentioned earlier that we can rotate a circle. Most people will know immediately what that means. Some additional clarification that may be necessary here is whether the rotation is done clockwise or counter clockwise. Also, the smallest rotation between any of these six situations of the circle is 60 degrees (or pi/3, if you will).

However, there is an opportunity here to improve the language we use to discuss this topic. First, we will arbitrarily chose that rotation is **always performed counter clockwise**. Second, lets name the rotation by 60 degrees a 'step.' So, we can start with the circle having its yellow arc on top and rotate it one step to get the blue arc on top. That is how we got the second situation of the circle starting with the first in the list of six situations of the circle above.

With the current language, one can say "start with circle having its yellow arc on top and rotate it three steps". In an effort to develop some notation, we may end up with something like rotate(yellow, 3). Or, to reuse notation from before, yellow + 3. But if we use this notation, then rotation of our circle becomes a matter of taking some state of the circle as defined by what arc is on top and a number, and then getting the circle in a different situation defined by a new arc sitting on top. That is all fine, but there is a better way.

We can have a language that does not use anything other than the colors of the arcs. Things like yellow + blue = blue, or purple + red = blue make sense in that language. Also, this language now lets us define rotation completely in terms of the colors of the arcs, and we can teach this language to a hypothetical robot that does not know what 3 means.

To help the robot understand the language, we will need to:

1. Arbitrarily pick one state of the circle as a kind of point of reference. In the example here that is the circle with yellow arc on top.
2. Teach the robot to read requests like purple + red.

Let us teach the robot: purple + red is really a way to say "start with our point of reference and rotate the circle to get purple on top. Then, using the new situation of the circle perform the same rotation you would need to perform to get our initial point of reference in a situation where red is on top".
To our human mind "start with our point of reference and rotate the circle to get purple on top" is like saying "rotate the initial situation of the circle (the one that has yellow on top) and rotate it 3 steps."
And, the sentence "*then, using the new situation of the circle perform the same you would need to perform to get our initial point of reference in a situation where red is on top.*" to our experience means "count how many steps you need to rotate the circle with yellow arc on top to get red arc on top, and rotate our circle with purple on top that many steps." To make sure that you understand, you should see why purple + red = blue.

With the aim to clarify, I will repeate: to calculate purple + red:

1. Take initial circle, with yellow arc on top, rotate it until purple is on top. By looking at our circle, we see that this is a rotation of three steps. We do that and get purple on top.
2. Take initial circle, with yellow arc on top, rotate it until red is on top. From this, we learn that this takes a rotation of 4 steps.
3. Therefore, to add red to purple, we apply a rotation of four steps to our circle with purple arc on top, and that puts blue arc on top.

Now, the next linguistic improvement to describe this whole topic is to avoid having to use sentences like “**then, using the new situation of the circle perform the same you would need to perform to get our initial point of reference in a situation where red is on top.**”

For that improvement, we can count the steps we need to rotate the initial circle to get a given arc on top, and then use that number to lable the arc. Again, instead of labeling the arcs using colors, we can use the number of steps we need to rotate the initial circle to get arc of color X on top and label that arc with that number instead of color X. For the arc of color yellow that is 0 as you do not need to rotate the initial circle to get yellow arc on top. Similarly, blue arc will be labeled 1, brown arc will be labeled 2, and so on.

So, the circle may look like this:

![Circle of numbers]({{site.url}}/assets/images/circle_numbers.png)

And now, we can resort to what we know about usual addition of numbers to calculate 'addition' of colors in our circle. But, I do not know how that would be of any interest. What is of interest, though, is that addition modulo some number is really no different than performing rotations on a circle.

Anyways, so far we managed to combine different situations of the circle to get another situation of the circle that is amongst the initial list of six situations of the circle.

Also, by chosing an initial point of reference, or initial state of the circle, we managed to map one of the situations of the circle onto 'do no rotation' or 'no rotation steps.'

Next, we'd like to associate a given situation of the circle and another situation of the circle whose combination yields the initial point of reference. This is the inverse, but the inverse of what? The inverse of the operation, or the inverse of each situation of the circle in relation to the original operation?

The inverse
===========

I already mentioned that describing a situation of the circle by what color is the arc on top is just another way of indicating how many steps the initial circle, the one with yellow arc on top, needs to be rotated to get the arc of the desired color on top. There is a related idea here that describes the inverse. For example, with red arc on top, we would like to know what rotation we can perform to get yellow arc on top.
In notation we are familiar with, we may write red + X = yellow.
Or in our circle that uses numbers to label its arcs, we can have something like 4 + X = 0. From earlier math, we may say 4 - 4 = 0, but this is innacurate and here is why.

We use + to indicate rotation of our circle counter clockwise. And in both sides of + we have two situations of our circle, well defined by what color the arc on top is.

Let me point out that 4 + (-4) = 0 and 4 - 4 = 0 have the same meaning in our experience with adding numbers as we are used to.

But in our rotation of circles, we can read 4 + (-4) as "start with circle that has 4 on top and apply to it the change you need to apply to a circle with 0 on top to get -4 on top." But no arc in our circle is labeled -4?!?!

Or, we can read 4 - 4 as start with a circle that has 4 on top. But what does - mean? We only recognize + for rotation counter clockwise. We may chose for - to mean rotation clockwise but now we have two operations that in a sense are inverses of each other, but we want to know what situation of the circle is the inverse of a given situation of a circle only using a particular operation, namely rotation counter clockwise.

So, briefly, we want to avoid any operation but the one we started off with. And, we want red + X = yellow to have a solution in the collection of situations of the circle that we started off with.

So, we need to find a way to find the inverse using rotation counter clockwise and only the labels used in our circle from the beginning.

Let us look at the circle with red arc on top. What we see indicates unambiguously that we need to rotate it two steps to get yellow arc on top. Now we need to answer this question 'starting with yellow arc on top, if we rotate it 2 steps, what arc do we get on top?' Again, by looking at the circle with yellow arc on top, we see that we get the circle with brown arc on top if we rotate it 2 steps.

Hence red + X = yellow for X = brown, as red + brown = yellow.

Similarly, 4 + 2 = 0, in our circle with numbers for labels.

Let us for the sake of illustration solve 4 + X = 0 in the same manner as above.

Let's take the circle with arc 4 on top. By looking at it, we see that we need to rotate it counter clockwise 2 steps to get 0 on top. Now, we need to determine what change we need to apply to circle with arc 0 on top to get arc 2 on top. Because we chose to use for labels of arcs their distance from the arc labeled 0, the answer to this later question is 2. Hence, 4 + X = 0 has solution X = 2.

What happened so far
====================

I started with a circle with six arcs, each colored in a different color. I then picked rotation counter clockwise as an operation to apply to those circles. The operation was favorable to being described as a combination of two situations of the circle, and produced a situation of a circle. (**the set is closed in relation to the operation defined in it.**)

Next, there turned out to be a situation of the circle, namely the one described as yellow, such that yellow combined with any other situation X, always returned X, whether we ordered our rotations as yellow + X or X + yellow. That it was the circle with yellow arc on top is a result of our arbitrary choice to use that same cricle as our initial point of reference. That there is some such situation for our combination of choice, namely rotation counter clockwise, is an intrisic property of the combination. (**there is an identity element X such that X + Y = Y + X = the_identity_element.** Usually denoted by 0. When I used colors, I denoted it by yellow.)

Also, any X situation of the circle has a corresponding X\` stuation such that X + X\` = yellow. This is what I described under *The Inverse*. All that means is that one can start with a circle with arc colored X on top, perform the operation we have arbitrarily chosen in the beginning (rotation counter clockwise), and get yellow arc on top, regardless of what the initial X color is. (**for any X in the set, there is some Y in the set such that X + Y = Y + X = 0**)

And finally, I did not write anything on the fact that yellow + (purple + red) = (yellow + purple) + red, or more generally X + (Y + Z) = (X + Y) + Z holds true for any situations of the circle that X, Y, and Z may stand for, but this does hold true for the circles above, and the operation of rotation counter clockwise. (**the associativity of the operation**)

To help my self to see why this property is important I rely on my experience in programming with accumulating lists. And because I want to relate groups to more people's experience, I will refrain from describing how the associative rule relates to the experience of programmers, and ask you to just remember that this is a property that some combinations of elements in some collections have.

Finally, if you have a set of elements and an operation defined on it, and if that operation abides by these four rules, then... for the purpose of facilitating communication, it is said that the set and the operation form a group.

Another group
=============

Let's do something similar with triangles.

<img src="{{site.url}}/assets/images/trianglergb.svg" width="64px"/>
<img src="{{site.url}}/assets/images/trianglebrg.svg" width="64px"/>
<img src="{{site.url}}/assets/images/trianglegbr.svg" width="64px"/> \
<img src="{{site.url}}/assets/images/trianglerbg.svg" width="64px"/>
<img src="{{site.url}}/assets/images/trianglegrb.svg" width="64px"/>
<img src="{{site.url}}/assets/images/trianglebgr.svg" width="64px"/>

So, here is a triangle and all six ways it may occur in. The first three triangles generate each other by a mere rotation similar to the one above for circles. Same for the second row of three triangles. However, triangles from the first row do not generate triangles from the second row by such a rotation. But, the first triangles in each row are symmetries of each other. To help visualization, I am listing them here one next to the other.

<img src="{{site.url}}/assets/images/trianglergb.svg" width="64px"/>
<img src="{{site.url}}/assets/images/trianglerbg.svg" width="64px"/>

Nothing changed, except that the left edge and the right edge switched places.

When I was discussing circles, I wrote something like purple + red. However, since purple really specifies a situation of the original circle with purple arc on top, it also specifies the position of the other arcs in that particular situation of the circle. So, purple + red is really circle_with_purple_on_top + circle_with_red_on_top. Hence, I was not adding arcs. I was rather adding various situations of the original circle. And, I can 'add' the triangles shown above, too. First_triangle + Second_triangle... is equal to Second_triangle, assuming that First_triangle is my 'point of reference'.

To use language similar to what I used earlier for circle, First_triangle + Second_triangle is like saying:

1. Note what you need to do to the point of reference (that is First_triangle) to get First_triangle.
2. Apply that same collection of steps to Second_triangle.

Since the answer to 1 is 'nothing,' then we do nothing to Second_triangle to get it in return.

Let us see what Fourth_triangle + Second_triangle equals to.

1. To get Fourth_triangle from First_triangle, First_triangle's left edge and right edge need to switch places.
2. So, let us do that to the Second_triangle. We get the Sixth_triangle back.

Now, 'adding' circles is somewhat different than adding triangles. Besides, a circle may have more, say 100, arcs. But, there are a few rules that hold true for 'adding' the circles above, 'adding' the triangles above, 'adding' circles with 100 arcs, and adding numbers as we are used to. Matter of factly, addition of matrices of same dimensions abides by the same rules. And those rules are those I listed above under **What happened so far**. In other words, 'addition' of triangles as shown above, 'addition' of circles as shown above, and addition of same-dimensioned matrices are groups.

That is all there is to what a group is. But there is a whole lot to what understanding of groups facilitates. If you are curious, there are only [209 different crystals](http://abstract.ups.edu/) in nature and it was the idea of groups that revealed this number.

And finally, there are books that focus on visualization of these and more ideas. Haven't had a chance to read any just yet, but if you search online for 'visual group theory' or 'visual algebra' you will find those books.
