---
title: this keyword in JavaScript 101
date: 2023-12-31
---
## Outline
1. Global Objects and This
2. This and Classes
3. Call, Apply, and Bind Methods
4. More on Binding (Arguments, Event Listeners, Timers)
5. Arrow Functions and This

Although we've encountered the keyword `this` multiple times before, its role might still seem mysterious. Does it serve the same purpose when it appears in objects and classes? Let's (hopefully) shed some light on this in the following post!
## 1. Global Objects and This
Before delving into the intricacies of `this`, let's briefly examine a familiar snippet below and explore how things truly function.
```
function greeting() {
  console.log("Hello world!");
}
```
Calling this function is simple:
```
greeting();    // 'Hello world!'
```
While nothing particularly noteworthy is happening here, it might surprise you (or not) to discover that behind the scenes, we're calling the `greeting` function on the `Window` object. The `Window` object is a built-in global object that the browser provides when running our code. As a result, calling the `greeting` function in the above example is effectively equivalent to invoking it on the `Window` object.
```
Window.greeting();    // 'Hello world!' -> Same as calling 'greeting()' itself!
```
It turns out that JavaScript doesn't have pure functions. Every function we invoke is, in fact, attached to some objects, essentially making them all methods. Indeed, when we print out the `this` value inside the `greeting` function, the `Window` object reveals itself with a multitude of properties and methods.
```
function greeting() {
  console.log("This is: ", this);
}

greeting();
```
This will return a really big object, starting with something like this:

![image](https://github.com/luchd/OneOhOne/assets/30389774/ab40d8fc-f646-4e36-8b0d-5521d0ad7677)

So, let's revisit our initial question: What exactly is `this`? Think of `this` as the entity that triggers the method. In simpler terms, it's the object on the left side of the dot notation. By default, when we declare a function or a variable in the global scope, we're essentially attaching it to the `Window` object (or some other global objects). Consequently, when we invoke it, the `Window` object is implicitly added to the left side of the dot notation. However, things become more intriguing when we explore the concept of `this` within a class, a topic we'll delve into in the next section.
## 2. This and Classes
Let's kick off this section with a snippet that's quite familiar to us.
```
class Pet {
  constructor(name) {
    this._name = name;
    this._isCute = true;
  }
  getName() {
    return `Hi there, my name is ${this._name}!`;
  }
  static whatIsThis() {
    console.log("This is: ", this);
  }
}
```
Why don't we initiate an instance and see how it works?
```
const zily = new Pet("Zily");
console.log(zily);    // Pet { _name: 'Zily', _isCute: true }
console.log(zily.getName());    // 'Hi there, my name is Zily!'
```
Of course, that should work as expected! Now, let's delve into the details of the `getName` method.
```
console.log(zily.getName());    // 'Hi there, my name is Zily!'
```
We are aware that this is an instance method, so we need to attach it to an instance to make it work. As discussed earlier, the `this` within the `getName` method refers to the `zily` instance, allowing us to retrieve its name using `this._name`.
Now, let's delve into something interesting!
```
const gName = zily.getName;
```
In essence, we're storing the `getName` function's definition in a variable named `gName`. What happens when we invoke `gName`?
```
console.log(gName());
```
Oh no! Depending on different browsers and interpreters, you might come across various error messages. Nevertheless, a common one could be something like `Uncaught TypeError: Cannot read properties of undefined (reading '_name')`. Considering our knowledge so far, you can expect the browser to interpret this line of code somewhat like this!
```
Window.console.log(Window.gName());
```
The `Window.console.log()` is provided by the browser, functioning smoothly. However, the `gName()` function, or more precisely, `Window.gName()`, seems to be the culprit behind the error. This is logical since, upon examining the definition of the `gName` function (copied from `zily.getName`), we notice that this line involves the use of the `this` keyword:
```
return `Hi there, my name is ${this._name}!`;
```
Unfortunately, we're not calling `gName` within a class or invoking it on a specific instance. Instead, it's called in the global scope, so one might assume that this should reference the `Window` object. This assumption seems reasonable and aligns with our experience so far. However, if that were the case, why doesn't `this._name` return `undefined`? We haven't declared any `_name` property in the global scope, so we should expect to get `undefined` back. Surprisingly, it throws an error.

The error message provides a clue that challenges our initial analysis: `Cannot read properties of undefined (reading '_name')`. The part `_name` only occurs with `this._name`, suggesting that this is `undefined`. Hence, the interpreter couldn't read the property (`_name`) of `this` (which is `undefined`). Whenever we try to access anything from an `undefined` object, an error occurs. This leaves me with a big question mark! What's happening? Isn't this a `Window` object?

I'm sorry to say that in this case, `this` is not a `Window` object as it initially seems! Actually, `this` is first introduced inside the `Pet` class, referencing either an instance or the class itself. However, when we bring it outside of the class and attempt to make a call in the global scope, `this` is unfortunately `undefined`; it references nothing in this context, and we lose the connection of `this` with the desired object (`Pet` in this case). This is how `this` behaves inside and outside of a class. The good news is there are a couple of ways to make `this` "point" to the expected object, which we will explore in the next section. It's not the end of the world, is it?
## 3. Call, Apply, and Bind Methods
There are a couple of methods that assist us in specifying the context of the `this` keyword. In simpler terms, these methods help us set the value of `this` to refer to the desired object. Before we dive into discussing these helpful methods, let's define two literal objects; this way, we'll have something to work with along the way.
```
const javascript = {
  name: "JavaScript",
  creator: "Brendan Eich",
  firstAppeared: 1995,
  helloWorld: function() {
    return `Hello world, I'm ${this.name}. I was designed by ${this.creator} in ${this.firstAppeared}.`
  },
  influencedBy: function(...langs) {
    const langsToStr = langs.join(',');
    return `I was influenced by ${langsToStr}.`;
  }
}

