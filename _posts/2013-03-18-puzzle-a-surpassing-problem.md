---
layout: post
title: "Puzzle - A surpassing problem"
description: ""
category: 
tags: [ruby, algorithms]
permalink: blog/2013/03/18/puzzle-a-surpassing-problem
---
{% include JB/setup %}
Another week, another interesting puzzle. This puzzle is taken from a programming exercise posed by *Martin Rem* in 1988

> *A Surpassing Problem*

> By definition, a surpasser of an element of an array is a greater element to the right, so x[j] is a 
> surpasser of x[i] if i < j and x[i] < x[j]. The surpasser count of an element is the number of 
> surpassers. For example, the surpasser count of the string GENERATING are given by 

> ```G E N E R A T I N G```
>
>  ```5 6 2 5 1 4 0 I 0 0```

> The maximum surpasser count is 6. The first occurrence of the letter E has six surpassers, namely 
> N, R, T, I, N, and G. Rem's problem is to compute the maximum surpasser count of an array of 
> length n > 1 and to do so with an O(n log n) algorithm.

<a onclick="$('#solution_surpassing_problem').slideDown();$(this).slideUp();return false;" href="#" >Click to view my solution</a>


<div id="solution_surpassing_problem" style="display:none">
<div class="text"><p>Not the most succint solution, but it does match the <i>O(n logn) </i>criteria. Infact I am using the same<b>&nbsp;RangeCountTree</b> used in the <a href="http://shadabahmed.com/blog/2013/03/09/rangecounttree-a-binary-search-tree-for-ranges"><b><i>last puzzle</i></b></a>.</p><p>Here's what I am doing (a<span style="font-family: Arial, Helvetica, Verdana, Tahoma, sans-serif; font-size: 15px; line-height: 1.45em;">ssuming word only contains capital letters):</span></p><p>1. Start iterating from the reverse of the string. For the current example, you first see a <i style="font-weight: bold;">G&nbsp;</i>so insert a range <b><i>A,G</i></b> in the tree. The range <b style="font-style: italic;">A,G</b> in the tree&nbsp;would now have count <b>1.</b> This just means that surpassers count for letters A to F is <b style="font-style: italic;">1.&nbsp;&nbsp;</b>Please note that&nbsp;<i style="font-style: italic; font-weight: bold;"><b>A,G</b></i><b style="font-style: italic;">&nbsp;</b>means letters&nbsp;<i style="font-style: italic; font-weight: bold;"><b>A to F(G is excluded)</b>.&nbsp;</i></p><p>2. Now let's build this tree for <b><i>NERATING</i></b>. See it below:</p><p><img src="http://i.imgur.com/UxFQsvy.png" style=""><br></p><p>The representation is such, that for any new character, the corresponding range in the tree would give the count of surpassers.</p><p>3. Calculate the <i><b>max_surpassers</b></i> while building the tree itself. So now when you see <b><i>E</i></b> next after <b><i>NERATING</i></b>, the matching range for <b><i>E</i></b> in the above tree is&nbsp;<i style="font-weight: bold;">E,G -&gt; 6</i>. Therefore the current <b style="font-style: italic;">max_surpassers </b>is now <b style="font-style: italic;">6</b></p><p>Let's see how the tree looks for the whole string <b><i>GENERATING</i></b>:</p><p><img src="http://i.imgur.com/oePeptt.png" style=""><br></p><p><br></p><p>Now if the string was changed to <b><i>REGENERATING</i></b>, you would see that the current tree has <b><i>E,G -&gt; 7</i></b> , so the max surpassers calculated would be 7</p><p>We could do the same thing for finding max surpassers in a list of numbers, only difference being that we would insert each range as (<i>-Infinity, current_number</i>)</p><p>Code here:</p><p></p><pre class="ruby"><span class="keyword">require</span> <span class="string">'./range_count_tree'</span>

data = <span class="string">"REGENERATING"</span>

rtree = <span class="constant">RangeCountTree</span>.new
max_surpassers = <span class="number">0</span>

data.reverse.each_char <span class="keyword">do</span> |char|
  <span class="comment"># Set range_start to A for the min possible range</span>
  range_start = <span class="number">?A</span>.ord
  range_end = char.ord

  <span class="comment"># Find the surpassers for the current character in the current tree</span>
  current_max_node = rtree.find(range_end)

  <span class="comment"># set max surpassers if current surpassers is larger</span>
  max_surpassers = current_max_node.count <span class="keyword">if</span> current_max_node &amp;&amp; current_max_node.count &gt; max_surpassers

  <span class="comment"># Do not add this range to the tree, since we will not encounter any characters in this range</span>
  <span class="comment"># This is just an optimization and not necessary</span>
  <span class="keyword">next</span> <span class="keyword">if</span> range_start == range_end

  <span class="comment"># Update the current tree for the next stream of characters</span>
  rtree.add([range_start, range_end])
<span class="keyword">end</span>

puts <span class="string">"Max surpassers is <span class="subst">#{max_surpassers}</span>"</span></pre><p></p><p></p><p>
I think there is a much better solution which would put my verbose approach to shame with its simplicity
</p>
</div>
</div>
