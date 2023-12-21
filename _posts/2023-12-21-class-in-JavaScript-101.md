---
title: Class in JavaScript 101
date: 2023-12-21
---
## Outline
1. What is class? And why we need it?
2. Constructor
3. Instance Methods
4. Inheritance Basics
5. Static Properties

##

Whenever the term 'class' arises in programming, my thoughts instinctively turn to the Object-Oriented Programming paradigm. In JavaScript, the 'class' keyword empowers us to effortlessly model real-life objects, making them tangible entities within our code.
## 1. What is class? And why we need it?
Imagine we're incorporating a car into our program. How do we articulate its presence in the programming realm? One of the initial thoughts that occurs to me is employing an object. Through this, we can define not only its properties but also its actions or behaviors, effectively emulating a real-world car. The representation might take a form similar to this:

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520car%2520%253D%2520%257B%250A%2520%2520color%253A%2520%2522red%2522%252C%250A%2520%2520brand%253A%2520%2522Tesla%2522%252C%250A%2520%2520model%253A%2520%2522Model%2520S%2522%252C%250A%2520%2520isElectric%253A%2520true%252C%250A%2520%2520price%253A%252074990%252C%250A%2520%2520start%253A%2520function%2520%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2522Starting%2520Tesla%2520Model%2520S%2520.%2520.%2520.%2522%29%253B%250A%2520%2520%257D%252C%250A%2520%2520speedUp%253A%2520function%28mph%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Speeding%2520up%2520to%2520.%2520.%2520.%2520%2524%257Bmph%257D%2520mph%2560%29%253B%250A%2520%2520%257D%250A%257D"
  style="width: 777px; height: 500px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

