---
title: OOP - Newer Features in JavaScript 101
date: 2023-12-24
---
## Outline
1. Getters
2. Setters
3. Public Fields
4. Private Fields
5. Private Methods
6. Static Initialization Blocks

Since ECMAScript 2009 (ES5), JavaScript has embraced newer features that enhance the convenience and safety of working with Object-Oriented Programming (OOP) in JS.

## 1. Getters
In the realm of JavaScript classes, accessing properties and invoking methods is a straightforward endeavor. This is achieved effortlessly using dot notation (`.`), whether directly applied to a class for static properties/methods or to an instance for instance properties/methods. Let's exemplify this concept with the class provided below.
```
class Meow {
  constructor(name, breed, age) {
    this.name = name;
    this.breed = breed;
    this.age = age;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.name);    // "Bong"
const newCat = Meow.createMeow("Zyl", "Persian", 3);
console.log(newCat.age);    // 3
```
While the current approach functions well, it's often preferable to discourage direct access to a class's properties (i.e., keeping an entity's properties hidden from public access). Instead, we provide users with specific functions to retrieve the desired information. This practice allows us to incorporate additional logic before granting access—such as checking user permissions, authorizing users, performing calculations, or executing various business logics. Let's transform this idea into code using the `Meow` class above!
```
class Meow {
  constructor(name, breed, age) {
    this.name = name;
    this.breed = breed;
    this.age = age;
  }
  getName = function() {
    // Do some logics here if neccessary before returning the name
    return this.name;
  }
  getBreed = function() {
    // Do some logics here if neccessary before returning the breed
    return this.breed;
  }
  getAge = function() {
    // Do some logics here if neccessary before returning the age
    return this.age;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.getName());    // "Bong"
console.log(cat.getBreed());   // "Birman"
console.log(cat.getAge());     // 1
```
The returned results appear identical to the previous example. While, at this juncture, direct access to properties using dot notation remains possible, we aim to conceal these properties in a subsequent section. The functions `getName`, `getBreed`, and `getAge` are commonly known as 'getter' functions in programming, serving the purpose of retrieving data from the class. Recognizing this common practice, ES5+ introduces the `get` keyword to facilitate a more convenient and cleaner implementation of this approach. The provided code can be equivalently expressed using this new syntax with the `get` keyword.
```
class Meow {
  constructor(name, breed, age) {
    this.name = name;
    this.breed = breed;
    this.age = age;
  }
  get getName() {
    // Do some logics here if neccessary before returning the name
    return this.name;
  }
  get getBreed() {
    // Do some logics here if neccessary before returning the breed
    return this.breed;
  }
  get getAge() {
    // Do some logics here if neccessary before returning the age
    return this.age;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.getName);    // "Bong"
console.log(cat.getBreed);   // "Birman"
console.log(cat.getAge);     // 1
```
Once more, the outputs remain identical to the previous example. Yet, thanks to the 'get' keyword, we treat getter functions as properties, concealing the fact that they are essentially functions. The versatility of getters becomes particularly evident when creating a property that depends on other properties.
```
class Meow {
  constructor(name, breed, age) {
    this.name = name;
    this.breed = breed;
    this.age = age;
  }
  // ...
  get happyBirthday() {
    return `Happy birthday ${this.name}! You're ${this.age} years old now!`;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 2);
console.log(cat.happyBirthday);    // "Happy birthday Bong! You're 2 years old now!"
```
## 2. Setters
As individuals can freely access a class's properties, there is no way to prevent them from modifying the data.
```
const cat = new Meow("Bong", "Birman", 1);
console.log(cat.getAge());     // 1

cat.age = 2;
console.log(cat.getAge());    // 2
```
Although updating the 'age' to the number '2' works fine, what if someone attempts to change it to a negative number or a string? This could potentially damage the program and lead to errors. To mitigate this risk, we'll apply a similar concept to 'getter' functions by introducing specific functions that allow controlled updates to the data. This approach enables us to maintain the desired status of properties and establish a well-managed scenario for controlling the input people may provide. Let's implement these enhancements in the `Meow` class!
```
class Meow {
  constructor(name, breed, age) {
    this.name = name;
    this.breed = breed;
    this.age = age;
  }
  // ...
  setName = function(newName) {
    // Just doing some basic data-type checks for now
    if(typeof newName !== 'string') {
      return "Invalid input value for name!";
    }
    this.name = newName;
  }
  setBreed = function(newBreed) {
    // Just doing some basic data-type checks for now
    if(typeof newBreed !== 'string') {
      return "Invalid input value for breed!";
    }
    this.breed = newBreed;
  }
  setAge = function(newAge) {
    // Just doing some basic data-type checks for now
    if(typeof newAge !== 'number') {
      return "Invalid input value for age!";
    }
    this.age = newAge;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.getName;    // "Bong"
cat.setName("Bea");
console.log(cat.getName;    // "Bea"
cat.setName(2023);    // "Invalid input value for name!";
console.log(cat.getName;    // "Bea"
```
This not only helps us steer clear of headaches and program crashes but also introduces a syntax akin to getters. We can use this syntax with setters to streamline the implementation and better align with the newer features in JavaScript. As you might have guessed, this syntax involves the keyword `set`.
```
class Meow {
  constructor(name, breed, age) {
    this._name = name;
    this._breed = breed;
    this._age = age;
  }
  // ...
  set name(newName) {
    // Just doing some basic data-type checks for now
    if(typeof newName !== 'string') {
      return "Invalid input value for name!";
    }
    this._name = newName;
  }
  set breed(newBreed) {
    // Just doing some basic data-type checks for now
    if(typeof newBreed !== 'string') {
      return "Invalid input value for breed!";
    }
    this._breed = newBreed;
  }
  set age(newAge) {
    // Just doing some basic data-type checks for now
    if(typeof newAge !== 'number') {
      return "Invalid input value for age!";
    }
    this._age = newAge;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.getName);    // "Bong"
cat.name = "Bea";
console.log(cat.getName);    // "Bea"
cat.setName(2023);    // "Invalid input value for name!";
console.log(cat.getName);    // "Bea"
```
Much like getters, setters operate behind the scenes as functions. Yet, using them is as straightforward as reassigning a property value. However, at the moment, anyone can freely access properties and make changes without restrictions. Soon, we will explore how to address this issue.
## 3. Public Fields
Instance properties introduced outside of the `constructor` function are known as public fields.
```
class Meow {
  numOfLegs = 4;
  constructor(name, breed, age) {
    this._name = name;
    this._breed = breed;
    this._age = age;
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.numOfLegs);    // 4
```
Consider it a shared property that all instances are expected to possess. You might wonder why we need it when we can simply declare it inside the `constructor`.
```
class Meow {
  constructor(name, breed, age) {
    this._name = name;
    this._breed = breed;
    this._age = age;
    this.numOfLegs = 4;
  }
}

const cat = new Meow("Bong", "Birman", 1);
console.log(cat.numOfLegs);    // 4
```
This refactoring yields the exact same result. In most cases, we can expect both of these approaches to work fine (even though there's a subtle difference, it's beyond the scope for this 101). One of the benefits of using public fields is that it distinctly reveals the structure of a class, enabling us to intuitively keep track of all the information that a class offers.
```
class Meow {
  _name;
  _breed;
  _age;
  _numOfLegs = 4;
  constructor(name, breed, age, numOfLegs) {
    this._name = name;
    this._breed = breed;
    this._age = age;
    this._numOfLegs = numOfLegs;
  }
}
```
Upon reviewing this boilerplate, it becomes evident that the `Meow` class encompasses properties such as 'name', 'breed', 'age', and 'numOfLegs'. It's entirely acceptable to either initialize them with specific values or leave them as undefined and update them within the `constructor` function. In the upcoming section, we'll delve into another crucial use case for public fields, particularly when working with private properties.
## 4. Private Fields
Until now, the getters and setters we defined above haven't been truly effective because it's still possible to use the dot notation to access and modify property values. As mentioned earlier, in most cases, we prefer to conceal those properties and allow interaction only via getters and setters to avoid unexpected behaviors. Fortunately, ES5+ has provided a convenient method to achieve this by introducing the '#' symbol as a private protector. To designate a property as private, we need to initially introduce it as a public field and then simply attach the '#' symbol in front of those fields.
```
class Meow {
  #_name;
  #_breed;
  #_age;
  constructor(name, breed, age) {
    this.#_name = name;
    this.#_breed = breed;
    this.#_age = age;
  }
  get name() {
    // Do some logics here if neccessary before returning the name
    return this.#_name;
  }
  get breed() {
    // Do some logics here if neccessary before returning the breed
    return this.#_breed;
  }
  get age() {
    // Do some logics here if neccessary before returning the age
    return this.#_age;
  }
  set name(newName) {
    // Just doing some basic data-type checks for now
    if(typeof newName !== 'string') {
      return "Invalid input value for name!";
    }
    this.#_name = newName;
  }
  set breed(newBreed) {
    // Just doing some basic data-type checks for now
    if(typeof newBreed !== 'string') {
      return "Invalid input value for breed!";
    }
    this.#_breed = newBreed;
  }
  set age(newAge) {
    // Just doing some basic data-type checks for now
    if(typeof newAge !== 'number') {
      return "Invalid input value for age!";
    }
    this.#_age = newAge;
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1.5);
console.log(cat.name);    // "Bong"
cat.name = "Bea";
console.log(cat.name);    // "Bea"
console.log(cat.#_name);    // ERROR -> because #_name is private
```
Although all the properties are now private, it's worth noting that we can still access them from the functions inside the `Meow` class. This mechanism renders those properties invisible from the outside but retains their availability internally.
## 5. Private Methods
Private methods essentially serve the same purpose as private properties, restricting their access to only within the class. To denote a method as private, we simply prefix the method's name with the '#' symbol.
```
class Meow {
  #_name;
  #_breed;
  #_age;
  constructor(name, breed, age) {
    this.#_name = name;
    this.#_breed = breed;
    this.#_age = age;
  }
  #secretNumber() {
    return 42;
  }
  get secretNumber() {
    return this.#secretNumber();
  }
  static createMeow = function(name, breed, age = 0) {
    return new Meow(name, breed, age);
  }
}

const cat = new Meow("Bong", "Birman", 1.5);
console.log(cat.name);    // "Bong"
console.log(cat.#secretNumber());    // ERROR
console.log(cat.secretNumber);    // 42
```
Private features prove to be quite handy, significantly improving our capacity to secure the application.
## 6. Static Initialization Blocks
Introducing a novel concept that allows us to execute a block of code just once—specifically when the class is first invoked with the `new` keyword. By prefixing this block with the `static` keyword, everything inside is treated as the initialization block.
```
class Meow {
  static initializeMe;
  static {
    this.initializeMe = 33;
  }
}

const cat = new Meow("Bong", "Birman", 1.5);
console.log(Meow.initializeMe);    33
```
Please note that the `this` keyword inside the `static` block refers to the class itself, not an instance. In this example, the advantages of using the static initialization block over pure static properties might not be immediately apparent. However, the static initialization block allows for the inclusion of more complex logic, such as connecting to a database or loading resources.
## Key takeaways
- Utilize getters and setters for effective management of user interaction with properties.
- Explore the use of public fields to distinctly outline the blueprint of a class.
- Safeguard (sensitive) properties/methods by hiding them from the public, allowing access exclusively through getters and setters.
- Acquaint yourself with the static initialization block for tasks like preparing initial data or executing actions only once during initialization.

**Read more:**
- [Get - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)
- [Set - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)
- [static - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)
