---
layout: post
title: "Skyline Algorithm - A Binary Tree Approach"
description: ""
category: algorithms
tags: [algorithms]
permalink: blog/2013/04/24/skyline-algorithm-a-binary-tree-approach
---
{% include JB/setup %}
If you're into algorithms, you must have heard of this puzzle:

> ## Drawing the Skyline
> A number of buildings are visible form a point. Each building is a rectangle, and the bottom of each 
> buliding lies on a fixed line. A building is specified using a triple of (Left, Height, Right). One building 
> may partly obstruct another, as shown below:
>
> ![Skyline](http://i.imgur.com/nv5RL1s.jpg)
>
> The skyline is the list of coordinates and corresponding heights of what is visible.
>
> For example, the skyline to the buildings on the left in figure above is given in the figure on the right.
>
> Example input: 
> ```(1,11,5), (2,6,7), (3,13,9), (12,7,16), (14,3,25), (19,18,22), (23,13,29), (24,4,28)```
>
> Example output:
> ```1, 11, 3, 13, 9, 0, 12, 7, 16, 3, 19, 18, 22, 3, 23, 13, 29, 0```

This puzzle is particularly popular in academia and also as an interview question. If you [google](http://googl.com/#q=skyline+algorithm) it, you would find many research papers as well.

## Skyline Tree

Now I am presenting a binary tree solution - Skyline Tree. This tree is very much similar to a tree I created earlier - [Range Count Tree](http://shadabahmed.com/blog/2013/03/09/rangecounttree--binary-search-tree-for-ranges). Here is goes:

For the input we are given a set of tuplets containing - where a building starts, where it ends and the height. We use this input to build a binary search tree in such a way, that the nodes in the tree represent the maximum height for a given range of start and end coordinates. 

Each node in the tree has attributes ***range_start***, ***range_end*** and ***value***(height). We just follow these rules when creating the tree:

* Each parent has a greater ***value(height)*** than any of its children

* When a child is being inserted at a node, it is added in left subtree if the range of the child completely lies to the left of the node's range and vice versa for the right

* If the incoming range intersects with the current node, we split the incoming range and then add the split parts as children of the node. For e.g. if we already have 4,5 as an existing range and incoming is 2,8 it becomes:
<pre>
<code>                           4,5 
                              /    \
                             2,4  5,8</code></pre>

This is done because the parent 4,5 has greater ***value(height)*** than range 2,8 so only the tallest portions are added to the tree, for the ranges of these nodes. 

Now, how do we ensure that a ***parent*** is taller than its ***children*** ?
> We simply insert nodes in the Skyline tree sorted by their height.

Time for pretty pictures:

    Input: (1,11,5), (2,6,7), (3,13,9), (12,7,16)

![Skyline Tree Partial](http://i.imgur.com/Nom2PVV.png)

Starting from the left, you can see, range ***1 to 3*** has maximum height ***11***, ***3 to 9*** has ***13*** and ***12 to 16***, ***7***. Now let's see the tree for the full input:

    Input: (1,11,5), (2,6,7), (3,13,9), (12,7,16), (14,3,25), (19,18,22), (23,13,29), (24,4,28)

![Skyline Tree Full](http://i.imgur.com/iHGE7Mn.png)

The code for SkylineTree is [here(gist)](https://gist.github.com/shadabahmed/5440961) and the code to solve the puzzle using the Skyline tree:

    require './skyline_tree'

    input = [[1,11,5], [2,6,7], [3,13,9], [12,7,16], [14,3,25], [19,18,22], [23,13,29], [24,4,28]]
    input.sort!{|x,y| y[1] <=> x[1]}

    stree = SkylineTree.new
    input.each do |start_range, value, end_range|
      stree.add [start_range, end_range], value
    end
    stree.print_skyline
    # output
    # 1, 11, 3, 13, 9, 0, 12, 7, 16, 3, 19, 18, 22, 3, 23, 13, 29, 0

You can also run the code on [CodeBunk](http://codebunk.com/bunk#-IsprTqfSpGJKyfkuYeC)

The tree we built can be used for similar problems like area under the skyline etc.
