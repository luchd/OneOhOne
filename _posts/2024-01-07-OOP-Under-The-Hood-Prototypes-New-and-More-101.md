---
title: OOP Under The Hood - Prototypes, New, and More 101!
date: 2024-01-07
---
## Outline
1. The New Keyword
2. Prototypes
3. The Prototype Chain
4. Classes, Inheritance, and Prototypes
5. `__proto__` vs. `prototype`
6. Useful Prototype Methods

Object-Oriented Programming (OOP) stands as a pivotal concept and serves as the backbone for many software design principles. Despite often appearing mysterious and abstract, I find it intriguing. Every time I encounter it, a sense of intimidation arises, yet I firmly believe that delving into its intricacies will pave the way for a meaningful and close relationship. Join me on this exhilarating journey as we unravel the beauty inherent in OOP!

## 1. The New Keyword
The `new` keyword proved to be one of the most abstract concepts I encountered when initially delving into OOP. Despite encountering it multiple times, I often dismissed it and convinced myself of understanding. Now, it's time to demystify some of its intricacies and recognize its significance! The `new` keyword appears in various scenarios, but its two most prevalent use cases are preceding a class and preceding a function.

```
const myArray = new Array();    // creating an empty array []
const myObject = new Object();    // creating an empty object {}
```

Essentially, the new keyword performs four crucial tasks behind the scenes:
1. Creates an empty object
2. Binds the `this` keyword to that object
3. Implicitly returns the object - `return this`
4. Creates a link to the object's prototype

