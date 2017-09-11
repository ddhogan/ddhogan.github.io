---
layout: post
title:  "push/pop vs. shift/unshift"
date:   2017-09-11 20:55:00 +0000
---


I love to make tables to help me integrate ideas or objects and how they relate to each other.  

When I was learning how to work with arrays in Ruby, the words for functions `.push`, `.pop`, `.shift`, and `.unshift`, didn’t carry any intrinstic meaning or distinction to me in terms of working with an array (unlike `.collect`, and `.size`, which do).  I kept getting confused about which thing returns an element, and which one returns the rest of the array, and whether it acts on the beginning or the end.  So, here’s my table!

|   	  |beginning|end|
|-------|:--------:|:----:|
|adding, returns array| .unshift|.shift|
|returns removed element| .push| .pop|


also: `<<` (shovel) is syntactic sugar for `.push`
