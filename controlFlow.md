### What is "control flow?"
Control flow describes the order that statements of a program are executed in. Mastering control flow not only makes software more efficient but can also make it easier to maintain and debug.

### Core control flow tools

#### Conditionals
Conditionals create "branches" in code where a single path of the branch is followed depending on a condition (boolean expression). If/else statements are the most well-known form of conditional statements.

```js
var age = 49;

// If the conditional statement (age less than 12 in this case) is true, the code
// inside of the curly braces will be executed; otherwise the code in the else
// will be. In this case, there are three different potential "branches" but only
// one of the three will run.
if (age < 12) {
   console.log("You're a child.");
} else if (age < 20) {
   console.log("You're a teenager.");
} else {
   console.log("You're probably an adult.");
}
```

If/else statements are very common and will exist in almost all software that you write. Sometimes you'll need to create conditionals that have many different potential branches; in these cases, a [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#switch_statement) may be a better match. Similar to an if/else statement, switches also allows you to execute different code based on a condition.

```js
var command = "stop";

switch (command) {
   // Each case is a potential branch. The first case will be run if command == "start".
   case "start":
      console.log("Starting the service...");
      // The end of a case is indicated by a `break` statement, which indicates that the
      // program should continue running after the switch statement. WARNING: forgetting
      // to add a switch statement is a common source of bugs, since the program will "fall
      // through" to the next case if an explicit break isn't added.
      break;
   case "stop":
      console.log("Stopping the service...");
      break;
   case "restart":
      console.log("Restarting the service...");
      break;
   // `Default` is a special case, and runs if none of the other cases end up being true. You
   // don't need to have a default case unless it's useful.
   default:
      console.log("I don't recognize that command!");
      break;
}
```
#### Loops
Sometimes you may want to run a particular block of code more than once; for example, you may want to print out all of the song names from an array containing a bunch of song objects. Writing the same code over and over again doesn't work well for a variety of reasons; loops make it straightforward to repeat an expression or set of expressions more than once.

```js
// "For loops"
// For running a particular block of code a certain number of times. In this example, you
// can read the loop as "using the variable `i`, repeat the following block while `i` is less than
// favoriteThings.length (3 in this case), incrementing the value of `i` each time."
//
// The loop will run three times, printing out "I love" followed by one of my favorite things.
var favoriteThings = ["lamp", "dogs", "guacamole"];
for (var i = 0; i < favoriteThings.length; i++) {
   console.log("I love " + favoriteThings[i]);
}

// "While loops"
// Used for running as long as a given condition is true. For loops almost always use a
// counter variable (like `i` above); while loops may or may not. In this case we're using a
// while loop very similarly to a for loop by creating that `i` variable ourselves.
var message = "I can't go a day without thinking about ";
var i = 0;
while (i < favoriteThings.length - 1) {
   message = message + favoriteThings[i] + ",";
   // Don't forget to increment your counter variable if you're using one!
   i++;
}
message = message + " and " + favoriteThings[i];
console.log(message); // "I can't go a day without thinking about lamp, dogs, and guacamole"

// "For...of loops"
// New as of ES6, these loops are a simplification over standard for loops that hide the need
// for the `i` variable. You're still creating a new variable, but in this case it's set to the value of
// a different element in the `favoriteThings` array for each iteration.
for (var item of favoriteThings) {
   console.log("Did I mention that I love " + item + "?");
}
```

#### Functions
Functions allow us to provide a name and distinct [scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope) of execution for a set of expressions. Javascript has extremely powerful support for functions, and we'll cover the details in the Javascript Functions lesson in this unit.

#### Exceptions
Exceptions are an unambiguous way to indicate that a block of code isn't going to be able to complete it's job. Exceptions are "thrown" (created) when the issue occurs, and execution of the current function is immediately stopped before passing the exception back to the calling function to see if it can "catch" it (and if not, then it continues to the function that called it, then the function that called that function, and so on). Eventually either some code must handle the exception or the program will crash.

Exceptions are *thrown* when the error occurs, and need to be *caught* by a function that invoked it or the program will crash.

```js

// Try blocks are the fences that exceptions are caught in. If an exception is thrown by any
// code within the try block, it'll be passed to the catch block below it. If an exception occurs
// outside of the try block then the catch block won't be called.
try {
   callMyFunction(); // we're nervous that this might throw an exception
} catch (error) { // `error` is an object that contains some information about what went wrong
   // Do something with the error to recover.
}
```

Proper use of exceptions ("error handling") takes some practice but can make applications far more robust, especially those that have unpredictable inputs (humans, for example). [Here's a pretty good guide](https://www.joyent.com/developers/node/design/errors) to exception handling in Javascript.

### Promises, asynchronicity, and callbacks
One of the more complicated set of control flow mechanisms are related to asynchronous control flow; all of the tools above are synchronous, meaning they don't move on to the next expression until the one before it has finished. This leads to a very clear control flow because it's easy to see what happens next, but has the downside of making it harder to deal effectively with long-running tasks (like streaming a video) or responding to naturally asynchronous circumstances (events like a user's click, for example). Javascript leverages *callback functions* and *promises* to manage asynchronous control flows.

We'll be covering promises and callbacks thoroughly in other lessons, but let's take a look at a few examples in the context of how they might affect control flow.

```js
// Callbacks are functions that should be called when something interesting happens; for example,
// the loading of a page. For example, in web applications we often want to wait until the window
// is done loading before starting. We do this by providing a callback.
//
// In this example, the second parameter is a callback function. We're informing the browser that
// we want to invoke the callback function when the "load" event occurs. This would be difficult
// to handle with a synchronous control flow.
window.addEventListener('load', function() {
   console.log("Page loaded!");
});

// Another example: load events for XMLHttpRequest's. We'll set up the callbacks below the
// function definition.
function process() {
   // do what we need to with the response
}

// Set up a callback: "run the process() function when we get a response from our request."
var request = new XMLHttpRequest();
request.addEventListener('load', process); // another callback!
// ... more configuration and sending the request ...
```

The core problem that promises address is keeping track of an *eventual* result (such as a response from a server). Promises similarly make heavy use of the callback mechanism and are designed to make it easier to compose dependent asynchronous functions together. They are a new feature to the language in ES6 but have been around as a pattern for a little while[^1] because callbacks on their own can get quite confusing, especially when many of them are composed together in sequence.

Writing a promise for an XMLHttpRequest object, for example, might look like this:

```js
// This function returns a `Promise` object, which is then used by the example code below.
function PromiseXHR(method, url) {
   // The callback function that you pass to a promise needs to call `resolve` when
   // the operation is successful or `reject` when it isn't.
   return new Promise(function(resolve, reject) {
      var request = new XMLHttpRequest();
      request.open(method, url);
      request.addEventListener('load', function() {
         // 200 indicates that the request was a success.
         if (request.statusCode == 200) {
            resolve(req.response);
         } else {
            reject(new Error("Error making request: " + request.statusText));
         }
      });
      request.addEventListener('error', function() {
         reject(new Error("Error making AJAX request!"));
      });
      request.send();
   });
}

var xhrRequest = PromiseXHR("GET", "http://example.com/data.json");
// First run the parseResponse function, then the updateUI function (neither shown here).
// If either of those throw an exception, logError should be used to catch it.
xhrRequest.then(parseResponse).then(updateUI).catch(logError);
```
Don't worry if promises are blurry. We'll cover them in more detail later but just know that they're a powerful mechanism for managing control flow in asynchronous applications.
[^1]: https://www.promisejs.org/
