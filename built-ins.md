
## JSON

We've seen a lot of JavaScript objects at this point, but there is a related structure that is important for you to understand and know the differences between the two. Where as JavaScript objects defined with curly braces: `{}` are used inside executable blocks of code, JSON is a data interchange format. [JSON stands for "JavaScript Object Notation"](http://json.org/) and it is a simple format for transferring data from one place to another, whether the two places are different servers, or even just in files on the same machine.

The primary difference between the two structures is that because JSON is not used in executable code blocks, it can only contain static data - that means no expressions (like math) and no function calls! For example, this is a valid JavaScript _object_ but **is not valid JSON**:

```js
{
  name: "John Doe",
  birthday: "2016-" + currentMonth + "-21"
}
```

It is not valid because it uses a variable which can only exist in executable code! In addition, JSON must adhere to a strict format so that it can be used by various types of systems. Ruby, PHP, Java, Python... all of these languages can use JSON as a data transfer format, but obviously they cannot run JavaScript code. Below is a valid JSON object showing all allowed data structures (but note that you can nest multiple objects or arrays inside each other).

```js
{
  "name": "John Doe",
  "age": 35,
  "isAwesome": true,
  "hobbies": [ "coding", "programming", "javascript" ],
  "pet": { "name": "Spot", "type": "dog" },
  "significantOther": null
}
```

And luckily for us, JavaScript has a convenient way to [convert from JavaScript objects to JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) and back:

```js
var aValidJSObject = JSON.parse("{ ... }");
var aJSONString = JSON.stringify({ ... });
// Note that you would put your JSON or JS object where "..." is above!
```

## Date

The [`Date` object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) give us the ability to get the current date, but also to extract specific pieces of a date from a string representation of a date. You must first create an "instance" of a Date. We'll discuss creating object constructors later, but for now just know that this syntax (with the `new` keyword) gives you an object of type `Date` for you to work with. Here are some uses:

```js
var d = new Date();  // Create a new Date representing the moment this object was created
d.getFullYear()      // returns the four-digit current year
d.getMonth()         // returns the current month index (0-based), so March is 2
d.getDate()          // returns the day of the month for the date object

var indDay = new Date("July 4, 2016");  // Create a date object for the given date
indDay.getHours()    // military hours... since we didn't specify them above, it will be the number of hours
                     // offset from greenwich mean time from midnight (in other words, don't rely on it)
ind.getDay()         // index day of the week this falls on (0-based, Sunday first), so this would be 1
ind.toISOString()    // this is a very common date format to return because it is standardized
                     // in our case this would probably be: "2016-07-04T04:00:00.000Z" (depending on timezone)
```

If you need to format date and time strings more specifically, then you may want to look at a JavaScript code library like [moment.js](http://momentjs.com/).

### Timestamps

You may already be able to see how the format of dates as strings can complicate things. Because of this, many people in software developer choose to work with timestamps instead of date strings whenever possible. A timestamp in JavaScript is the number of milliseconds that have passed since midnight, January 1, 1970 (a point in time called the [Unix epoch](https://en.wikipedia.org/wiki/Unix_time)). For example, while writing this lesson, my current timestamp is `1458499087578` which is "2016-03-20T18:38:07.578Z".

You can get the current timestamp from a `Date` object using the `getTime()` method on the object: `myDate.getTime()` or you can get the current timestamp any time using: `Date.now()`

Be careful though, many other languages define timestamps in **seconds** and not **milliseconds** like JavaScript does!

## Math

The [`Math` object in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) allows us to perform a number of complicated mathematical calculations as well as storing good representations of difficult constants (like `PI`). Some of the constants and methods are below, but you should explore the documentation on your own! Note that all functions on the `Math` object are static - you do not need to "create" a Math instance object.

```js
Math.PI
Math.E
Math.sin(x)
Math.max(x, y, z, ...)
Math.random()  // note that the random number is between 0 and 1 with numerous decimal places
               // also note that this *SHOULD NOT* be used for cryptography!
Math.round()
Math.floor()   // round a number down to the nearest integer
Math.ceil()    // round a number up to the nearest integer
```

## Strings

We've already seen quite a bit about strings when discussing our primitive data types, but there is also an [object form of strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) we can use when we need to inspect or manipulate a string. This interface gives us much more information about our strings:

```js
var name = new String("Jordan Kasper");
name.length          // 13
name.toUpperCase()   // "JORDAN KASPER"
name.indexOf("a")    // 4 (0-based index, only the first entry!)
name.indexOf("a", 5) // 8 (start the search AT index 5, will be -1 if nothing found!)
name.split(" ")      // ["Jordan", "Kasper"]
name.substr(7)       // "Kasper"
name.substr(7, 3)    // "Kas"
```

It is important to note that while I created a `String` object above, I could have performed all of these operations directly on a String primitive:

```js
var name = "Jordan";
name.toLowerCase()   // "jordan" ... but why did this work???
```

We can do this because JavaScript is smart enough to know that if I give it a string primitive, but then try to perform an object action on it (like calling the `toLowerCase()` method), that it should first convert the primitive string into a `String` object. This process is called "boxing" and is found in some other languages, and required by the developer in others.
