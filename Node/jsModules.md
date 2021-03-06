# Modularizing Code in Node/Javascript #
If you write a Something.js that has some functions in it you'd like to expose to the outside world, put `exports.FunctionIwantToExpose = FunctionIwantToExpose;` at the end. Then in the file that you want to call the function in just add `var stuff = require('Something.js');`  That creates an object called `stuff` that you can use to call the function `stuff.FunctionIwantToExpose()`. Note the first `FunctionIwantToExpose` behind exports is the actual name of the function you'll call and can be different than the original function from the library (ie. `exports.newname = oldname` would allow you to call it as something else).

It can be used two ways (this is from below):

```javascript
module.exports.User = User; //this is the same as exports.User = User; it ends up getting use by var user = require('./user'); then user.User;
//vs
module.exports = User;
```
Results in you being able to use it as:

```javascript
var user = require('./user');

var u = new user.User();
//vsvar u = new user();
```

This would allow you to use a namespaced object with functions in it without having a clumsy additional name.

```javascript
BunchaFunctions //object that contains several functions that are used BunchaFunctions.funct1, BunchaFunctions.funct2, etc.
module.exports.BunchaFunctions = BunchaFunctions;
//vs module.exports = BunchaFunctions;
var importedFunctions = require('./FileThatContainsBunchaFunctions');
```

```javascript
var UseFunction = new importedFunctions.BunchaFunctions.funct1();  //This is clumsy//
//vs.
var UseFunction = new Functions.funct1();                          //This is obviously much better
```

## Examples ##
### [Simple Example.](http://blog.liangzan.net/blog/2012/06/04/how-to-use-exports-in-nodejs/) ###

