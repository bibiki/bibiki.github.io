---
layout: post
title: Learning to not Program
---

There is this puzzle that you may have come across online that asks you to count the number of ways to move from the upper left corner of a grid to another point on the same grid. The constraint is that one may not make a move left or up. One may only move down or right. To illustrate, I am showing here the image that I took from the puzzle.

![Paths]({{site.url}}/assets/images/paths-puzzle.png)

So, there are six ways to move from the upper left corner of a grid to a point that is two steps removed to the right and two steps removed down. Let us resort to using coordinates. The upper left corner becomes [0,0] and the goal in the case that the image illustrates is [2,2]. Now, what is the number of ways to move from [0,0] to, for example, point [18,23]?

Let us write a solution in code. We will have a method that takes in as argument the coordinates of the goal and returns the number of ways to reach it according to the above specified constraints.

```
int countPaths(int[] goal) {
    return -1;
}
```

For an algorithm to count all the paths we may imagine we are moving from coordinates [0,0] toward goal [gx, gy], following all valid paths to the goal, and counting them. Generally, we move from coordinates [x,y] to coordinates [x+1, y] and to coordinates [x,y+1]. If, however, x+1 is greater than gx, we do not move to [x+1,y]. Similarly, if y+1 is greater than gy, we do not move to [x,y+1]. Also, we **do not** move from coordinates [gx,gy]. Once we are on [gx,gy], we stop seeking paths.

By looking at the illustration image, we can see that regardless of the particular path taken from [0,0] to the goal [gx,gy], the number of steps to the right is always the same. Similarly, the number of steps downward is always the same. This should convince us that all paths reach the goal at the same total number of steps; at the same time.

Also, at the beginning when we start traversing paths there is a total of one visited point, that is coordinates [0,0], but after taking one step downward and one step rightward, we have had traversed two different paths. After this, by taking steps from the two new points we may end up with up to four new points, and so on. We countinue this process until we have reached the goal, at which point we stop.

With these specifications, here is some pseudocode that implements this algorithm:

```
int countPaths(int[] goal) {
    //keep track of all traversed paths. at the beginning, there is only one path, the starting point

    //while goal has not been reached, take steps downward and rightward if possible and go back to checking if goal has been reached

    //if, however, goal has been reached, return the count of all paths traversed
}
```

Here is the actual code:

```
int countPaths(int[] goal) {
    //keep track of all traversed paths. at the beginning, there is only one path, the starting point
    List<int[]> traversedPaths = new ArrayList();
    traversedPaths.add(new int[]{0,0});
    //while goal has not been reached, take steps downward and rightward if possible and go back to checking if goal has been reached
    while(!contains(traversedPaths, goal)) {
        List nextPaths = new ArrayList();
        for(int[] current : traversedPaths) {
            if (current[0] < goal[0] && current[1] < goal[1]) {
                nextPaths.add(new int[]{current[0]+1, current[1]});
                nextPaths.add(new int[]{current[0], current[1]+1});
                continue;
            }
            if(current[0] < goal[0]) {
                nextPaths.add(new int[]{current[0]+1, current[1]});
            }
            if(current[1] < goal[1]) {
                nextPaths.add(new int[]{current[0], current[1]+1});
            }
        }
        traversedPaths = nextPaths;
    }
    //if, however, goal has been reached, return the count of all paths traversed
    return traversedPaths.size();
}

boolean contains(List<int[]> paths, int[] path) {
    for(int[] p : paths) {
        if(p[0] == path[0] && p[1] == path[1]) return true;
    }
    return false;
}
```

This code does count the number of ways to reach the goal on a grid from the upper left corner of the grid. However, let us **look at the problem another way.**

To reach goal at coordinates [18,23], one must get to it either from its left or from above it. That is, to reach goal at [18,23], one must get to it from either coordinates [17,23] or the coordinates [18,22]. No other way given our constraints of only moving rightward or downward. This indicates that the sum of the number of all paths to the coordinates [17,23] and the number of all paths to the coordinates [18,22] is equal to the total number of paths to [18,23].

To reiterate, ```countPaths([17,23]) + countPaths([18,22]) = countPaths([18,23])```. This suggests a recursive algorithm for our solution. To complete it, we just need to notice that there is a single path possible from [0,0] to any point [px,py] if it is on the first row or the first column of the grid.

Here is the recursive solution in code:

```
int countPaths(int[] goal) {
    //if goal is on top row, or left most column
    if(goal[0] == 0 || goal[1] == 0) return 1;
    //otherwise, add number of paths to the left of goal and number of paths to the right of goal
    return countPaths(new int{goal[0] - 1, goal[1]}) + countPaths(new int{goal[0], goal[1] - 1});
}
```

What an improvement in code aesthetics a slight shift in perspective brought about?! But this is showing how to program. Let us see how not to program. Let us again **change the way we look at the problem.**

Here is again the image from the original puzzle.

