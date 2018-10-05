---
layout: post
title:      "Scope and Hoisting of variables, functions, and `this` in JavaScript"
date:       2018-08-18 18:58:43 -0400
permalink:  scope_and_hoisting_of_variables_functions_and_this_in_javascript
---

> *Blog posts and articles abound for this topic (a reference list follows) so I will attempt to highlight common pitfalls for beginner-intermediate web development students learning Javascript.*  

### Scope
-----

Although JavaScript is an interpreted language, implementation in a web development setting involves a step immediately prior to execution called lexical scoping (tokenizing), in which the interpreter skims through your code and identifies all the variables you've declared, makes a note of when they're reassigned, and delineates chunks of code into **scopes** in three levels: block, function, and global.

{% highlight javascript %}
// Example 1 (credit MDN)
function exampleFunction() {
    var x = "declared inside function";  // x can only be used in exampleFunction
    console.log("Inside function");
    console.log(x);
}

console.log(x);  // Causes Reference Error
{% endhighlight %}

The function's scope includes the variable x, so that variable is only known within that function.  Trying to access it out in the global scope will throw an error because x is not a declared variable (it's not even undefined).

If we move that `var` declaration outside the function, it'll be in the global scope, everyone knows about it, and we can access it within and outside of the function:

{% highlight javascript %}
// Example 2 (credit MDN)
var x = "declared outside function";

exampleFunction();

function exampleFunction() {
    console.log("Inside function");
    console.log(x);
}

console.log("Outside function");
console.log(x);
{% endhighlight %}

With the advent of ECMAScript2015 (aka "ES6"), two new ways to declare variables were introduced: `let` and `const`, which are significant because they enable more granular control over the scope in which a variable is available.
Both `let` and `const` define local variables which are available only in the level in which they're defined (whether a code block or function, and any contained sub-blocks).

### Hoisting
--------

In the following example, x is declared with `var`, and that same variable called x is known throughout the function (even on lines which precede it!) and in sub-blocks.  If x is declared with the newer `let` or `const`, then outer scopes don't have access to it, and if we "`let x;`" again in a sub-block, it's effectively a different variable (like how human twins separated at birth, but given the same name, are not the same person).

{% highlight javascript %}
// Example 3 (credit MDN)
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
{% endhighlight %}

This is relevant because of the lexing step that happens immediately prior to executing the code, and the fact that this:

{% highlight javascript %}
var x = 5;
{% endhighlight %}

is two steps listed on one line: the *declaration* of the variable x, and the *assignment* of the integer 5 to that variable. We could also write it like this:

{% highlight javascript %}
var x;
x = 5;
{% endhighlight %}

When the variable is declared with `var`, its value is undefined, but it is known to be a variable.  Then the next line makes x's value equal to 5. But, this isn't the case with `let` and `const`.  Welcome to the [Temporal Dead Zone](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone).

{% highlight javascript %}
// Example 4 (credit MDN)
function do_something() {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError
  var bar = 1;
  let foo = 2;
}
{% endhighlight %}

Within this function, the declaration of `bar` is ***hoisted*** to the top of the scope, in this case, the code contained within the function `do_something()`.  So, effectively, it's executed like this:

{% highlight javascript %}
// Example 5
function do_something() {
  var bar;
  console.log(bar); // undefined
  console.log(foo); // ReferenceError
  bar = 1;
  let foo = 2;
}
{% endhighlight %}