While the current representation is somewhat simplistic, the concept is clear. Fortunately, we currently possess only one car, but envision the excitement if a lottery win grants us the opportunity to expand our collection. Each of us likely harbors unique plans for managing such a windfall. However, consider the process of acquiring additional cars to diversify our collection. In the real world, this is a straightforward task—visit a showroom, make a selection. In the realm of programming, it's equally manageable; creating new objects allows us to describe each new addition. Yet, with repeated acquisitions, a challenge arises. We find ourselves introducing a new object with nearly identical information each time we acquire a car. Here's a glimpse of what our car collection may transform into after these successive additions:

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520tesla%2520%253D%2520%257B%250A%2520%2520color%253A%2520%2522red%2522%252C%250A%2520%2520brand%253A%2520%2522Tesla%2522%252C%250A%2520%2520model%253A%2520%2522Model%2520S%2522%252C%250A%2520%2520isElectric%253A%2520true%252C%250A%2520%2520price%253A%252074990%252C%250A%2520%2520start%253A%2520function%2520%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2522Starting%2520Tesla%2520Model%2520S%2520.%2520.%2520.%2522%29%253B%250A%2520%2520%257D%252C%250A%2520%2520speedUp%253A%2520function%28mph%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Speeding%2520up%2520to%2520.%2520.%2520.%2520%2524%257Bmph%257D%2520mph%2560%29%253B%250A%2520%2520%257D%250A%257D%253B%250A%250Aconst%2520volvo%2520%253D%2520%257B%250A%2520%2520color%253A%2520%2522blue%2522%252C%250A%2520%2520brand%253A%2520%2522Volvo%2522%252C%250A%2520%2520model%253A%2520%2522XC90%2522%252C%250A%2520%2520isElectric%253A%2520false%252C%250A%2520%2520price%253A%2520210000%252C%250A%2520%2520start%253A%2520function%2520%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2522Starting%2520Volvo%2520XC90%2520.%2520.%2520.%2522%29%253B%250A%2520%2520%257D%252C%250A%2520%2520speedUp%253A%2520function%28mph%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Speeding%2520up%2520to%2520.%2520.%2520.%2520%2524%257Bmph%257D%2520mph%2560%29%253B%250A%2520%2520%257D%250A%257D%253B%250A%250Aconst%2520ford%2520%253D%2520%257B%250A%2520%2520color%253A%2520%2522red%2522%252C%250A%2520%2520brand%253A%2520%2522Ford%2522%252C%250A%2520%2520model%253A%2520%2522Explorer%2522%252C%250A%2520%2520isElectric%253A%2520false%252C%250A%2520%2520price%253A%2520102000%252C%250A%2520%2520start%253A%2520function%2520%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2522Starting%2520Ford%2520Explorer%2520.%2520.%2520.%2522%29%253B%250A%2520%2520%257D%252C%250A%2520%2520speedUp%253A%2520function%28mph%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Speeding%2520up%2520to%2520.%2520.%2520.%2520%2524%257Bmph%257D%2520mph%2560%29%253B%250A%2520%2520%257D%250A%257D%253B%250A%250Aconst%2520toyota%2520%253D%2520%257B%250A%2520%2520...%250A%257D"
  style="width: 777px; height: 1464px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Tasked with avoiding the tedium and upholding the DRY (Don't Repeat Yourself) principle in programming, we find a solution in the versatile class. Acting as an object in JavaScript, a class efficiently consolidates all pertinent information, ensuring accessibility and ease of manipulation. To declare a class in JavaScript, employ the class keyword, and outline its properties and methods, as exemplified below.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520Car%2520%257B%250A%2520%2520constructor%28%29%2520%257B%250A%2520%2520%2520%2520this.color%2520%253D%2520%2522red%2522%253B%250A%2520%2520%2520%2520this.brand%2520%253D%2520%2522Tesla%2522%253B%250A%2520%2520%2520%2520this.model%2520%253D%2520%2522Model%2520S%2522%253B%250A%2520%2520%2520%2520this.isElectric%2520%253D%2520true%253B%250A%2520%2520%2520%2520this.price%2520%253D%252074990%253B%2509%250A%2520%2520%257D%250A%2520%2520start%2520%253D%2520function%2520%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Starting%2520Tesla%2520Model%2520S%2520.%2520.%2520.%2560%29%253B%250A%2520%2520%257D%253B%250A%2520%2520speedUp%2520%253D%2520function%28mph%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Speeding%2520up%2520to%2520.%2520.%2520.%2520%2524%257Bmph%257D%2520mph%2560%29%253B%250A%2520%2520%257D%253B%250A%257D"
  style="width: 777px; height: 552px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Temporarily setting aside the intricacies of the constructor and the `this`[^this_note] keyword, the Car class bears a striking resemblance to the tesla object we crafted earlier. Essentially, a class serves as a template for molding the desired object. To actively engage with a tangible instance of this 'blueprint' and bring a 'real' object to life, we employ the `new`[^new_note] keyword. Creating instances of the Car class promises an enjoyable exploration – let's dive in!

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520tesla%2520%253D%2520new%2520Car%28%29%253B%250Aconst%2520volvo%2520%253D%2520new%2520Car%28%29%253B%250Aconst%2520toyota%2520%253D%2520new%2520Car%28%29%253B"
  style="width: 777px; height: 187px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

