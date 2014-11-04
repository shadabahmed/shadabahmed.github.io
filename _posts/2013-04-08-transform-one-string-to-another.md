---
layout: post
title: "Transform One String to Another"
description: ""
category: Algorithms
tags: [ruby, algorithms]
permalink: blog/2013/04/08/transform-one-string-to-another
---
{% include JB/setup %}
Another interesting puzzle :-

> Transform One String to Another
>
> Let S and T be strings and D a set of strings. You can say that S produces T if there exists a 
> sequence of strings, SEQ=[s1, s2, ..., sn-1] which meets these criteria:
>
> * s0 = S
> 
> * sn-1 = T
>
> * All members of SEQ belong to set D
> 
> * Adjacent strings have the same length and differ in exactly one character.
> 
> For example, given the set {"cat", "bar", "bat"}, you can say that "cat" produces "bar" by
> 
> ["cat", "bat", "bar"]
> 
> Or, given the set {"cat", "red", "bat"}, you can say that "cat" does not produce "red".
> 
> Given a set D and two strings S and T, write a function to determine if S produces T. Assume that all > characters are lowercase letters. If S does produce T, output the length of a shortest production 
> sequence; otherwise, output -1.

<a onclick="$('#solution_transform_string').slideDown();$(this).slideUp();return false;" href="#" >Click to view my solution</a>

<div id="solution_transform_string" style="display:none">
<div class="text">Solving this using a graph. Not the most efficient graph, but it does have pretty pictures :)</p><p>First let's take this sample data:<br></p><p></p><pre class="ruby">words = [<span class="string">'simple'</span>, <span class="string">'dimple'</span> , <span class="string">'pimple'</span>,<span class="string">'fickle'</span>, <span class="string">'sickle'</span>, <span class="string">'simkle'</span>, <span class="string">'kettle'</span>, <span class="string">'settle'</span>]
start_word = <span class="string">'simple'</span>
end_word = <span class="string">'fickle'</span></pre><p></p><p></p><p>Now let's generate a graph, where each node is the word and an edge represents, the index where the words differ and the character that is different.</p><p></p><p><img src="http://i.imgur.com/RCSiCGm.png" style=""></p><br><p>Next, using <a href="http://en.wikipedia.org/wiki/Dijkstra's_algorithm" target="_window">Djiktra's algorithm</a>, just find the shortest path between the <i>start_word</i> and <i>end_word</i>::</p><p></p><p><img src="http://i.imgur.com/2hUFmRf.png" style=""></p><p></p><p>Code here(also generates images using graphviz):</p>
<script src="https://gist.github.com/shadabahmed/d205cd0aa8fdbb69f17e.js"></script>
</div>
</div>