which is why trying to `console.log(bar)` yields `undefined` (we know it's a variable, it just has no value), while `console.log(foo)` throws a reference error ("a variable called 'foo'? what are you even talking about, human?")

This makes things like this possible:

{% highlight javascript %}
// Example 6 (credit MDN)
num = 6;
console.log(num); // returns 6
var num;
{% endhighlight %}

and:

{% highlight javascript %}
// Example 7 (credit MDN)
dogName("Watson");

function dogName(name) {
  console.log("My dog's name is " + name);
}
// "My dog's name is Watson"
{% endhighlight %}

In the first example, even though it looks like `var num` is declared after we assign it, from the computer's perspective, it noticed that we've declared it in the relevant scope (global), pins that to the top, and then proceeds with executing the rest of the code.  In the second example, even though we invoke/call the function before we've defined it, that definition is hoisted to the top of the scope, so by the time we actually start executing the code, the interpreter already knows what `dogName()` is.  
For `var` variables, note that only the declaration gets hoisted, not the assignment.  So, in Example 6, writing it like this:

{% highlight javascript %}
console.log(num); // returns undefined
var num = 6;
{% endhighlight %}

returns undefined.
**For this reason, it's often suggested to always declare variables at the top of the scope they're in, so you remember the order in which the interpreter will execute your code.**
Alternatively, using `let` and `const` offer some protection against this behavior, since variables declared this way are not initialized with a value of 'undefined'.  So even though they're hoisted, you'll still get a reference error because they won't be initialized until they're assigned.  It's almost like they're not being hoisted at all.
`const` has an added advantage of protecting against unexpected reassignment (although an object declared this way may still have its properties modified), like so:

{% highlight javascript %}
// Example 8 (credit Digital Ocean)
// Create a CAR object with two properties
const CAR = {
    color: "blue",
    price: 15000
}

// Modify a property of CAR
CAR.price = 20000;

console.log(CAR);

// Output
// { color: 'blue', price: 20000 }
{% endhighlight %}

Similarly, functions follow similar rules.
Function *declarations* are hoisted:

{% highlight javascript %}
// Example 9 (credit Elizabeth Mabishi at Scotch.io)
hoisted(); // Output: "This function has been hoisted."

function hoisted() {
  console.log('This function has been hoisted.');
};
{% endhighlight %}

... while function *expressions* are not:

{% highlight javascript %}
// Example 10 (credit Elizabeth Mabishi at Scotch.io)
expression(); //Output: "TypeError: expression is not a function

var expression = function() {
  console.log('Will this work?');
};
{% endhighlight %}

### How `this` fits into this
-------------------------

A related topic, which I will discuss as it relates to scope and hoisting, is `this`.
Gordon Zhu created a nice [cheatsheet](https://github.com/gordonmzhu/cheatsheet-js) which summarizes the corresponding lesson in his *life-changing* curriculum on [Watch & Code](https://watchandcode.com/).  Essentially `this` is scope dependent.
If it's called within a regular function or just out in the wild, `this` points to `window`.
If you're in a function being called as a method, `this` points to the object being acted upon (whatever's just to the left of the dot).  

{% highlight javascript %}
// Example 11 (credit Gordon Zhu)
var myObject = {
  myMethod: function() {
    console.log(this);
  }
};
myObject.myMethod(); // --> myObject
{% endhighlight %}

An extension of this is the case where `this` is used in a constructor as in Example 12, in which `this` refers to the instance of the class Person:

{% highlight javascript %}
// Example 12 (credit MDN)
function Person(name, age) {
  this.name = name;
  this.age = age;
}
{% endhighlight %}

Explicitly setting the value of `this` using `call`, `bind`, and `apply` is outside the scope (haha!) of this blogpost.

And in a callback function, it depends on where (what scope) `this` is being called in, in accordance with the previous examples.

## References
-----

* [Javascript’s lexical scope, hoisting and closures without mystery](https://medium.com/@nickbalestra/javascripts-lexical-scope-hoisting-and-closures-without-mystery-c2324681d4be)
* [Understanding Variables, Scope, and Hoisting in JavaScript ― DigitalOcean](https://www.digitalocean.com/community/tutorials/understanding-variables-scope-hoisting-in-javascript#difference-between-var,-let,-and-const)
* [Understanding Hoisting in JavaScript ― Scotch](https://scotch.io/tutorials/understanding-hoisting-in-javascript)
* [Hoisting - MDN Web Docs Glossary: Definitions of Web-related terms](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
* [Scope - MDN Web Docs Glossary: Definitions of Web-related terms](https://developer.mozilla.org/en-US/docs/Glossary/Scope)
* [Grammar and types - JavaScript MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Declarations)
