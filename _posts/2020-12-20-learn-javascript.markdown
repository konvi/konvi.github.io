---
layout: post
title:  "Finally reading learn.javascript.ru"
date:   2020-10-09 21:14:44 -0700
---
Decided to read [JavaScript Tutorial https://javascript.info/]

Below are the notes for future me.

### this 

From [https://javascript.info/object-methods](https://javascript.info/object-methods)

This is the object before '.'
{% highlight javascript %}
let dog = { sound: "woof" };
let cat = { sound: "meow" };

function converse() {
  alert(this.sound);
}

dog.f = converse;
cat.f = converse;

// "this" inside the function is the object "before the dot"
dog.f(); // woof  (this == dog)
cat.f(); // meow  (this == cat)
{% endhighlight %}

### Call, Apply, Bind
How to keep "this"? Pass it to the function invocation.

The only syntax difference between call and apply is that call expects a list of arguments, while apply takes an array-like object with them.

So these two calls are almost equivalent:

{% highlight javascript %}
func.call(context, ...args); // pass an array as list with spread syntax
func.apply(context, args);   // is same as using call
{% endhighlight %}

Bind doesn't invoke a function. It creates an "exotic object" instead:
{% highlight javascript %}
let user = {
  firstName: "Vasya"
};

function func(prefix) {
  console.log(`${prefix} ${this.firstName}`);
}

let funcUser = func.bind(user);
funcUser("Hi"); // Vasya

function wrapper(prefix) {
  func.call(user, prefix);  //"Bye Vasya"
  func.apply(user, arguments);  //"Bye Vasya"
}
wrapper("Bye");
{% endhighlight %}

Arrow functions have no "this" and no "arguments". They use "this" and "arguments" from their closure.
{% highlight javascript %}
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}
{% endhighlight %}

### Prototypes, inheritance
"prototype" is a property of a function, [[Prototype]] is a property of an object, __proto__ is a historical getter/setter for [[Prototype]], but we'll use Object.getPrototypeOf/Object.setPrototypeOf.

Some examples are too frightening:
{% highlight javascript %}
Function.prototype.defer = function(ms) {
  let f = this;
  return function(...args) {
    setTimeout(() => f.apply(this, args), ms);
  }
};

// check it
function f(a, b) {
  alert( a + b );
}

f.defer(1000)(1, 2); // output 3 in 1 second
{% endhighlight %}

### Classes

{% highlight javascript %}
class User {
}
console.log(typeof User);
// prints 'function'
{% endhighlight %}