After creating three instances from the `Car` class, a quick printout reveals that all of them share the same static data inherited from the class boilerplate—the details of the `tesla` object. This limitation diminishes its practicality. Nevertheless, we can elevate its usefulness significantly by leveraging the `constructor` function to fashion instances with dynamic, customizable data.
## 2. Constructor
The constructor serves as a fundamental function that activates whenever a new instance is created. Put simply, it acts as the entry point when we invoke a class with the `new` keyword. At this juncture, we can 'register' properties specific to an instance of the class using the `this` keyword. These properties become attached to a particular instance upon creation, earning the designation of 'instance properties.' Moreover, given that the constructor functions as a standard function, we have the flexibility to pass in parameters. This versatility becomes intriguing as it allows us to generate multiple instances with the same structure but distinct information. Let's refine the `Car` class by incorporating this enhancement.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520Car%2520%257B%250A%2520%2520constructor%28color%252C%2520brand%252C%2520model%252C%2520isElectric%252C%2520price%29%2520%257B%250A%2520%2520%2520%2520this.color%2520%253D%2520color%253B%250A%2520%2520%2520%2520this.brand%2520%253D%2520brand%253B%250A%2520%2520%2520%2520this.model%2520%253D%2520model%253B%250A%2520%2520%2520%2520this.isElectric%2520%253D%2520isElectric%253B%250A%2520%2520%2520%2520this.price%2520%253D%2520price%253B%2509%250A%2520%2520%257D%250A%2520%2520start%2520%253D%2520function%2520%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Starting%2520%2524%257Bthis.model%257D%2520.%2520.%2520.%2560%29%253B%250A%2520%2520%257D%253B%250A%2520%2520speedUp%2520%253D%2520function%28mph%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560Speeding%2520up%2520to%2520.%2520.%2520.%2520%2524%257Bmph%257D%2520mph%2560%29%253B%250A%2520%2520%257D%253B%250A%257D"
  style="width: 777px; height: 552px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Now, by effortlessly supplying this information during the instantiation of a new instance, voilà, our car collection accurately mirrors our actual possessions.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520tesla%2520%253D%2520new%2520Car%28%2522red%2522%252C%2520%2522Tesla%2522%252C%2520%2522Model%2520S%2522%252C%2520true%252C%252074990%29%253B%250Aconsole.log%28tesla.color%29%253B%2520%2520%2520%2520%252F%252F%2520%2522red%2522%250A%250Aconst%2520volvo%2520%253D%2520new%2520Car%28%2522blue%2522%252C%2520%2522Volvo%2522%252C%2520%2522XC90%2522%252C%2520false%252C%2520210000%29%253B%250Aconsole.log%28volvo.brand%29%253B%2520%2520%2520%2520%252F%252F%2520%2522Volvo%2522%250A%250Aconst%2520toyota%2520%253D%2520new%2520Car%28%2522black%2522%252C%2520%2522Toyota%2522%252C%2520%2522Camry%2522%252C%2520false%252C%252026420%29%253B%250Aconsole.log%28toyota.price%29%253B%2520%2520%2520%2520%252F%252F%252026420"
  style="width: 777px; height: 424px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Through this approach, we've effectively addressed the drawbacks of using literal objects to flexibly model real-world entities within the realm of OOP.
## 3. Instance Methods
If you've experimented with the `Car` class mentioned earlier, you might have noticed that invoking the `start` and `speedUp` methods via an instance follows the same syntax as accessing properties. Hence, these methods are commonly known as 'instance methods,' distinguishing them from 'static methods'—a topic we'll explore later. The process of defining these methods doesn't deviate from creating regular functions. To solidify this concept, let's enrich the `Car` class by introducing some new instance methods and observing their functionality.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520Car%2520%257B%250A%2520%2520constructor%28color%252C%2520brand%252C%2520model%252C%2520isElectric%252C%2520price%29%2520%257B%250A%2520%2520%2520%2520this.color%2520%253D%2520color%253B%250A%2520%2520%2520%2520this.brand%2520%253D%2520brand%253B%250A%2520%2520%2520%2520this.model%2520%253D%2520model%253B%250A%2520%2520%2520%2520this.isElectric%2520%253D%2520isElectric%253B%250A%2520%2520%2520%2520this.price%2520%253D%2520price%253B%250A%2520%2520%257D%250A%2520%2520maintain%2520%253D%2520function%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560%2524%257Bthis.model%257D%2520is%2520currently%2520under%2520maintainance%2560%29%253B%250A%2520%2520%257D%253B%250A%2520%2520changeColor%2520%253D%2520function%28newColor%29%2520%257B%250A%2520%2520%2520%2520this.color%2520%253D%2520newColor%253B%250A%2520%2520%2520%2520console.log%28%2560%2524%257Bthis.model%257D%2520turned%2520into%2520%2524%257BnewColor%257D%2560%29%253B%250A%2520%2520%257D%250A%257D"
  style="width: 777px; height: 620px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Take note that we can effortlessly access any properties from a method by using the `this` keyword. Recently, we introduced two new instance methods, `maintain` and `changeColor`, to the `Car` class. Invoking either of them mirrors the syntax of accessing a property.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520bmw%2520%253D%2520new%2520Car%28%2522grey%2522%252C%2520%2522BMW%2522%252C%2520%2522BMW%2520X3%2522%252C%2520false%252C%252085500%29%253B%250Abmw.changeColor%28%2522white%2522%29%253B%2520%2520%2520%2520%252F%252F%2520%2522BMW%2520X3%2520turned%2520into%2520white%2522"
  style="width: 777px; height: 157px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