![Paths]({{site.url}}/assets/images/paths-puzzle.png)

Every possible path from [0,0] to [2,2] must take exactly two steps downward. Each of these steps will have to be taken in one of three columns of the grid between [0,0] and [2,2]. In other words, we have an equation ```a+b+c=2``` and we need to count the number of non-negative, whole-number solutions to it. Here are solutions for the particular case illustrated in the image.

```
0 + 0 + 2 = 2
0 + 1 + 1 = 2
0 + 2 + 0 = 2
1 + 0 + 1 = 2
1 + 1 + 0 = 2
2 + 0 + 0 = 2
```

This, however, is not feasable for goals that a little further from [0,0] than [2,2] is. The number of solutions to such an equation grows quickly. But, luckily, there is an easy way to count the solutions without producing them. Here is how. We need to count solutions to the equation ```a+b+c=2``` for a,b,c in {0, 1, 2}. We can assume we have 2 sticks and need to split them in three different groups. To do that, we start by arranging the sticks \|\|. We need two markers to mark three groups. For example, M\|M\| corresponding to ```0+1+1```. To the left of the first M, there is an empty group. Another way to put the Ms among the two sticks would be \|\|MM, corresponding to ```2+0+0```. There are two sticks to the left of the first M, and an empty group between the two Ms and an empty group to the right of the last M. So, with two sticks to be split in three groups, we have two Ms and two sticks, regardless of what order they are in. So, we have four items, two of which must be Ms. Hence, what we are looking at is the problem of counting the ways to pick two from four available items. That is determined with n!/(k!(n-k)!); take k from n available. At this point, we just need to determine what are our n and k for a goal defined as [gx,gy]. We can chose to count the number of solutions to an equation of gx+1 variables whose sum is gy. Or, the other way around; count the number of solutions to an equation of gy+1 variables whose sum is gx.

Let us chose to count the number of solutions to an equation of gx+1 variables whose sum is gy. To use the same words as in the previous paragraph, we have gy sticks and need to split them in gx+1 groups. Hence, we have gy sticks and gx Ms. In total, we have gx+gy items and we need to pick gx of them to be Ms. That indicates that our n is gx+gy and our k is gx.

In code, this newer solution now looks like this:

```
int countPaths(int[] goal) {
    return factorial(goal[0]+goal[1])/(factorial(goal[0])*factorial(goal[1]));
}

int factorial(int n) {
    int res = 1;
    for(int i = 2; i <= n; i++) {
        res *= i;
    }
    return res;
}
```

So, here we are with three different ways to solve the same problem. The second is an improvement on the first way. The recursive method makes for better looking code. However, it is pretty slow and consumes large amounts of memory as the goals move further from [0,0]. The third solution is an improvement as it requires way less resources. It is both faster and consumes less memory. It is funny how the same problem has different appearances and hence different solutions. I suppose this should drive home the idea that the actual problem and its discription are two different things. Therefore, when trying to solve a problem it might be helpful to look at it independently of the particular description it is initially introduced in.

Pascal's Triangle
=================

Here is a small part of Pascal's Triangle.

```
0        1
1       1 1
2      1 2 1
3     1 3 3 1
4    1 4 6 4 1
```

This triangle is built with the rule that the first and last elements on each layer are 1, and every element is the sum of the two elements above it. Take the element 6 in the sample triangle above, for example. It is the sum of the two elements above it, 3 and 3. This is also an accurate description of the recursive method written above. Also, every layer of the triangle corresponds to the binomial expansion for some exponent. Here is what binomial expansion is:

```
(a + b)^0 =                      1
(a + b)^1 =                1*a^1 + 1*b^1
(a + b)^2 =           1*a^2 + 2*a^1b^1 + 1*b^2
(a + b)^3 =      1*a^3 + 3*a^2b^1 + 3*a^1b^2 + 1*b^3
(a + b)^4 = 1*a^4 + 4*a^3b^1 + 6*a^2b^2 + 4*a^1b^3 + 1*b^4
```

So, it just expands (a+b)^n. Layer indexed n in Pascal's Triangle has n+1 elements. And, element indexed k on layer indexed n is equal to the number of ways to pick k elements from among n elements, which is the crux of the third way we used to solve the problem at hand.

These last two observations indicate that the original problem is Pascal's Triangle in disguise. Let us show that that really is so.

Here is a grid along with the number of paths to different coordinates on it.

![Paths]({{site.url}}/assets/images/grid.png)


Here is the same grid rotated 90 degrees clockwise.

![Paths]({{site.url}}/assets/images/pascalstriangle.png)

Looking at the first five layers in this rotated square, we see the same part of Pascal's Triangle shown in the beginning of this section. I suppose this shows that it pays to solve by hand small versions of a problem before trying to devise an algorithm for it. For example, trying to solve this particular problem by hand on a piece of paper for goals close to [0,0] would probably remind one of Pascal's Triangle and lead one to the simplest solution that I listed here last.
