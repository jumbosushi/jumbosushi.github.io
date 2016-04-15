---
layout: post
title: "Introduction to Design Pattern in JavaScript"
published: true
---

### Hey there :smile: !

If you've been coding for a while, you might have heard about this thing called **"Design Pattern"**. But what the heck are they? Why are they useful? Should you learn them?
As another JavaScript novice like yourself, I just learned about this concept recently. I found it really cool so I wanted to share with you!

This article is aimed toward programmers who have some experience with JavaScript, maybe wrote an app or two, but never thought about how to design their code. Even if you are a novice programmer, this article should entertain you and give you a quick refresher on one of the most common design patterns - The Revealing Module Pattern.

It would be helpful to have a basic understanding of Object Oriented Programming and closures. If you are unsure of what they are, I found the following articles by JavaScript.isSexy to be very helpful and ... well, sexy.

- ["OOP In JavaScript: What You NEED to Know"](http://javascriptissexy.com/oop-in-javascript-what-you-need-to-know/)
- [Understand JavaScript Closures With Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)

### But first, what is a design pattern?

![wut is this](http://img.pandawhale.com/post-32023-what-is-this-are-you-trying-to-UN60.gif)

Nope.

According to the author of ["Learning JavaScript Design Patterns"](https://addyosmani.com/resources/essentialjsdesignpatterns/book/), Design Pattern is *"a reusable solution that can be applied to commonly occurring problems in software design - in our case - in writing JavaScript web applications."*

In other words, design patterns provide a set ways to solve common problems, which is super helpful if you are new to JavaScript like me since it let me focus on building cool apps instead of focusing on how to structure my code.

### Do I need to know design pattern?

![Yup](https://cdn0.vox-cdn.com/imported_assets/2294960/yup.gif)

Before we get into to the meat and potato of this article, I know what you're thinking. **"C'mon Atsushi ¯\_(ツ)_/¯, I don't NEED to learn any design patterns. My code works!** Great shirt btw" Thanks, and you are not wrong. I know your code is utterly beautiful as of now.

But think of it like this. Imagine that you are trying to build a house. You could build it by putting together woods based on how you think a house should look like, and I know you are smart so it would probably work. However, you might find it hard to change part of the house after a while as many things are depended on each other unintentionally.

Instead, let's say your architect friend Joe comes along to help you build the house. Joes's a nice guy, so he draws a planned out diagram on how the house should be build based on his experience. Once the house is build, you would find it easy to manipulate part of the house since you have understanding of how changing one part affect others.

![Two Houses](http://i.imgur.com/BnvyFnp.jpg)

The planning sheet that Joe made is exactly what design pattern is! It gives you a roadmap to build a robust software that is easy to manipulate and scale for everyone!

Beside from the fact you can knock your friend's socks off by saying you know JavaScript design pattern, knowing design patterns makes you a better programmer! It allows you to:

- Design new applications quickly and efficiently
- Follow the solution proven right by programmers before us
- Understand the structure of other programs
- Communicate more expressively with other programmers.

To sum it up it is almost like a ....

![WoaRIGHT!?](http://s.quickmeme.com/img/69/6976f2093556822ff4f333b667e636b1ee7702ac9b1548e5a85dcf9d26121c34.jpg)


### Which pattern should I learn?

Awesome! Learning Design pattern sounds like a great deal eh?
But now the questions is, **which one should you learn???**

There are lots of design patterns out there. Important thing to keep in mind is that *there is no design pattern is one size fits all.*
With that being said, there are some patterns that are applicable to many cases.

Please allow me to introduce - **Module Design Pattern!**

![tada](https://i23.photobucket.com/albums/b381/dancer_chique17/tada.gif)

Module design pattern is one of the most common JavaScript design pattern used, and one of the easiest to use!
The main idea is that **it provide JavaScript classes an ability to have private variables and private methods!**

Why are private variables and methods good? These StackOverflow answers gives a great explanation than I ever could!

- [Why do we need private variables?](https://programmers.stackexchange.com/questions/143736/why-do-we-need-private-variables)
- [Why “private” methods in the object oriented?](https://stackoverflow.com/questions/2620699/why-private-methods-in-the-object-oriented)

To summarize above answers:
- Having private variables protect them from being manipulated outside of its class unintentionally.
- Private methods keep the functions that will only be used inside the class, hence hide what the world out of the class doesn't need to see.
- Less clutter the global namespace

But enough talking! Let's dive into the code.
Let's say we're making a program that keeps track of how many people are in the movie theater!

```JavaScript
var pplCounterModule = (function() {

  // Private Variable
  var count = 0;

  // Public Methods
  return {
    // Add num or 1 to counter if no param is specified
    addPerson: function(num) {
      if (num == undefined)
        count++;
      else
        count += num;
    },

    // Remove num or 1 to counter if no param is specified
    removePerson: function(num) {
      if (num == undefined)
        count--;
      else
        count -= num;
    },

    returnCount: function() {
      return count;
    }
  };
})();

pplCounterModule.returnCount()  //  0
pplCounterModule.addPerson(6)   // +6
pplCounterModule.removePerson() // -1
pplCounterModule.returnCount()  //  5
```


Pretty cool right?

As you can see, "count" variable is kept inside pplCounterModule class, so the outside world can't access it or manipulate it. Because you know that the only way to manipulate the variable is through its public methods, it'll be super easy to debug!

Guess. What. You now a design patternin JavaScript! Congrats :tada: :tada: :tada:!
![WOAAA](https://static1.squarespace.com/static/560d5034e4b0db2bbb878258/t/5651312ae4b08b846b819e13/1448161583756/Amazed.gif)

Now that we're on a roll, let's give it a go at another pattern - **The Revealing Module Pattern**

As you might have been able to tell from the name, it is very similar to Module Design Pattern except one small part - everything is defined privately!

```JavaScript
var pplCounterModule = (function() {

  // Private Variable
  var count = 0;

  // Addd num or 1 to counter if no param is specified
  function addPerson(num) {
    if (num == undefined)
      count++;
    else
      count += num;
  }

  // Remove num or 1 to counter if no param is specified
  function removePerson(num) {
    if (num == undefined)
      count--;
    else
      count -= num;
  }

  function returnCount() {
    return count;
  }

  // Public Methods
  return {
    add: addPerson,
    remove: removePerson,
    current: returnCount
  };
})();

pplCounterModule.current() //  0
pplCounterModule.add(6)    // +6
pplCounterModule.remove()  // -1
pplCounterModule.current() //  5
```

Notice the difference?

This one gives couple extra benefits:
- Cleaner code
- The public methods name can be simple
- Easier to maintain which method is public or not

Aaaand that's a wrap!
You can pat yourself on the back, and can now tell your friends that you know TWO Design Patterns!

![yas](https://media.giphy.com/media/WKdPOVCG5LPaM/giphy.gif)


There are many other cool patterns out there.

["Learning JavaScript Design Patterns" by Addy Osmani](https://addyosmani.com/resources/essentialjsdesignpatterns/book/) is a great resource if you want to continue learning other patterns!

Till next time, chao!
