---
layout: post
title: "RangeCountTree - A BST for range intersections"
description: ""
category: algorithms
tags: [ruby, algorithms]
permalink: blog/2013/03/09/rangecounttree-a-binary-search-tree-for-ranges
---
{% include JB/setup %}
Recently I came across an interesting puzzle:

> An editor of The History of the World of Science wants to find out the time when the largest number 
> of prominent scientists were alive. The prominent scientists are, by definition, the people mentioned > in the book with the dates of their birth and death. (No living scientists are included in the book.) ?
> Devise an algorithm for this task if it has the book's index as its input. The entries in the index are 
> sorted alphabetically and give the persons' birth and death years. If person A died the same year >person B was born, assume that the former event happened before the latter one.

After giving it a bit of thought, I implemented an interesting data structure to solve it like a basic binary search tree. Here it goes:

<script src="https://gist.github.com/shadabahmed/5123498.js"></script>

Here's the tree generated for the data in scientists.rb(using graphviz):

![tree](/assets/images/Ss7FYI0.png){: style="width:419px" }