## 4. Inheritance Basics
Another noteworthy advantage of employing the `class` is its support for inheritance. Specifically, it allows us to use a boilerplate class as a blueprint to create new classes that share many similarities with the inherited (base) class. This eliminates the need to redundantly redefine common properties and methods. Instead, we can focus on adding private information unique to the derived classes. Consider a scenario where we aim to manage information for all employees in a company, spanning different levels such as CEO, Manager, and Employee. While the instinctive approach might involve creating separate classes for each level, a more streamlined solution emerges when we recognize that these classes share common information like "name," "address," "start_date," "salary," and more. In response, we can create a base class named "Person," encapsulating only shared information from the CEO, Manager, and Employee classes. Subsequently, other classes can inherit from the "Person" class while adding their own specific properties. The "Person" class may take a form similar to this:

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520Person%2520%257B%250A%2520%2520constructor%28name%252C%2520address%252C%2520start_date%252C%2520salary%29%2520%257B%250A%2520%2520%2520%2520this.name%2520%253D%2520name%253B%250A%2520%2520%2520%2520this.address%2520%253D%2520address%253B%250A%2520%2520%2520%2520this.start_date%2520%253D%2520start_date%253B%250A%2520%2520%2520%2520this.salary%2520%253D%2520salary%253B%250A%2520%2520%257D%250A%2520%2520greet%2520%253D%2520function%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2522Hello%2520world%21%2522%29%253B%250A%2520%2520%257D%250A%257D"
  style="width: 777px; height: 430px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

To establish inheritance for the 'CEO,' 'Manager,' and 'Employee' classes from the 'Person' class, we utilize the `extends` keyword. With inheritance, the derived class automatically incorporates all properties from the base class, requiring it only to specify its unique properties in the constructor. Below is the definition for the 'CEO' class (similarly applicable to the 'Manager' and 'Employee' classes).

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520CEO%2520extends%2520Person%2520%257B%250A%2520%2520constructor%28year_of_exp%29%2520%257B%250A%2520%2520%2520%2520this.yoe%2520%253D%2520year_of_exp%253B%250A%2520%2520%257D%250A%2520%2520assign%2520%253D%2520function%28task%252C%2520employee%252C%2520time%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560This%2520%2524%257Btask%257D%2520task%2520is%2520assigned%2520to%2520%2524%257Bemployee%257D%2520at%2520%2524%257Btime%257D%2560%29%253B%250A%2520%2520%257D%250A%257D"
  style="width: 777px; height: 366px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

This seems satisfactory! However, an issue arises when we attempt to instantiate a CEO instance:

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520ceo%2520%253D%2520new%2520CEO%286%29%253B"
  style="width: 777px; height: 126px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

An error occurs since JavaScript is unaware of other properties from the `Person` class. Fortunately, this can be easily addressed by employing the `super` method to initialize the base class before adding any private properties in the constructor.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520CEO%2520extends%2520Person%2520%257B%250A%2520%2520constructor%28name%252C%2520address%252C%2520start_date%252C%2520salary%252C%2520year_of_exp%29%2520%257B%250A%2520%2520%2520%2520super%28name%252C%2520address%252C%2520start_date%252C%2520salary%29%253B%250A%2520%2520%2520%2520this.yoe%2520%253D%2520year_of_exp%253B%250A%2520%2520%257D%250A%2520%2520assign%2520%253D%2520function%28task%252C%2520employee%252C%2520time%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560This%2520%2524%257Btask%257D%2520task%2520is%2520assigned%2520to%2520%2524%257Bemployee%257D%2520at%2520%2524%257Btime%257D%2560%29%253B%250A%2520%2520%257D%250A%257D"
  style="width: 777px; height: 425px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

