
## Javascript Scope & `this`

To effectively use Javascript, it's important to understand the concept of "scope". In Javascript, [scope](https://en.wikipedia.org/wiki/Scope_(computer_science)) refers to the context in which a piece of code is operating. Your scope determines which parts of your code are accessible at any given time.

## Global Scope
Code outside of any function or declaration is considered to be in the "global scope". This means it's accessible from anywhere in our code. New programmers generally start by placing everything in the global scope, but this is a bad practice that should be avoided whenever possible. Placing variables in the global scope can result in ["collisions"](https://en.wikipedia.org/wiki/Collision_(computer_science)), where multiple variables react unexpectedly and break our code. We associate the global scope on webpages with the `window` object. Any functions, variables, or objects defined in the global scope are members of the `window` object.[^1]

## Function  & Lexical Scope
Javascript employs [function scope]() when determining what's available to particular scopes. This means each object creates a new scope for itself, and any variables declared within that object are accessible through its own scope. We also use [lexical scoping](https://en.wikipedia.org/wiki/Scope_%28computer_science%29#Lexical_scope_vs._dynamic_scope), which means scopes are nested and parent scopes are accessible to children (but not the other way around). In lexical scoping, we define variables using an "inside-out" approach: first we check our immediate scope, then its first parent, then subsequent parents, until we find what we're looking for. This can feel overwhelming to understand at first, but it's pretty simple in practice. Let's take a look:
```javascript
// This is in global scope, so we can access it anywhere.
var myName = "Alex";

function sayAnyName(name) {
  console.log(name);
}

function sayAnotherName() {
  // This is in this function's local scope, so we can only access
  // it from within this function.
  var myName = "Jane";
  sayAnyName(myName);
}

//This looks for myName in the global scope
sayAnyName(myName);  // => "Alex"

//This looks for myName in its local scope, finds it, and returns that value.
sayAnotherName(); // => "Jane"
```

## Immediately Invoked Function Expressions (IIFE's)
We can utilize this concept of scoping to keep our code compartmentalized and reduce ["collision"](https://en.wikipedia.org/wiki/Collision_(computer_science)), where multiple scripts interact unexpectedly and break our code. You'll often see entire scripts wrapped in a function expression to protect their contents. We refer to this as an [Immediately-Invoked Function Expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), or IIFE ("iffy"). We'll talk more about these in a future lesson, but here's a simple example of what you might see in production:
```javascript
(function() {

    var name = "Alex";

    function sayMyName() {
      console.log(name);
    }

})();
```

You can read a lot more about scoping in Javascript at [this great blog post](http://toddmotto.com/everything-you-wanted-to-know-about-javascript-scope/) by [Todd Motto](https://github.com/toddmotto).

## "This" & That
Javascript provides us with a powerful keyword you'll see often in your code: `this`. `This` refers to the "owner" of an object, and is used as a self-reference to the current scope for accessing content. It's a small word, but `this` is very important when creating reusable code! Let's look at a simple example of `this` in action:
```javascript
function person(name) {
  this.name = name;
  this.sayName = function() {
    console.log(this.name);
  }
  return this;
}

var steve = new person("Steve");
var kyle = new person("Kyle");

steve.sayName();  // => "Steve"
kyle.sayName(); // => "Kyle"
```

As you can see, `this` is useful for accessing new objects without having to repeat our code. It lets object instances reference themselves after creation, which means we can give variables new values and still have access to them.

The value of `this` changes depending on the scope it's used within. This can be confusing for new developers, but there's a simple rule we can fall back on to help keep the value of `this` in mind at any time: "*`this` refers to the object that invoked our current environment*". If we're in global scope, `this` will refer to the `window` object. If we're inside an object's scope, `this` will refer to that object. Here are some quick examples:
```javascript
console.log(this);  //=> window

var newScope = {
  gimmeThis: function() {
    console.log(this);
  }
}

newScope.gimmeThis();  //=> newScope
```

A common problem we find is that iterator functions create new scopes and prevent us from modifying their environments. We can get around this by holding a reference to the object we want in a custom variable. Here's an example of the problem in action:

```javascript
var myNumbers = {
  numbers: [1, 2, 3, 4],
  addSome: function() {
    // "this" here refers to the myNumbers object, so "this.numbers" === [1, 2, 3, 4]
    this.numbers.forEach(function(num) {
      num += 4;
      // Here, "this" refers to the anonymous function invoked by .each(), so "this.numbers" === undefined
      this.numbers.push(num);
    });
    // Now that we're out of our anonymous function scope, "this" refers to myNumbers again.
    return this.numbers;
  }
};

myNumbers.addSome(); // => This will fail on "this.numbers.push()", because "this.numbers" is undefined.
```

This problem will come up frequently when implementing new objects throughout your code. The most common solution is to use a variable to store the value of `this` you want to target. Here's our `myNumbers()` example again. Notice how we store `this` in a variable that we can reference from anywhere else:

```javascript
var myNumbers = {
  numbers: [1, 2, 3, 4],
  addSome: function() {
    // we'll store our current "this" in the "self " variable
    var self = this;
    self.numbers.forEach(function(num) {
      num += 4;
      // because "self" refers to the outside "this", this line will perform as expected
      self.numbers.push(num);
    });
    return self.numbers;
  }
};

myNumbers.addSome(); // => [1, 2, 3, 4, 5, 6, 7, 8]
```

Dealing with unexpected scoping problems can get frustrating, but it's good practice to know which scope you're working in at any given point in your code. We'll discuss some alternative options for defining the value of "this" in future lessons.

[^1]:https://developer.mozilla.org/en-US/docs/Web/API/Window