Prior to the introduction of classes in JavaScript, the process of instantiating an object involved creating [a constructor function](## "This is not a constructor of a class, but just a convention to call a function that is creating an object. By convention, this function starts with an uppercase letter."). Let's create one to explore how the initial three steps function.

```
function Company(name, numOfEmployees, isStartUp) {
  this.name = name,
  this.numOfEmployees = numOfEmployees,
  this.isStartUp = isStartUp,
  this.fundraise = function(amount) {
    return `${this.name} is fundraising to ${amount}.`;
  }
}
```

In the domain of OOP, we refer to the `Company` function as a constructor since its purpose is to create an object. As per convention, the function name starts with an uppercase letter, distinguishing it from class constructors. Now, let's call the `Company` function to instantiate a `Company` object, following our usual practice.

```
const myCom = Company("OneOhOne", 16, true);
console.log(myCom);    // undefined
```

Unfortunately, instead of an object, we receive `undefined`. This outcome aligns with our expectations since we are not currently returning anything from the `Company()` function. Given that we are dealing with the `Company()` function, our assumption is that the `this` keyword should reference the 'Company' object. To verify this, let's enhance the `Company` function by incorporating a `return this` statement.

```
function Company(name, numOfEmployees, isStartUp) {
  this.name = name,
  this.numOfEmployees = numOfEmployees,
  this.isStartUp = isStartUp,
  this.fundraise = function(amount) {
    return `${this.name} is fundraising to ${amount}.`;
  }
  return this;
}
```

Re-instantiate `myCom` variable to confirm our assumption.

```
const myCom = Company("OneOhOne", 16, true);
console.log(myCom);    // Window {...}
```

As anticipated, `myCom` turns out to be a `Window` object, a concept we explored in a previous post. If you're unfamiliar with this behavior, you can gain more insights about the `this` keyword at [this Keyword in JavaScript 101](https://luchd.github.io/OneOhOne/2023/12/31/this-in-JavaScript-101.html).
Once again, the result returned is not the 'Company' object we intend to work with. This raises the question: 'Can we somehow bind `this` to the `Company` object?' Honestly, I'm unsure of the possibility or the method to achieve it. However, the `new` keyword comes to our rescue, making the process considerably more straightforward.

```
const myCom = new Company("OneOhOne", 16, true);
console.log(myCom);    // Company { name: 'OneOhOne', numOfEmployees: 16, isStartUp: true, fundraise: function(amount) { return `${this.name} is fundraising to ${amount}.`; } }
```

Voila! We now have the 'Company' object with the desired properties and methods returned. With the `new` keyword, there's no need to explicitly return `this`, adding to the convenience of the process.

```
function Company(name, numOfEmployees, isStartUp) {
  this.name = name,
  this.numOfEmployees = numOfEmployees,
  this.isStartUp = isStartUp,
  this.fundraise = function(amount) {
    return `${this.name} is fundraising to ${amount}.`;
  }
}

const myCom = new Company("OneOhOne", 16, true);
console.log(myCom);    // Company { name: 'OneOhOne', numOfEmployees: 16, isStartUp: true, fundraise: function(amount) { return `${this.name} is fundraising to ${amount}.`; } }
```

This refactoring operates effectively, yielding the exact same result as the previous version. Let's now delve deeper into understanding how the `new` keyword operates under the hood. We'll follow and examine the first three steps outlined at the beginning of this section using the example above.

```
const myCom = new Company("OneOhOne", 16, true);
console.log(myCom);
```

1. Creating an empty object

```
myCom = Company { }
this = Window { }
```

2. Bind the `this` keyword to the object

```
myCom = Company { }
this = myCom { }
```

3. Implicitly return the object

```
myCom = Company { }
this = myCom { }

... adding some stuff
... like properties
... and methods

return this
```

Moreover, the `new` keyword performs a crucial task by automatically establishing a link to the object's prototype. Before delving into this, let's first understand what a prototype is in the upcoming section.
## 2. Prototypes
Before delving into the intricacies of prototypes, let's take a moment to review how objects are created, exploring both the modern `class` syntax and the traditional constructor functions.

```
class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }
  bark() {
    return `${this.name} says woof!`;
  }
  sleep() {
    return `${this.name} is sleeping!`;
  }
}

const yel = new Dog("Yel", "Akita");
console.log(yel);
```

We've just instantiated a Dog object named 'yel' using the 'Dog' class. Each Dog instance features properties like 'name' and 'breed,' along with methods such as 'bark()' and 'sleep()'. However, upon inspecting the 'yel' instance, we observe that only its properties (name and breed) are visible.

![image](https://hackmd.io/_uploads/HyZDBB_uT.png)

Although we can call the 'bark()' and 'sleep()' methods, interestingly, they seem 'invisible' when inspected through the console!

```
console.log(yel.bark());    // 'Yel says woof!'
console.log(yel.sleep());    // 'Yel is sleeping!'
```

If we create another Dog instance, we still observe the same behavior.

```
const taki = new Dog("Taki", "Collie");
console.log(taki.bark());    // 'Taki says woof!'
console.log(taki.sleep());    // 'Taki is sleeping!'
```

All functionalities are operating smoothly. What becomes intriguing is when we compare the 'bark()' (or 'sleep()') method between these two instances.

```
console.log(yel.bark === taki.bark);    // true
```

The surprising result of 'true' in the comparison suggests that both 'yel' and 'taki' are internally calling the same function. Essentially, the 'bark()' (or 'sleep()') method shares a common reference in memory. However, it's important to note that this behavior does not apply to the constructor function.

```
function Dog(name, breed) {
  this.name = name;
  this.breed = breed;
  this.bark = function() {
    return `${this.name} says woof!`;
  }
  this.sleep = function() {
    return `${this.name} is sleeping!`;
  }
}

const odo = new Dog("Odo", "Poodle");
```

We've just defined the same Dog blueprint and instantiated an object named 'odo.' Upon inspection, we now observe the complete structure of the Dog, rather than just its individual properties.

```
console.log(odo);
```

![image](https://hackmd.io/_uploads/Sy7dBrOd6.png)

Similar to the 'class' behavior, this pattern persists for other instances.

```
const ada = new Dog("Ada", "Boxer");
console.log(ada);
```

![image](https://hackmd.io/_uploads/r1iuSHuuT.png)

It seems that each instance maintains its individual copies of the 'bark()' and 'sleep()' methods. This observation becomes evident when we perform a comparison, mirroring our previous assessment with the Dog class.

```
console.log(odo.sleep === ada.sleep);    // false
```

We obtained 'false,' indicating that the 'odo' and 'ada' instances are referencing two distinct memory locations for the 'sleep()' (or 'bark()') method. This behavior isn't efficient because, with each new instance creation, it duplicates everything from the constructor function and stores it independently. Unlike 'class,' this isn't the case.

A valid concern arises: how can we achieve the same efficiency as a class when using constructor functions? Enter the prototype!

The prototype is a fundamental mechanism enabling objects to inherit features and functionalities from one another. In JavaScript, every object, array, instance, etc., has its built-in prototype object, housing pre-defined common methods or functionalities. When we define methods in a class, they are automatically added to the prototype. Although there's no standard name for referring to a prototype, the widely used term, especially in Chrome, is `__proto__`. Let's print out the prototype of the 'taki' instance from the Dog class above to explore its contents.

```
console.log(taki.__proto__);
```

![image](https://hackmd.io/_uploads/SJ7Yrrd_p.png)

Voila! Here they are, the 'bark()' and 'sleep()' methods.
To move methods from a constructor function to its prototype, we can access the 'prototype' object, a pre-defined property for every function in JavaScript.

```
function Dog(name, breed) {
  this.name = name;
  this.breed = breed;
}

Dog.prototype.bark = function() {
  return `${this.name} says woof!`;
}

Dog.prototype.sleep = function() {
  return `${this.name} is sleeping!`;
}
```

With this Dog function refactoring, we can now utilize shared references for the 'bark()' and 'sleep()' methods, eliminating the need for duplication when creating new instances.

In terms of user-defined methods, it's worth noting that the 'constructor' function is consistently linked to each instance, pointing back to the function itself. This characteristic allows us to create a new instance using this constructor.

```
const genky = new taki.constructor("Genky", "Boxer");
console.log(genky);
```

This is functioning perfectly! Now, let's revisit the `new` keyword from the previous section, as it plays a crucial role in the prototype chain—especially in the context of the fourth behavior. We'll explore this in greater detail in the next section.
## 3. The Prototype Chain
Continuing with the Dog constructor function mentioned earlier, let's now create a new instance as illustrated below.

```
const foo = new Dog("Foo", "Bulldog");
```

We've observed that invoking the 'bark()' or 'sleep()' method presents no issues, thanks to these methods being defined in the Dog's prototype—ensuring they are shared among all instances. Additionally, we can explore the invocation of other methods.

```
console.log(foo.toString());    // '[object Object]'
```

Even though we obtained something not particularly useful, there are no errors! Where is the 'toString()' method defined? Unlike 'bark()' and 'sleep()', 'toString()' wasn't found inside foo's prototype. However, delving deeper into the prototype inside 'foo', we uncover a few other things, including the presence of the 'toString()' method.

![image](https://hackmd.io/_uploads/rJatBBuua.png)

Indeed, this is how JavaScript behaves when attempting to access a property or invoke a method on an object. If a property or method is not found on the current object, JavaScript then looks for it in the object's prototype. This search continues up the prototype chain until the property or method is found or the chain reaches its end. Let's illustrate this concept with another example.

```
const grandparent = {
  greet() {
    return "Nice to meet you!";
  }
}

const parent = {
  color: "green",
  sing() {
    return "La La La!";
  },
  __proto__: grandparent
}

const child = {
  number: 3,
  __proto__: parent
}
```

We might not typically perform such actions in real-world scenarios (directly accessing the `__proto__` property of an object is not recommended—we'll explore a better approach later), but it illustrates the concept. Here, we set the prototype of 'child' to the 'parent' object and repeat the process for 'parent' by assigning its prototype to the 'grandparent' object. Now, let's invoke the 'sing()' method on the 'child'.

```
console.log(child.sing());    // 'La La La!'
```

It begins by inspecting the 'child' object, but as the 'sing()' method isn't found there, JavaScript continues its search in the prototype object (which is the 'parent' object in this case). Fortunately, the 'sing()' method is defined there, bringing the search process to a halt, and we hear 'La La La!'. JavaScript maintains its relentless search for what we need, regardless of how deep the prototype chain extends.

```
console.log(child.greet());    // 'Nice to meet you!'
```

The process remains consistent when invoking the 'greet()' method, delving one level deeper into the prototype chain.

```
console.log(child.toString());    // '[object Object]'
```

Let's go one level deeper. Remember, every object has its own pre-defined prototype, with some built-in methods already present. Feel free to print the chain to examine it.

```
console.log(child.__proto__);    // -> parent object
console.log(child.__proto__.__proto__);    // -> grandparent object
console.log(child.__proto__.__proto__.__proto__);    // -> the pre-defined prototype object (shown below)
```

![image](https://hackmd.io/_uploads/Hy_5HSdOT.png)

## 4. Classes, Inheritance, and Prototypes
It appears that classes and inheritance in JavaScript essentially translate to prototypes and prototype chains. To gain a deeper understanding of the inner workings, let's revisit the Dog class introduced at the beginning of this post.

```
class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }
  bark() {
    return `${this.name} says woof!`;
  }
  sleep() {
    return `${this.name} is sleeping!`;
  }
}
```

Automatically, any methods defined within a class are added to its prototype. As a result, all instances can invoke these shared methods without the need to create individual copies. Moreover, when one class inherits from another, a prototype link is automatically forged between the derived and inherited classes, shaping a cohesive prototype chain.

```
class GuideDog extends Dog {
  constructor(name, breed, owner) {
    super(name, breed);
    this.owner = owner;
  }
  alert() {
    return `${this.name} alerts you to danger.`;
  }
}
```

Thanks to the default mechanism, accessing the 'bark()' and 'sleep()' methods from the Dog class is now effortless, with no need to concern ourselves about the prototype.

```
const buki = new GuideDog("Buki", "Beagle", "Lulu");
console.log(buki.alert());    // 'Buki alerts you to danger.'
console.log(buki.bark());    // 'Buki says woof!'
console.log(buki.sleep());    // 'Buki is sleeping!'
```

Of course, we can print out the prototype to observe how things are set up.

```
console.log(buki.__proto__);    // -> The prototype of buki
console.log(buki.__proto__.__proto__);    // -> The prototype of the Dog class
console.log(buki.__proto__.__proto__.__proto__);    // -> The prototype of the base class
```

## 5. `__proto__` vs. `prototype`
We've established the prototype value of an object using the `__proto__` keyword and added methods to the prototype of a constructor function. But what distinguishes them? In essence, the `prototype` is a property of a constructor function, defining properties and methods inherited by objects created from it. On the contrary, objects lack a direct `prototype` property. Instead, they possess an internal link to their prototype named `__proto__`. While it's entirely acceptable to directly manipulate the `prototype` property of a constructor function, the same approach doesn't apply to objects. For objects, built-in methods exist to facilitate better interaction with their prototype, a topic we'll delve into in the next section.
## 6. Useful Prototype Methods
A couple of useful methods are available to help us interact with objects and prototypes in a standardized and more efficient manner.
### 6.1. Object.create()
We can utilize `Object.create()` to generate an empty object and establish its prototype by passing in another object.

```
const person = {
  isHuman: true,
  greet() {
    return `Hi there!`
  }
}

const john = Object.create(person);
console.log(john);    // {}
```

When we print out the prototype of 'john,' we'll indeed find that it is the 'person' object.

```
console.log(john.__proto__);    // -> The person object
```

### 6.2. Object.getPrototypeOf()
We utilized `__proto__` to access the prototype of an object. While it works, this method is not recommended since `__proto__` serves as an internal link to an object's prototype. Instead, it's more advisable to use the commonly employed `Object.getPrototypeOf()`.

```
console.log(Object.getPrototypeOf(john));    // -> The person object
```

### 6.3. Object.setPrototypeOf()
For the same reason, it's not advisable to set the prototype of an object using the `__proto__` keyword, it is recommended to use `Object.setPrototypeOf()` instead.

```
const employee = {
  role: "Software Engineer",
  work() {
    return `I'm debugging!`;
  }
}

Object.setPrototypeOf(john, employee);
```

We've recently assigned the prototype of 'john' to a new 'employee' object. Consequently, whenever we access its prototype, we'll retrieve the 'employee' object rather than the 'person'.

```
console.log(Object.getPrototypeOf(john));    // -> The employee object
```

### 6.4. isPrototypeOf()
This method comes in handy when we need to determine whether an object is the prototype of another object.

```
console.log(person.isPrototypeOf(john));    // false
```

We're verifying if the 'person' object serves as the prototype for 'john', and as anticipated, the boolean 'false' is returned. This aligns with our recent update to the prototype of 'john' to 'employee'.

```
console.log(employee.isPrototypeOf(john));    // true
```

## Key Takeaways
- Every time an instance is created using the `new` keyword, this is what takes place under the hood:
    - Creates an empty object
    - Binds the this keyword to that object
    - Implicitly returns the object - return `this`
    - Creates a link to the object’s prototype
- The prototype is where shared properties and methods are stored, enabling multiple instances to access them without the need to create individual copies.
- The prototype chain is a mechanism that enables JavaScript to search for properties and methods within nested prototype objects.
- By default, class methods are automatically added to the class prototype. Inheritance further establishes a prototype link from the derived class to the inherited one, thereby forming a cohesive prototype chain.
- `__proto__` is an internal link pointing to an object's prototype, and direct interaction with it is not recommended. In contrast, the `prototype` is a property in constructor functions that enables the direct addition of properties or methods to their prototypes.
- Here are some useful methods for working with objects and prototypes: `Object.create()`, `Object.getPrototypeOf()`, `Object.setPrototypeOf()`, and `isPrototypeOf()`.
## Read more
- [Object prototypes - MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
- [Object - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
