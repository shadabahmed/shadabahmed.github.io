---
layout: post
title: "Ditch jsFiddle - Codepen is awesome"
description: ""
category: javascript
tags: [javascript, html]
permalink: blog/2013/03/25/ditch-jsfiddle-codepen-is-awesome
---
{% include JB/setup %}
If you like __[jsFiddle](http://jsfiddle.net)__ then you should definitely try __[Codepen](http://codepen.io)__. I am now a Codepen convert for the reasons listed.

## Reasons why __[Codepen](http://codepen.io)__ is better than __[jsFiddle](http://jsfiddle.net)__

* The most **awesome** feature on *Codepen* is **live update** . No need to save and reload unlike *jsFiddle*

* Another awesome feature on *Codepen* - **Compiled preview** for both *CoffeScript* and *SCSS*. See below:

![Codepen Preview](http://i.imgur.com/2wnXZjZ.png)

* More options for markup, apart from HTML

![HTML Options Codepen](http://i.imgur.com/oH2ZqtY.png)

* Fully SCSS compliant. *jsFiddle* fails to compile SCSS when I added this:
```
.arrow::after{
  content: ">";
}
```

* Complete error description for SCSS unlike jsFiddle, which silently fails and you have to dig with your browser inspector tool.
 
![jsFiddle SCSS Error](http://i.imgur.com/7ZXOUGS.png)
*jsFiddle Error - Silently renders scss as css and just adds an error message in the beginning of the file*

![Codepen SCSS Error](http://i.imgur.com/E3MMdXV.png)
*Codepen Error - Matching line number and error description*

* For *CoffeeScript*, *Codepen* shows you a similar error banner like the scss error but does not show any details. This is atleast better than *jsFiddle* which fails silently

* The UI is your preference, but I had no problems switching from jsFiddle to Codepen.

## Cool Codepens

This is a simulator I created for the problem:

> ## N Rumors
> 
> There are n people, each in possession of a different rumor. They want to share the news with each 
> other by sending electronic messages. What is the minimum number of messages they need to send 
> to guarantee that everyone of them gets all the rumors? Assume that a sender includes all the 
> rumors he or she knows at the time the message is sent and that a message may only have one 
> addressee.

Each cell in the simulator has the person number in blue and number of rumors with the person in green. If a person is sending out a message his cell becomes pink and the cells for people receiving the message are green. Full code [here](http://codepen.io/shadabahmed/pen/EItGg). Works in Chrome and Firefox.

<pre class="codepen" data-height="520" data-type="result" data-href="EItGg" data-user="shadabahmed" data-safe="true"><code></code><a href="http://codepen.io/shadabahmed/pen/EItGg">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

## Much more impressive codepens:

* [Falling Leaves](http://codepen.io/anandylaanbaatar/pen/tqdmv)
* [Tearable Fabric](http://codepen.io/anon/pen/Jveaj)
* [Wall Clock](http://codepen.io/tylerbre/pen/gqdnA)

You will find many more cool codepens on their homepage. Happy Codepenning :)