With this update, the `CEO` class is expected to work as intended.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=const%2520ceo%2520%253D%2520new%2520CEO%28%2522Hanny%2522%252C%2520%252212A%2520Sahara%2520St%2522%252C%2520%252220%252F02%252F2002%2522%252C%2520104000%252C%25206%29%253B%250Aconsole.log%28ceo.name%29%253B%2520%2520%2520%2520%252F%252F%2520%2522Hanny%2522%250Aconsole.log%28ceo.yoe%29%253B%2520%2520%2520%2520%252F%252F%25206%250Aceo.greet%28%29%253B%2520%2520%2520%2520%252F%252F%2520%2522Hello%2520world%21%2522%250Aceo.assign%28%2522deployment%2522%252C%2520%2522Arol%2522%252C%2520%25223pm%2522%29%253B%2520%2520%2520%2520%252F%252F%2520%2522The%2520deployment%2520task%2520is%2520assigned%2520to%2520Arol%2520at%25203pm%2522"
  style="width: 777px; height: 321px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Take note that we have the capability to override any properties or methods of the parent class in the derived class. Conceptually, when a method is invoked from an instance, the system first checks if the instance itself possesses that feature. If it does, JavaScript will execute that feature from the owner. Alternatively, if the instance lacks that feature, JavaScript will search for it among all its parent classes and invoke the closest one it finds. Let's delve into how this mechanism operates!

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520CEO%2520extends%2520Person%2520%257B%250A%2520%2520constructor%28name%252C%2520address%252C%2520start_date%252C%2520salary%252C%2520year_of_exp%29%2520%257B%250A%2520%2520%2520%2520super%28name%252C%2520address%252C%2520start_date%252C%2520salary%29%253B%250A%2520%2520%2520%2520this.yoe%2520%253D%2520year_of_exp%253B%250A%2520%2520%257D%250A%2520%2520assign%2520%253D%2520function%28task%252C%2520employee%252C%2520time%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2560This%2520%2524%257Btask%257D%2520task%2520is%2520assigned%2520to%2520%2524%257Bemployee%257D%2520at%2520%2524%257Btime%257D%2560%29%253B%250A%2520%2520%257D%250A%2520%2520greet%2520%253D%2520function%28%29%2520%257B%250A%2520%2520%2520%2520console.log%28%2522Hello%2520world%2520from%2520CEO%2520class%21%2522%29%253B%250A%2520%2520%257D%250A%257D%250A%250Aconst%2520ceo%2520%253D%2520new%2520CEO%28%2522Hanny%2522%252C%2520%252212A%2520Sahara%2520St%2522%252C%2520%252220%252F02%252F2002%2522%252C%2520104000%252C%25206%29%253B%250Aceo.greet%28%29%253B%2520%2520%2520%2520%252F%252F%2520%2522Hello%2520world%2520from%2520CEO%2520class%21%2522"
  style="width: 777px; height: 660px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

