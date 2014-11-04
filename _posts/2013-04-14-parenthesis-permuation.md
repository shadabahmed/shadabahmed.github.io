---
layout: post
title: "Parenthesis Permutation"
description: ""
category: Algorithms 
tags: [ruby, algorithms]
permalink: blog/2013/04/14/parenthesis-permuation
---
{% include JB/setup %}
Another interesting puzzle:

> ## Parenthesis Permutation
> 
> Given N pair of parenthesis. Write an algorithm which would print out all permutations, possible with 
>  those parenthesis given that parenthesis are in correct order (i.e. every open parenthesis is matched 
> with closed parenthesis) For .e.g. .. N =3 should give: 
>
> ()()()
> 
> (()()) 
> 
> ()(()) 
> 
> (())() 
> 
> ((()))

There are recursive solutions for this you can find just by googling. I thought of a non-recursive solution

<a onclick="$('#solution_paren_perm').slideDown();$(this).slideUp();return false;" href="#" >Click to view my solution</a>

<div id="solution_paren_perm" style="display:none">
<p>The basic idea is to construct a Binary Tree, where left node is <i>'<b>('</b></i> extra and right node is <i>'<b>)'</b></i> extra from the current node. &nbsp;Each node has a <b style="font-style: italic;">weight = number of '(' - number of ')'.&nbsp;</b></p><p>We start with a root node <b>'('</b> at level <b>0&nbsp;</b>and create the tree such that the <b>2N-1</b> level will contain permutations for <b>N</b> pairs.</p><p>Now when constructing the tree, we create a child only if:</p><p></p><span style="font-size: 15px;"><ul><li><span style="font-size: 15px;">Weight of incoming child is not less than 0</span><br></li><li><span style="font-size: 15px;">When weight >= 0, it should be less than or equal to the numbers of levels to create, so that we have enough parenthesis left to balance the string</span><br></li></ul></span><p></p><p>Let's take a look at the tree created (string -&gt; weight) for <b>N = 3</b>:</p><p></p><p><img src="http://i.imgur.com/fdTA0P0.png" style=""></p><p></p><p></p><p></p>An optimization - the last level is not required since all that is added is a '<b><i>)'</i></b><p></p><p>Now the code. Infact not using a tree at all, just a linked list and traversing in preorder style, using the concepts above of adding a child. I could have used an array instead of linked list, but it suffers from list expansion frequently hence slowing everything down.</p><p></p><script src="https://gist.github.com/shadabahmed/f8dd82c83ba6cc9c0287.js"></script>The code to generate the tree picture is <a href="https://gist.github.com/shadabahmed/5381574">here</a>
<p>Infact the number of permutations for a given N pair form a series called <a href="http://en.wikipedia.org/wiki/Catalan_number">Catalan Numbers</a>&nbsp;. It goes like this:</p>
<pre class="no-highlight">N=1 P=1
N=2 P=2
N=3 P=5
N=4 P=14
N=5 P=42
N=6 P=132
N=7 P=429
N=8 P=1430
N=9 P=4862
N=10 P=16796
N=11 P=58786
N=12 P=208012
N=13 P=742900
N=14 P=2674440
N=15 P=9694845</pre>
</div>