[This shows that exports is literally just an object that you add stuff to.](http://blog.liangzan.net/blog/2012/06/04/how-to-use-exports-in-nodejs/)

### [StackOverflow Question Exposing Functions to Anothre Module.](http://stackoverflow.com/questions/5311334/what-is-the-purpose-of-nodejs-module-exports-and-how-do-you-use-it) ###

`module.exports` is the object that's actually returned as the result of a `require` call.

The `exports` variable is initially set to that same object (i.e. it's a shorthand "alias"), so in the module code you would usually write something like this:

```javascript
var myFunc1 = function() { ... };var myFunc2 = function() { ... };
exports.myFunc1 = myFunc1;
exports.myFunc2 = myFunc2;
```
to export (or "expose") the internally scoped functions `myFunc1` and `myFunc2`.

In the calling code you would use:

```javascript
var m = require('mymodule');
m.myFunc1();
```
where the last line shows how the result of require is (usually) just a plain object whose properties may be accessed.

NB: if you overwrite `exports` then it will no longer refer to `module.exports`. So if you wish to assign a new object (or a function reference) to `exports` then you should also assign that new object to `module.exports`.

---
It's worth noting that the name added to the exports object does not have to be the same as the module's internally scoped name for the value that you're adding, so you could have:

```javascript
var myVeryLongInternalName = function() { ... };
exports.shortName = myVeryLongInternalName;// add other objects, functions, as required
```

followed by:

```javascript
var m = require('mymodule');
m.shortName(); // invokes module.myVeryLongInternalName
```

### Node.js, Require and Exports ###
This is another way to do the exact same thing (except instead of `exports`, it uses `module.exports`):

Back when I first started playing with node.js, there was one thing that always made me uncomfortable. Embarrassingly, I'm talking about `module.exports`. I say embarrassingly because it's such a fundamental part of node.js and it's quite simple. In fact, looking back, I have no idea what my hang up was...I just remember being fuzzy on it. Assuming I'm not the only one who's had to take a second, and third, look at it before it finally started sinking in, I thought I could do a little write up.

In Node, things are only visible to other things in the same file. By things, I mean variables, functions, classes and class members. So, given a file misc.js with the following contents:

```javascript
var x = 5;
var addX = function(value) {
  return value + x;
};
```

Another file cannot access the `x` variable or `addX` function. This has nothing to do with the use of the var keyword. Rather, the fundamental Node building block is called a module which maps directly to a file. So we could say that the above file corresponds to a module named file1 and everything within that module (or any module) is private.

Now, before we look at how to expose things out of a module, let's look at loading a module. This is where require comes in. require is used to load a module, which is why its return value is typically assigned to a variable: `var misc = require('./misc');`

Of course, as long as our module doesn't expose anything, the above isn't very useful. To expose things we use module.exports and export everything we want:

```javascript
var x = 5;
var addX = function(value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;
```

Now we can use our loaded module:

```javascript
var misc = require('./misc');
console.log("Adding %d to 10 gives us %d", misc.x, misc.addX(10));
```

There's another way to expose things in a module:

```javascript
var User = function(name, email) {
  this.name = name;
  this.email = email;
};
module.exports = User;
```

The difference is subtle by important. See it? We are exporting user directly, without any indirection. The difference between:

```javascript
module.exports.User = User;
//vs
module.exports = User;
```

is all about how it's used:

```javascript
var user = require('./user');

var u = new user.User();
//vsvar u = new user();
```

It's pretty much a matter of whether your module is a container of exported values or not. You can actually mix the two within the same module, but I think that leads to a pretty ugly API.
Finally, the last thing to consider is what happens when you directly export a function:

```javascript
var powerLevel = function(level) {
  return level > 9000 ? "it's over 9000!!!" : level;
};
module.exports = powerLevel;
```

When you require the above file, the returned value is the actual function. This means that you can do: 

```javascript
`require('./powerlevel')(9050);`
```

Which is really just a condensed version of:

```javascript
var powerLevel = require('./powerlevel')
powerLevel(9050);
```

### [Difference between `exports` and `module.exports`](http://www.hacksparrow.com/node-js-exports-vs-module-exports.html) ###

#### What is the difference between exports and module.exports in Node.js? ####
You must be familiar with the exports object in Node.js modules, using which you create functions in your modules like this (assume in a file named rocker.js):

```javascript
exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

which you call from another file thus:

```javascript
var rocker = require('./rocker.js');
rocker.name(); // 'My name is Lemmy Kilmister'
```

#### But what the heck is module.exports? Is it even legal? ####
Here is an eye-opener - `module.exports` is the real deal. `exports` is just `module.exports`'s little helper. Your module returns `module.exports` to the caller ultimately, not exports. All `exports` does is collect properties and attach them to `module.exports` IF `module.exports` doesn't have something on it already. If there's something attached to module.exports already, everything on exports is ignored.

Put the following in rocker.js:

```javascript
module.exports = 'ROCK IT!';
exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

And this in another file, and run it:

```javascript
var rocker = require('./rocker.js');
rocker.name(); // TypeError: Object ROCK IT! has no method 'name'
```

The rocker module completely ignored `exports.name`, and returned a string 'ROCK IT!'. From that you probably realize that your modules don't always have to be 'module instances'. Your modules can be any legal JavaScript object - boolean, number, date, JSON, string, function, array, and so on. Your module is whatever you set `module.exports` to. If you don't set `module.exports` to anything explicitly, the properties of exports and attached to it and returned.

In this case, your module is a class:

```javascript
module.exports = function(name, age) {
    this.name = name;
    this.age = age;
    this.about = function() {
        console.log(this.name +' is '+ this.age +' years old');
    };
};
```

and you'd use it this way:

```javascript
var Rocker = require('./rocker.js');
var r = new Rocker('Ozzy', 62);
r.about(); // Ozzy is 62 years old
```

In this case, your module is an array:

```javascript
module.exports = ['Lemmy Kilmister', 'Ozzy Osbourne', 'Ronnie James Dio', 'Steven Tyler', 'Mick Jagger'];
```

and you may use it this way:

```javascript
var rocker = require('./rocker.js');
console.log('Rockin in heaven: ' + rocker[2]); //Rockin in heaven: Ronnie James Dio
```

So you get the point now - if you want your module to be of a specific object type, use `module.exports`; if you want your module to be a typical module instance, use `exports`.

The result of attaching properties to `module.exports` is akin to attaching properties to `exports`. For example this:

```javascript
module.exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

does the same thing as:

```javascript
exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

But note that, they are not the same thing. As I said earlier `module.exports` is the real deal, `exports` is just its little helper. Having said that, `exports` is the recommended object unless you are planning to change the object type of your module from the traditional 'module instance' to something else.

### [Final Example of Exports vs Module.Exports](http://timnew.me/blog/2012/04/20/exports_vs_module_exports_in_node_js/) ###

I was confused about how require function works in node.js for a long time. I found when I require a module, sometimes I can get the object I want, but sometimes, I just got an empty object, which give the concern that we cannot export the object by assigning it to exports, but it seems somehow we can export a function by assignment.

Today, I re-read the [documentation](https://nodejs.org/en/docs/#modules_module_exports) again, and it's clear that I misunderstood the “require” mechanism.

I clearly remember this sentence in the doc that said `module.exports` is the **same** as the `exports` object.

So I believed that the `exports` was just a shortcut alias for `module.exports`, we can use one instead of another without worrying about the differences between them two. But this was incorrect. `exports` and `module.exports` are different.

Today I found this in the doc:

```
The exports object is created by the Module system. Sometimes this is not acceptable, many want their module to be an instance of some class. To do this assign the desired export object to module.exports.
```

So it says that `module.exports` is different from `exports`. And it you export something by assignment, you need to assign it to `module.exports`.

Let’s try to understand these sentences deeper by code examples. The documentation says "The exports object is created by the Module system."

The word “created by” actually means when node.js tries to load a javascript file, before executing any line of code in your file, the module system executes the following code first for you: `var exports = module.exports`.

So the actual interface in node.js’s module system is `module` object. the actual exported object is `module.exports` not `exports`. And the `exports` is just a normal variable, and there isn't any “magic” in it. So if you assign something to it, it is replaced absolutely.

That’s why I failed to get the exported object I want when I assign the it to `exports` variable.

So to export some variable as a whole, we should always assign it to `module.exports`. And at same time, if there is no good excuse, we’d better to keep the convention that exports is the shortcut alias to `module.exports`. So we should also assign the `module.exports` to exports.

As a conclusion, to export something in node.js by assignment, we should always follow the following pattern:

```javascript
exports = module.exports = {  ...}
```