## 5. Static Properties
Up until now, our focus has been on declaring instance properties, accessible only through a specific instance using the dot (`.`) syntax. This approach is effective and logical since the properties are closely tied to the invoked instance, articulating precisely what we intend to do with it. However, there are scenarios where creating an instance to access a class's properties and methods is unnecessary. This is where static properties prove useful. Consider the scenario of crafting our own `MyMath` class, housing utility properties and methods. Most often, we prefer accessing these utilities directly via the class name, as they don't delineate characteristics or attributes of a specific instance. Implementing this involves a methodology similar to what we've employed so far, with the only difference being the substitution of variable declaration keywords (`const`, `let`, and `var`) with the `static` keyword. Let's now explore the possibilities with the `MyMath` class!

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=class%2520MyMath%2520%257B%250A%2520%2520static%2520PI%2520%253D%25203.1415%253B%250A%2520%2520static%2520add%2520%253D%2520function%28a%252C%2520b%29%2520%257B%250A%2520%2520%2520%2520return%2520a%2520%252B%2520b%253B%250A%2520%2520%257D%250A%2520%2520static%2520subtract%2520%253D%2520function%28a%252C%2520b%29%2520%257B%250A%2520%2520%2520%2520return%2520a%2520-%2520b%253B%250A%2520%2520%257D%250A%2520%2520static%2520pow%2520%253D%2520function%28base%252C%2520power%29%2520%257B%250A%2520%2520%2520%2520if%28base%2520%253D%253D%253D%25200%2520%2526%2526%2520power%2520%253D%253D%253D%25200%29%2520return%253B%250A%2520%2520%2520%2520if%28power%2520%253D%253D%253D%25200%29%2520return%25201%253B%250A%2520%2520%2520%2520let%2520res%2520%253D%2520base%253B%250A%2520%2520%2520%2520for%28let%2520i%2520%253D%25202%253B%2520i%2520%253C%253D%2520power%253B%2520i%252B%252B%29%2520%257B%250A%2520%2520%2520%2520%2520%2520res%2520*%253D%2520base%253B%250A%2520%2520%2520%2520%257D%250A%2520%2520%2520%2520return%2520res%253B%250A%2520%2520%257D%250A%257D"
  style="width: 777px; height: 643px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Utilizing the functionalities of `MyMath` is straightforward.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba%28171%2C+184%2C+195%2C+1%29&t=seti&wt=none&l=auto&width=680&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=11px&ph=14px&ln=true&fl=1&fm=Hack&fs=18px&lh=169%25&si=false&es=2x&wm=false&code=console.log%28MyMath.add%283%252C%25204%29%29%253B%2520%2520%2520%2520%252F%252F%25207%250Aconsole.log%28MyMath.pow%283%252C%25202%29%29%253B%2520%2520%2520%2520%252F%252F%25209%250Aconsole.log%28MyMath.PI%29%253B%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%252F%252F%25203.1415"
  style="width: 777px; height: 187px; border:0; transform: scale(1); overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>


These exemplify the fundamental uses of `class` in JavaScript, simplifying our engagement with objects and OOP. Further exploration into the advanced functionalities of `class` will be covered in upcoming posts.

## Key Takeaways
- Employing `class` to model a general structure shared by multiple objects with similar properties minimizes the repetition of writing redundant code.
- The `constructor` function serves as the 'entry' point during instance instantiation. Utilize it to 'register' properties and methods specific to an instance, commonly referred to as 'instance properties/methods'.
- Harness the inheritance paradigm to create diverse instances sharing the same properties.
- Avoid 'attaching' properties/methods to an instance if they don't describe the instance's characteristics or if the instance doesn't inherently possess those features. Instead, opt for static properties/methods.

## Tips!
- Class names conventionally follow UpperCamelCase.
- Utilize bracket notation (`[]`) to access properties from a literal object, especially when dealing with expressions, as opposed to dot notation (`.`).
- Static methods often function as 'factory' methods, facilitating the creation of instances.
- In JavaScript, classes can be inherited at arbitrary nested levels. This differs from the concept of multi-inheritance in programming, where a class can inherit from multiple parent classes simultaneously.

*As my knowledge is still evolving, my insights may have some limitations. I welcome comments and further discussions from readers, appreciating your time and support for this piece. Thank you for engaging with the content.*

**Read more**
- [Object - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

[^this_note]: The keyword `this` pertains to the instance we're working with, not the class itself. A more detailed discussion on this topic will be featured in another post.
[^new_note]: Consider the `new` keyword as the creator of a class object, commonly known as an 'instance,' structured according to the data outlined in the class boilerplate. This differs from creating a literal object. We will explore this concept in more detail in an upcoming post.
