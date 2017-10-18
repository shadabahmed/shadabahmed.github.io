---
layout: post
title: "BST Inorder Sucessor"
description: ""
category:
tags: [algorithms, trees]
---
{% include JB/setup %}

Getting back to blogging after a long time. Whatever be the reasons, just want to share a quick insight I had yesterday.

Finding an in-order successor to a BST node is a pretty popular problem. While trying to think of it, I realized I could create a more elegant solution than what I could find online. I don't know if similar solutions exist but here is a ruby implementation anyway (explanation within the comments):

```ruby
# let's quickly create a tree
class Node < Struct.new(:val, :left, :right); end
root = Node.new(4)
root.left = Node.new(2)
root.left.left = Node.new(1)
root.left.right = Node.new(3)
root.right = Node.new(6)
root.right.left = Node.new(5)
root.right.right = Node.new(7)

# finding min in a tree, simple to do
def find_min(node)
  return if node.nil?
  if node.left.nil?
    node
  else
    find_min(node.left)
  end
end

def find_successor(node, val, possible_successor = nil)
  return if node.nil?
  if val == node.val && node.right
  # if found and node has right, then successor is min in right
    find_min(node.right)
  elsif val == node.val
  # if found and node has no right, then it is one of the ancestors
    possible_successor
  elsif val < node.val
  # if the value is on the left subtree,
  # then current node is possible successor.
  # Pass it as possible_successor
    find_successor(node.left, val, node)
  else
  # if the value is on the right subtree,
  # then current node can't be possible successor.
  # Pass along ancestor which could be successor
    find_successor(node.right, val, possible_successor)
  end
end

# test by calling find_successor method
find_successor(root, 3).val
=> 4
```

I'll be more frequent with new posts now. I promise :)