const python = {
  name: "Python",
  creator: "Guido van Rossum",
  firstAppeared: 1991
}
```
### 3.1. Call
The `call` method can be expressed as "call the function on something".
```
const js = javascript.helloWorld;
console.log(js());    // ERROR
console.log(js.call(javascript));    // 'Hello world, I'm JavaScript. I was designed by Brendan Eich in 1995.'
```
Using the `call` method is quite straightforward. Just invoke `call` on a function and pass the object to which we want `this` to refer. In the example above, we aim to call the function `js` (which is essentially `javascript.helloWorld` behind the scenes) on the object `javascript`. Now, the `this` keyword inside the `helloWorld` function will point to the `javascript` object itself, eliminating the issue of it being `undefined`. Additionally, we can effortlessly pass arguments into the `call` method.
```
const js = javascript.influencedBy;
console.log(js.call(javascript, "JS+", "TypeScript", "CoffeeScript"));    // 'I was influenced by JS+,TypeScript,CoffeeScript.'
```
A quick note on the `call` method: upon invocation, it not only sets the value of the `this` keyword to the passed-in object but also executes the function on the new object.
### 3.2. Apply
The `apply` method functions much like the `call` method, differing only in its treatment of arguments. Instead of passing arguments individually, the `apply` method expects them to be provided as a list.
```
const js = javascript.influencedBy;
console.log(js.apply(python, "CoffeeScript", "Go", "Julia"));    // ERROR
console.log(js.apply(python, ["CoffeeScript", "Go", "Julia"]));    // 'I was influenced by CoffeeScript,Go,Julia.'
```
Much like the `call` method, `apply` also sets the value of `this` and executes the function on the new object simultaneously. One drawback of both the `call` and `apply` methods is the need to tediously invoke them every time we want to execute a function with a specific `this` context. Consequently, they are not as popular and are often used less compared to the method we will discuss next.
### 3.3. Bind
The `bind` method serves the same purpose. However, instead of returning the evaluated result of invoking the function on the new object, it returns a new function where the `this` keyword is always bound to the new object.
```
const js = javascript.helloWorld;
const helloWorldFromPython = js.bind(python);
```
Having said that, `helloWorldFromPython` is essentially a function produced by applying the `bind` method to the `javascript.helloWorld` method. Consequently, to execute the resulting function, we must invoke it ourselves.
```
console.log(helloWorldFromPython());    // 'Hello world, I'm Python. I was designed by Guido van Rossum in 1991.'
```
Moving forward, whenever we want to run the `helloWorld` function on the `python` object, we can effortlessly invoke the `helloWorldFromPython` function without the need to repeatedly bind the `this` keyword.
## 4. More on Binding (Arguments, Event Listeners, Timers)
Due to the flexibility of the `bind` method, it is more popular than others. Let's delve into some additional details about `bind`.
### 4.1. Arguments
Much like both the `call` and `apply` methods, we can pass arguments to the `bind` method, and the syntax remains identical to that of the `call` method.
```
const js = javascript.influencedBy;
const influencedByFromPython = js.bind(python, "CoffeeScript", "Go", "Julia");
console.log(influencedByFromPython());    // 'I was influenced by CoffeeScript,Go,Julia.'
```
In certain situations, we might wish to duplicate a function into another variable without concern for the `this` keyword. In such instances, we can pass `null` as the first argument. Let's go ahead and define a function in the global scope to delve deeper into this concept.
```
function add(a, b) {
  return a + b;
}
const addCopy = add.bind(null, 2);
```
We've just crafted a copied version of the `add` function where the value of the `this` keyword is inconsequential (which makes sense, considering there's no `this` in the `add` function). Additionally, we've set the value of `a` to the number `2`. However, exercise caution, as the `addCopy` function now appears like this under the hood.
```
function addCopy(b) {
  return 2 + b;
}
```
Now, the new function only requires one argument, unlike the original, which took two. Therefore, when we invoke it, we only need to provide the value for `b`. If we pass in more than one value, it will consider the first one as the value for `b` and ignore the rest.
```
console.log(addCopy(3));    // 5
console.log(addCopy(9, 4));    // 11 -> Only 9 being used as 'b', 4 is ignored
```
### 4.2. Event Listeners
Yet another scenario where `this` may come into play is within event listeners. Let's arrange a few components on an HTML page to explore how things unfold.
```
const swe = {
  name: "Lucas",
  helloWorld: function() {
    console.log(`${this.name} says Hello World!`);
  }
}
const clickMeBtn = document.querySelector("#clickMe");
clickMeBtn.addEventListener('click', swe.helloWorld);
```
Consider a button element with the id `clickMe` on an HTML page, selected using `document.querySelector`. We've set up a click event for the button, intending to trigger the `swe.helloWorld` function upon clicking. Unfortunately, this won't work as expected since the `this` keyword refers to the button element, resulting in `this.name` returning an empty string. One viable solution is to bind `this` with the `swe` object.
```
const clickMeBtn = document.querySelector("#clickMe");
clickMeBtn.addEventListener('click', swe.helloWorld.bind(swe));
```
Though not entirely new, this is another scenario where `this` may come into play, and the `bind` method can be utilized.
### 4.3. Timers
Another common scenario where binding the `this` keyword may be necessary is in timer functions such as `setTimeout` or `setInterval`.
```
class Counter {
  constructor(starting = 0, increment = 1) {
    this.count = starting;
    this.increment = increment;
  }
  start() {
    setInterval(function() {
      console.log(this.count);
      this.count += this.increment;
    }, 1000);
  }
}
```
Let's create an instance of `Counter` and call the `start` method to observe how things unfold.
```
const counter = new Counter();
counter.start();
```
As you might suspect, this is not functioning as the desired counter. It will result in 'NaN' or an error, depending on the environment in which we're running the code. The issue stems from the fact that `setInterval` is invoked as `Window.setInterval` behind the scenes, causing the `this` keyword to refer to the global `Window` object. Consequently, it lacks any `count` or `increment` properties. This situation underscores the need to bind the `this` value to a `Counter` instance.
```
class Counter {
  constructor(starting = 0, increment = 1) {
    this.count = starting;
    this.increment = increment;
  }
  start() {
    setInterval((function() {
      console.log(this.count);
      this.count += this.increment;
    }).bind(this), 1000);
  }
}
```
With this binding, it should function correctly now, but further improvements could be made with some refactoring for a cleaner implementation.
```
class Counter {
  constructor(starting = 0, increment = 1) {
    this.count = starting;
    this.increment = increment;
  }
  incrementAndPrint() {
    console.log(this.count);
    this.count += this.increment;
  }
  start() {
    setInterval(this.incrementAndPrint.bind(this), 1000);
  }
}
```
Everything seems in order, and it functions as anticipated. Nonetheless, the next section will explore an even more elegant solution to the `this` problem, introduced in later versions of JavaScript.
## 5. Arrow Functions and This
It's intriguing that we can employ a new syntax known as 'arrow functions' to maintain the context of the this `value` consistently across functions. With arrow functions, there's no longer a need to manually bind `this` to desired objects, as arrow functions do not create a new `this` object.
```
class Counter {
  constructor(starting = 0, increment = 1) {
    this.count = starting;
    this.increment = increment;
  }
  start() {
    setInterval(() => {
      console.log(this.count);
      this.count += this.increment;
    }, 1000);
  }
}
```
Thanks to this convenient syntax, we no longer have to bother with manually binding `this`. How convenient it is!
## Key takeaways
- Both functions and properties in JavaScript are manipulated through an object, whether it's a default global object or a user-defined one.
- In the context of classes, `this` points to either the class itself or an instance. However, it might become `undefined` when copied to a variable in the global scope.
- If you want to set the value of `this` to a specific object, you have the options of using the `call`, `apply`, or `bind` methods. Among these, `bind` is the more popular choice.
- Arrow functions are useful whenever we need to retain the context of `this`.
