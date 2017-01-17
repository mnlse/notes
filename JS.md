Author: Matthew Nielsen  
License: <a href="https://creativecommons.org/licenses/by/4.0/" target="_blank">Creative Commons</a>

# 8 - Functions

Function vs function expression:  
~~~
function(x) { ...code } // “function”
var fnc = function(x) { ...code } // “function expression”
~~~


## Anonymous functions

Immediately invoked function expression:  

Execute the function as soon as you define it. (no invoker at later point):

~~~
(function(x){ ... })(x); 
~~~
This function gets automatically invoked. This is an *anonymous* function - there is no name for it.

You can write it this way too:
~~~
(function(x){ ... }(x));  // less visually obvious, don’t use
~~~

Naming:  
* **IIFE** - **I**mmediately **I**nvoked **F**unction **E**xpression  
* Self Executing Anonymous function  

These functions can have parameters too:
~~~
(function(a,b) { console.log(a + “ “ + b) }) (10,20); 
=> 10 20
~~~       

## Lexical Scope
~~~
(function test(x) {

   (function() {
      console.log(x); })(); // Self executing function that prints variable “x”

}
~~~
During compile time, the compiler sees that the _child function_ does not have
the variable `x`, so it looks in the _parent function_ for `x`. 

The child function prints “x”, because the compiler passed this variable to the child.

_This is the essence of lexical scoping_.

## Closures

A _closure_ is a function that has access to its own private data

~~~
var fnc = function() {
   var name = “Matt”;
   console.log(name)
   } // This is a closure 

function Person(pName) {
   var name  = pName;
   this.getName = function() { // This function has access
      console.log(pName);     // to the private/local data: variable “pName”
   }
}
~~~
The `pName` is not directly visible globally, but you can get `pName` by calling the
function `getName`. Example:
~~~
var me = new Person(“Matt”);
me.getName(); // returns “pName” which in this case is “Matt”
=> "Matt"
~~~

This is the essence of a closure function. It’s a _function that has access to its own_
_private data_. 


Interesting use of a closure:
~~~
function counter() 
{
   var n = 0;
   return 
      {
      plusOne: function() { n++; },
      resetCounter: function() { n = 0; };
      }
}

var k = counter(); 
~~~

`counter()` returns an object with two properties: `plusOne`, `resetCounter`, which is now the `k` variable


You can increment and reset the variable `n`, don't have direct access to it.

## Function Properties, Methods, and Constructor
Functions are objects just like anything in JS. They have their own properties,
methods and a special `Function()` constructor

### arguments.length
Specifies the number of arguments passed to the function. This property is read-only.
In other words, it returns the _arity_ of the function. 

**arity** - the number of arguments a function takes at a given invocation

---
### call() and apply()

call() and apply() allow to indirectly _invoke a function_ **as if it were a method** of
some other object. 

~~~
function fnc() { ... };
fnc.call(objectName); 
~~~
`fnc` will be invoked as a method of object `objectName`.

_call()_ and _apply()_ are **methods** of the Function() constructor.

If you want to pass arguments to the function:
~~~
fnc.call(objectName, arg1, arg2)

fnc.apply(objectName, [arg1, arg2])
                        ^ This is an array!
~~~                        

The difference between `call()` and `apply()` is the way to pass arguments.  
`call()` takes arguments normally, and `apply()` takes an array of arguments.

This enables you to do some iteration on these arguments, or using the .length property.
You can modify the arguments of apply() more easily, because it’s an array.
### bind()

`bind()` ties a function to an object as a method. 

So when you _bind_ a function to an object:
you create a new object property - a new method, that when executed,
invokes the binded function **as a method**.

~~~
function fnc(arg) { return this.x + arg; }
var myObject = { x:1 };
~~~

You have a function and an object. The function takes the “x” property of the current
object and adds it to “arg”(argument passed to the function). Now I have to bind
the function and the object, so the function is a method of the object.

I can use the `call` and `apply` methods, or I can use the `bind` method.

~~~
var newMethod = fnc.bind(myObject);
~~~
This creates a new variable, that when executed, executes `fnc` **as a method** of 
`myObject`.

~~~
newMethod(3); 
~~~

Note: you can’t do just simply do “fnc.bind(myObject)” without the variable declaration,
because bind() doesn’t alter the myObject. It returns a new function that when executed
executes the passed function as a method of the passed object.

---

You can pass more arguments than just the name of the object.

~~~
function fnc(arg1, arg2) { return arg1 + arg2; }
var example = fnc.bind(null, 3);
example(2);

=> 5
~~~
The second argument of bind() - 3 gets assigned to `arg1`

So `arg1 = 3`, `arg2` is defined by the invocation.

Object passed is `null` just to show the example (but the code works).

`bind()` works only in ECMAScript 5+, but you can emulate it in ES3 with polyfills.

### toString()
Almost always returns the source code of the function.

### Function() constructor

~~~
var fnc = new Function(“x”, “y”, “return x+y”);
~~~

Each time `fnc` is invoked, a new function is created - not efficient.
`fnc` is always treated as a top-level(global) function. It’s not lexically scoped.

~~~
var scope = “global”;

function test() {
   var scope = “local”;
   return new Function(“return scope”); 
}

test()
=> "global"
~~~


## Functional programming <sub>basics<sub>
JS is not a functional prog. lang. like Lisp/Haskell, but the fact that
JavaScript can manipulate functions as objects means that we can use functional programming techniques in JavaScript.

ES5 methods such as `map()` and `reduce()` work well with functional programming techniques.

### Processing Arrays with Functions

~~~
var sum = function(a, b) {
    return a+b;
}

var data = [1,2,3,4,5,6]
var mean = data.reduce(sum)/data.length; // produces mean of data
~~~                  
ES6 way to write this function:
~~~
var sum = (a,b) => a+b
~~~

### Higher-Order Functions

A higher-order function is a _function that operates on functions_, taking one
or more functions as arguments and returning a new function.

Example:

~~~
function not(f) {
   return function() {                              // Return a new function
      var result = f.apply(this, arguments);        // That calls f
      return !result;                               // and returns the opposite of its result
   };
}

var is_even = function(x) { return x % 2 === 0; };

var is_odd = not(even);
~~~            

Function `not` returns a new function, and that function populates variable `odd`

So var `odd` is populated with:  
~~~
var odd = function() {
          var result = even.apply(this, arguments);
          return !result;
          };
~~~          

even(4)   
`=> true`  
odd(3) -> call even, negate result  
`=> true`

### Partial application of a function

This happens when you predefine arguments of a function either by call() or apply().

`f.call(null, 2, 3)`  
2 and 3 are predefined arguments, function is partially applied

### Memoization

**memoization** - storing the results of a function call, used to speed up programs

fnc that logs results of each invocation to an array as a property of the function (object):
~~~
function myFnc(x,y) {
   var result = x+y;

   if(!myFnc.array) {                           // if array doesn’t exist
      return myFnc.array = [result];            // create new array
   }
   else {                                       // if array exists
   myFnc.array.push(result);                    // push result to end
   var returnVal = myFnc.array.toString();      // convert array to string
   return returnVal;                            // return string
   }
}
~~~

Note: `myFnc.push` (etc.) was used as an example. It’s better to use `this`
so you can rename the function, or call it as a method of another object.

# 9 - Classes and Modules

A class of objects consists of _instances_ - members of a class.

If two objects inherit from the same prototype object, you can say that
they are instances of the same class.

## Classes and prototypes
In JavaScript, a class is a set of objects that inherit from the same prototype object.
The _prototype_ object is the central feature of a class.

A “factory” function is a function that creates new instances of a class.

~~~
function myFactory() {
   var obj = inherit(myFactory.myClass);
   return obj;
}

var myFactory.myClass = {
   x: 1,
   y: 2
};
~~~

It works, but you need to define an “inherit” function. Works for old browsers.

## Classes and constructors
Proper way to create a class

~~~
function myObject(val1, val2) {
   this.val1 = val1;
   this.val2 = val2;
   }

myObject.prototype = {
   x: this.val1,
   y: this.val2
};

var k = new myObject(1,2);
~~~
Constructor function fills “k” with the object: {x: 1, y: 2}

Note: the property of the function has to be called `prototype`. This is a special keyword that indicates the object and enables the constructor function.

Any JavaScript function can be a constructor, therefore every JavaScript function has a
`prototype` property.

The `prototype` property is an **object** with one default value: `constructor` which is
the content of the original function.

~~~
function myObj() { }
var content = myObj.prototype.constructor.toString();
// returns “function myObj() { }” - the actual content of the parent function

content === myObj; → true    // Same thing
~~~

## instanceof & isPrototypeOf

~~~
var k = new Date;
k instanceOf Date;
=> true
k isPrototypeOf Date;   // Omits the constructor function
=> true 
~~~

## ducktype approach
There are two basic approaches to classes:  
• the normal one - an object is an instance of a class if it inherited from it  
• ducktype - an object is an instance of a class if it looks simmilar to it  

If an object looks simmilar to a class, you assume that it’s an instance, even though
it might not directly inherit.

“If a ducks looks like a duck, and quacks like a duck, you assume it to be a duck”.

## Object-Oriented Techniques

A set class:

A _set_ is a data structure that represents an unordered collection of values, with
no duplicates. The fundamental operations on sets are adding values, and testing if
a value is a member of the set.

JavaScript’s objects are basically _sets of property names, with values associated with each name_.

## Inheritance

Instantiation - creating multiple instances of one thing.

Two types of inheritance:  
• Classical Inheritance  
• Prototypal Inheritance

Classical inheritance is when you use a function and the `new` keyword. 

Prototypal inheritance is when you use a normal object, and you instanciate with
`Object.create()`


Classical:
~~~
var Person = function(x, y) { this.x = x, this.y = y };
var mike = new Person(1, 2);
~~~

Prototypial:
~~~
var Person = { x:1, y:2 };
var mike = Object.create(Person);
~~~

The difference is that in Prototypial Inheritance you don’t have to use a function, and it’s
easier to grasp. But often you need a function to initialize some values:

~~~
var Person = function(name) { this.name: name };
var mike = new Person(‘mike’);

console.log(mike.name)
=> “mike”
~~~

You could do this with prototypial this way:  

~~~
var Person = 
{ 
    initializeName: function(name) 
    { 
        this.name = name; 
    } 
};

var mike = Object.create(Person);
mike.initializeName(‘mike’);
~~~

This is a very simplistic, rather ineffective way to do this. More effective, writing less:

~~~
var Person = { 
               species: ‘homo sapiens’,
               newInstance: function(name, haircolor, gender) {
                  var newObject = Object.create(this);  // Create new instance
                  newObject.name = name;  // Initialize name
                  newObject.haircolor = haircolor; // etc.
                  newObject.gender = gender;
                  return newObject;
               };
~~~               

And now you can do this:
~~~
var mike = Person.newInstance(‘Michael’, ‘black’, ‘male’);
~~~

You can also initialize `mike` as a class:
~~~
var mikesDaughter = mike.newInstance(....);
~~~

To prevent these objects from being parents you you must destroy their reproductive ability, a.k.a. getting rid of
the `newInstance` method.  

`isPrototypeOf` works here:

~~~
Person.isPrototypeOf(mike);
=> true
~~~

To make `instanceOf` work:
mike instanceof Person.constructor; → true

## Subclasses
In OO programming, a class B can _extend_ or _subclass_ another class A.

We say that A is the _superclass_ and B is the _subclass_. Instances of B inherit all
instance methods of A.

B can define its own methods, and override its superclass. 

~~~
var superclass = { x:1, y:2 };
var subclass = Object.create(superclass);
               ^ Inherit from “superclass”

subclass.x
=> 1
subclass.x = 43;
subclass.x;
=> 43                   // “x” defined by superclass gets overridden by subclass

superclass.z = 99;
subclass.z;
=> 99                   // Subclass is responsive to any superclass changes
~~~



# Advanced JavaScript
## Introduction
Books:  
• You don’t know JS  
• JavaScript Patterns  
• High Performance JavaScript

<a href="http://www.github.com/rwldrn/idiomatic.js" target="_blank">Idiomatic.js</a> style guide


EcmaScript Language Specification - difficult reading
Search functionality is your friend when reading the spec.

## 1. Scope
### Intro 
JavaScript is a compiled language, but it’s different - we don’t distribute
binary blobs. 

The difference between compiled vs interpreted:  
When interpreter language looks at line 3, it doesn’t know what is in line 4.
A compiled language generally knows what’s going on in before execution.


JavaScript gets compiled first, and a few miliseconds after gets executed.
JavaScript consists of **two passes** - the compilation, and the execution.

### LHS vs RHS - LeftHandSide, RightHandSide

`lhs` = left hand side  
`rhs` = right hand side
~~~
var foo = “bar”;
    ^lhs    ^rhs

var lhs_Var = “rhs_Value”;
~~~

1. **Before compilation:**
~~~
function bar() {       // 1. create individual scope for function bar();
   var foo = “baz”;    // 2. add variable “foo” to function scope w/ val “baz”
}
~~~

Every function has its own scope, defined at compilation.

2. **After compilation:**
~~~
function bar() {       // Start function
   foo = “baz”         // Ask function scope for value of variable ‘foo’
}                      // end
~~~

Answer from function bar:  
Yes, I have variable “foo” → send memory reference.

~~~
function bar() {
   21312323 = baz;
   ^ memory id
}
~~~
(assign value to this memory ID)

What happens when variable is not defined in scope:  
~~~
function bar() {
   bam = “yay”;     // Hey “bar”, do you have variable named “bam”?
                    // “bar”: No, I don’t have that variable. Look in outer scope.
}                   // Start search for “bam” in global scope
                    // If variable “x” is not created in global scope, global scope
                    // creates the variable “x”.
~~~                    


To prevent _variable leakage_ to global scope, use strict mode.

`undeclared` - we were unable to find a proper lhs reference for your variable
               in our scopes

`undefined`  - variable has been declared, but has a special empty value
               this should have been called “un-initialized”.
               “undefined” is a special kind of value, but it is a value

~~~
// “undeclared”:
unDec; 
=> TypeError, variable not defined

// “undefined”:
var unDef; // variable is declared
unDef;
=> undefined
~~~

~~~
function bar(sampleVar) {
   
   var sampleVar = 3;           // Hey scope, I want to declare “samplevar”
                                // scope: no big deal, it’s already declared

   “var samplevar = 3” -----> “samplevar = 3”
         ^ The compiler turns a declaration into a variable assignment
}
~~~

### L3 - More on LHS, RHS
~~~
function bar(x) { ... }
         ^ “bar” is an RHS reference - why?
            because it is not an LHS reference.
~~~

An **LHS** is only when you have assignment

~~~
var x = “hey”;
    ^ LHS   ^ RHS
~~~    

~~~
function bar(x) { ... }
          ^ RHS
~~~          

RHS and LHS have different behavior. 

~~~
bar();  // → Step 1: Retrieve “bar” variable
        // bar
        // → Step 2: See the parentheses
        // ()
        // → Step 3: Try to execute the variable
~~~        

**Everything that is not LHS is RHS**

~~~
var foo = “bar”;
    ^ LHS   ^ RHS
~~~

~~~
foo; // no assignment, therefore no LHS, therefore this expression is *RHS*

foo;
=> “bar”
~~~

If you ask for an undeclared (in current scope) **LHS** object, it gets created in 
the global scope.  
If you ask for an undeclared (in current scope) **RHS** object, it throws a
ReferenceError.


### L4 - Function declaration, anonymous functions, try catch is block-scoped

Function declaration vs function expression.

~~~
function hey(x) {...} // “function declaration”
var hey = function(x) {...} // “function expression”
~~~


~~~
var foo = function bar() {
            console.log(“I’m a function ‘bar’ inside a variable ‘foo’”);
          };

foo();
bar();     // Error!
~~~

Most function **expressions** are anonymous:
`var hey = function(x) { ... }` 

Three negatives to anonymous function expressions:  
• no way to refer to the current running function  
• they don’t play well in debugging  
• it self-descriptive (you can give it an intuitive name)  

Always use named functions in expressions!

It’s better to name your function, because your variable could be changed:

~~~
var foo = function(x) {
            var foo = “hey”;
            foo();              // No longer works to reference the current function
};

// Better way:

var foo = function bar(x) {
            var foo = “hey”;
            bar();
            };
~~~ 

The `catch` phrase is _block scoped_ 

~~~
catch(err) {           //
   var hey = err;      //  Block Scoped
}                      // 

console.log(hey); // ReferenceError
~~~


### L5 - Dynamic Scoping vs. Lexical Scoping, “eval”, “with” keywords
Dynamic scoping vs Lexical scoping

Most languages use lexical scoping. Lexical scoping means compile-time scope.

**Compile-time scope** - the compiler, during compile-time defines the scope, after
                        compilation the scopes can’t be changed

Eval is a way to cheat lexical scoping:

~~~
function foo(arg) {
   eval(arg);               // Evaluate passed in argument
   console.log(heyThere);   // heyThere = 42
};

foo(“var heyThere = 42;”);
~~~

This slows down the JS engine. It prevents optimizations.
Don’t use eval, ever. 


`with` keyword:

~~~
obj.a = obj.b + obj.c; 

with (obj) {
   a = b + c;     // It operates on properties of object “obj”
   d = b - a;
   d = 3;         // The problem with “with”
}

d;               // it’s not defined on “obj” !
=> 3

obj.d; // undefined
~~~

`with` behaves like a lexical scope, but it creates a completely new lexical scope.
It’s even worse than eval, no optimization etc.

### L6 - IIFEs and their Purpose
**IIFE** - Immediately Invoked Function Expression

Self-executing functions serve the function of _hiding variables_

~~~
(function() {
   var foo = “foo2”;
   console.log(foo);
})();                        
=> “foo2”

console.log(foo) 
=> ReferenceError: foo is not defined
~~~
It’s called “IIFE”. You want to name your “IIFE” functions. 

### L7 - “let” keyword - block scoped variables, and the “let” block
`let` keyword - block-scoped variables

The `let` keyword:  
• it does not hoist  
• needs more thinking than var  

`let` block:

~~~
let (varName = ‘hey there’) {
   console.log(varName);   // → ‘hey there’
}
   console.log(varName);   // Error
~~~   
`let` declares and initializes a variable that is block scoped to the { } block.


How does a “let block” work:
1. “let” statement + variable assignment

`let (name = “Matt”)`  
^ keyword    ^ variable declaration


2. Create the special block

~~~
let (name = “Matt”) 
{
   // special block
   // variable “name” is only defined within this block
}
~~~


It doesn’t work in ES6 though. How to solve it:
~~~
{
   let name = “Matt”;
   console.log(name);
}
~~~

### L8 - Lexical Scoping vs Dynamic Scoping with examples

Lexical Scoping vs Dynamic Scoping  
**lexical** = author time decision   
Lexical gets compiled, and then the scopes are solid and unchangeable

**dynamic** = runtime decision  
Dynamic scoping is fluid - scopes are defined during runtime

Example of theoritical dynamic scoping:
~~~
function foo() {
   console.log(bar);  // “bar”
   // Look down

   // Why does it print “bar” and not “ReferenceError”
   // When “bar” is looked up, it first looks at current function
   // There is no “bar” in current function/scope
   // So the _runtime_ looks for the place where *bar was declared*
   // This is an example of _dynamic scoping_ and it does not occur in JavaScript
   // In JavaScript, since it is lexically scoped, this would throw a
   // ReferenceError - variable is undeclared.
}

function baz() {
   var bar = “bar”;   // Declare “bar”
   foo();             // Call “foo"
   // => "bar"
}

baz();  // 1. Call function “baz”
~~~

### L9 - Hoisting with examples - Pre-compiled vs Post-compiled

Pre-compiled code:  
~~~
a;
b;
var a = b;
var b = 2;
b;
a;
~~~

^ This code gets compiled and you get  
Post-compiled code:
~~~
var a;  // Compiler looks for variables and declares them
var b;  // at the beginning

a;      // undefined
b;      // undefined
a = b;  // This is where assignment happens
b = 2;  // These variables are no longer declared here, but at the top. This was
        // changed during the compilation
b;      // undefined
a;      // 2;
~~~


All LHS gets handled during compile-time. 



How it works for functions:
~~~
var a = b(); // “Hey there” - function declaration
var c = d(); // Error: Not a function - function expression
a;  // 
c;  //

function b() {
   return “Hey there”;
}

var d = function() {
   return “I’m a function expression”;
};
~~~

Reason:  
function expressions behave differently to function declarations.

Variable `b` gets processed by the compiler, but variable `d` and its contained
function gets hoisted.

So:  
`var c = d()` // `d` is not a function, because d is undefined at this time.

How to think about it:
Function declarations get moved to the top during compile time.

~~~
function b() {          // Functions are at the very top
   return “Hey there”;  // after compilation
}

var a;                  // Now the variables get
var c;                  // declared

a = b();                // Function “b” is already initialized, so it gets executed
// => “Hey there”
c = d();                // Variable d that contains the function gets hoisted
// => not a function    // So it is undefined, and you can’t execute undefined()

d = function() {
   return “I’m a function expression”;
};
c = d();                // Now it works.
~~~

### L10 - Function Hoisting - Part 2 + Mutual Recursion

Pre-compiled code:  
~~~
answer();

var answer = 42;

function answer() {
   console.log(“first function”);
}

function answer() {
   console.log(“second function”);
}
~~~
Post-compiled code:

~~~
function answer() {                    // This function gets initialized first
   console.log(“first function”);   
}
function answer() {                    // Declared second, overrides the first
   console.log(“second function”);
}
var answer; // Gets ignored - already declared as a function

answer(); // → “second function”
~~~

Conclusion:
Functions get hoisted first
Variable declarations get hoisted second


**Recursion** - when a function calls itself  
**Mutual recursion** - when two or more functions call each other

Functions get hoisted, because **mutual recursion** needs to work.

Example:
~~~
function a(foo) {
   if (foo > 20} return foo;   // if foo > 20 return foo
   return b(foo+2);            // or else call function b to increase value of foo
}                              // If “b” wasn’t initialized, this code would not work

function b(foo) {
   return c(foo) + 1;
}                              // If “c” wasn’t initialized, this code wouldn’t work

function c(foo) {
   return a(foo+2);
}

a(1);
~~~

### L11 - “this” keyword

Every function, _while executing_ has a reference to its current execution context,
called `this`

The `this` keyword depends on the _call location_:

~~~
function foo() {
   console.log(this.bar);
}

var bar = “bar1”;
var o2 = { bar: “bar2”, foo: foo };
var o3 = { bar: “bar3”, foo: foo };

foo();       // “bar1”
o2.foo();    // “bar2”
o3.foo();    // “bar3”
~~~

Four rules of the `this` keyword:  
If you ever have a problem with the “this” keyword look at the _call location_ and
check these four rules:

                                     * * *

4) **Default binding rule** - `this` always returns an *object*, and it is always
                           _non empty_.

~~~
function foo() {
   console.log(this.bar);
}

var bar = “bar1”;

foo(); // “bar1” <- When the call site looks like this, this (fourth) rule always applies
~~~
In this case `this` = `window`

**This is the default rule**  
If none of the other rules apply, this rule is valid

The default rule:   
If I’m in _strict mode_ → default `this` keyword to `undefined`  
If I’m in _normal mode_ → default `this` keyword to the global object (window etc.)

Strict mode doesn’t have to be global: it can be function-wide.

~~~
“use strict”
function foo() { ... };  ← this function uses strict, so strict rule applies -
                           “undefined”
~~~                           

---

3) **The implicit binding rule** → the object at the call site becomes the `this` binding

~~~
function foo() {
   console.log(this.bar);
}
var o2 = { bar: “bar-o2”, foo: foo };   // This is the “base object”
                        ^ This is not a copy, but just a _reference_ to the function
                          The “foo” property is the call site.

// o2 is a “base object”. 

o2.foo() // “bar-o2” ← The “this” bind becomes the base object of the call site.
~~~

---

2) **The explicit binding rule** - if you use `.call` or `.apply`,
                               they take a `this` argument as the first parameter, which
                               is an object

~~~
function foo() {
   console.log(this.bar);
}

var bar = “bar1”;
var obj = { bar: “HeyThere” };

foo();    
=> “bar1” (window.bar)
foo.call(obj);          //  Base object gets changed, “this” binding = “obj”
=> “HeyThere” (obj.bar)
~~~

Sometimes I want to make the function foo() more predictable. Example:
I want the `this` object to be of a certain type (i don’t want to operate on any
junk object).

To do this - use “hard binding”.

~~~
function foo() {
   console.log(this.bar);
}

var obj = { bar: “bar” };
var obj2 = { bar: “bar2” };

var orig = foo;                        // Create new variable that references to function foo.

foo = function() { orig.call(obj); };  // Bind the “this” keyword of the function
                                       // foo to object “obj”.

foo();                                 // “bar”
foo.call(obj2);                        // “bar”  ← Can’t call it as a method of “obj2”, because it is
                                       //   hard binded to “obj”.
~~~                              

In this example, function foo() always use `obj` as the `this` reference.


Automated binding solution:  
~~~
function bind(fn, obj) {     // This function takes in an object
   return function() {       // and a function as arguments
      fn.call(obj);          // and it binds the function’s “this” keyword to passed
   }                         // object.
}

function foo() { console.log(this); };
var objectTheFunctionIsBoundTo = { };

foo = bind(foo, objectTheFunctionIsBoundTo);
~~~
This will always print “objectTheFunctionIsBoundTo”, no matter where it’s called from.

You can also add this binding solution to the prototype of function, using the apply
method (instead of call used here).

There is a native method of Function prototype called “bind” that works well.

---

<span>1</span>) **Was the function created with ‘new’**? If so, `this` is the object.
~~~
function foo() { console.log(this); }
var k = new foo();
k(); → “k”
~~~
The important difference between `this` and lexical scoping is that
the value of `this` is determined at runtime, while lexical scoping is determined
at author time.

That means → the value of “this” of a function _depends on where you call it from_.

You could run the same function from 4 different places and with each call there would
be a different “this” value for that function.

### L12 - The “new” keyword
The `new` keyword has nothing to do with instantiating classes. 
JavaScript _does not_ have classes, and the new keyword _has nothing to do_
with instantiating classes.

What the `new` keyword does:
1. A brand new empty object will be created
2. The new object gets linked to a different object (explained later)
3. That brand new object gets bound as the “this” keyword for the function.
4. If that function does not return anything, it will return the “this” object.



## 2. Closure
What is closure? Closure comes from _lambda calculus_. 

**closure** - when a function remembers its lexical scope even when the function
is executed outside that lexical scope.

### L1 - Basics

~~~
function foo() {
   var bar = “bar”;

   return function() {
      console.log(bar);
   };
}

function bam() {
   foo()();             // “bar”
}

bam();
~~~

As long as there is a something to be done later in the code that is related
to the lexical scope, the lexical scope doesn’t get deleted.

Only once the lexical scope truly has no further use in the code, it gets garbage
collected (purged from memory).

~~~
function counter() {
   if(!count) { var count = 0; };
   return function() { console.log(++count); };
}

var k = counter();
k(); // 1         The lexical scope of counter()
k(); // 2         is preserved, because it’s still in play
k(); // 3         by the “k” function expression.
~~~


### L2 - Classical module pattern - way to use closures

Crockford’s classical module pattern:  
• there has to be an enclosing function  
• there has to be one or more functions returned that have access to the private scope
  made possible by the enclosing function

Example:  
~~~
var foo = (function() {          // Enclosing function
            var name = ‘Matt’;   // private variables
            var surname = ‘Nielsen’;
            return {
               sayMyName: function() { console.log(name); },
               sayMySurname: function() { console.log(surname); }
            };
})();    // in this case enclosing function is an IIFE
~~~

Foo is _automatically initialized_ (since this is an IIFE) to an object
that contains two properties defined in the return statement.

Why use this? Principle of least privilege.

### L3 - Modified module pattern - stylistically more organized
A stylistically better way, that can use _named functions_:

~~~
var foo = (function() {
   var myFnc = {
      
      bar: function() { myFnc.baz(); },
      baz: function() { console.log(‘baz’); };

   };  // myFnc assignment ends
   return myFnc;  // Make this function return myFnc object (it’s named now)
})();  // End the IIFE
~~~

And also, you have a way to reference it during runtime.

### L4 - Modern module pattern
~~~
define(“foo”, function()
{
var name = ‘Matt’;
return {
   sayMyName: function() { console.log(name); }
}
});   // the first argument is the name of the variable
~~~

This is the same as the classical module pattern, except it requires an additional
library, and spares you the “inconvenience” of using IIFEs.

It converts your code info 
~~~
var foo = IIFE
^ 1st arg  ^ 2nd arg
define(“foo”,   function() { ... });
~~~

### L5 - How it works in ES6
In ES6 there is a public scope, accessible by the user and then there are private
scopes.

The public scope is the main javascript file loaded by HTML.
Every other file has its own private scope that is file-wide.

_<span>private</span>.js_
~~~
var o = { bar: “bar” };

export function bar() {
   return o.bar;
}
~~~

_<span>main</span>.js_
~~~
import bar from “private”;      // If you want to import one thing from “private”
bar();   // “bar”

module foo from “foo”            // If you want to import the whole “private” file
foo.bar();   // “bar”
~~~

## 3. Object-Orienting
### L1 - Prototype mechanism
Every single “object” is built by a constructor function. 
It’s not an instantiated class, but built by a constructor function.

Each time a constructor is called, a new object is called.
It’s not an instance, but a new object.

It’s often said that a constructor makes an object “based on” its own prototype.
It’s not true, because it implies that it takes a prototype and stamps a copy of it.
It doesn’t work like that in JS. 

Remember the four rules of the “new” keyword. 

To get the prototype of an object, you can use the 
objectName.<span>\_\_proto\_\_   
Two underscores → proto → two underscores

\_\_proto\_\_ and .constructor are both writeable, so you can change them.


Why doesn’t JavaScript make an object “based” on its prototype?
~~~
function Foo() {
    this.me = “hey”;
}

var k = new Foo(); 
~~~
  1. New object (`k`) gets created
  2. `k` gets linked to foo.prototype via [[P]] link
  3. `k` becomes the `this` keyword for function `Foo`
  4. function returns the `this` object

Note that the object `k` is **only** linked to the Foo.prototype object.
It’s NOT a new copy! It just has a link to Foo.prototype

function Foo(who) {
   this.me = who;
}
Foo.prototype.myName = “matt”;
var k = new Foo(“test”);
          
           k object           Foo.prototype
    / +——————————+            +——————————+
      |          |   [[P]]    |          |
      | me       | =========> | myName   |  ——> (constructor etc.)
      |          |            |          |
    / +——————————+            +——————————+

        ^ Notice that the “k” object does not have the property “myName”.
          You could create a thousand “new” objects and they wouldn’t have that
          property. Only Foo.property has “myName”.

To access `k.myName`, the compiler has to traverse to the Foo.prototype object and
extract it from there.

~~~
// The `k` object has only the properties created by the constructor function,
// because during execution

function Foo(who) {       // of this function
   this.me = who;         // this.me gets changed to k.me
}
var k = new Foo(“test”);  // on this line
~~~

Therefore `k.me == “test”`.

JavaScript _does not have classes_, because prototyping a new object does
_not create a copy of its prototype_, but instead - creates a **[[P]]
link** from the new object to its prototype. So it’s more memory efficient

The **[[P]]** link behaves like lexical scope.
If you can’t find a property on the current object, it traverses to its prototype object.

~~~
  +—————————+
  | - - - - |  < Top object                || go down
  | - - - - |  < prototype1                || 
  | - - - - |  < prototype of prototype1   || go down
  | - - - - |  < prototype3                ||
  | - - - - |  < prototype4                || go down
  | - - - - |  < prototype5                ||
  | - - - - |  < Object.prototype          —— If not in Object.prototype - can’t find
  +—————————+
~~~
 

### L2 - Linking prototypes

~~~
function Foo(who) {
   this.me = who;
}

Foo.prototype.identify = 
    function() {
        return “I am “ + this.me;
    };

function Bar(who) {
   foo.call(this,who);
}

Bar.prototype = Object.create(Foo.prototype);

Bar.prototype.speak = function() {
  alert(“Hello, “ + this.identify() + “.”);
  };

var b1 = new Bar(“b1”);
var b2 = new Bar(“b2”);

b1.speak();
b2.speak();
~~~


# Functional
## Separate mutation from calculation
~~~
function teaser(size, element)
{
   setText(element, slice(0, size, text(element)));
}
~~~

This is a function that takes in an element, like a div element, and slices it down.
The problem with this function is that **it sets the DOM on its own**.
Calculation and mutation are not separate. So, to separate it:

~~~
var teaser = slice(0);
map(compose(setText, teaser(50), text), all(‘p’));
~~~

## Recognize pure functions
Functions that don’t change anything are called “pure”.
Because they don’t change anything, they are:  
• testable  
• portable  
• memoizable (easy to remember)  
• paralleizable (asynchronous)  

A function shouldn’t log anything, set anything, or rely on any
other function.

## Recursion
Recursion - two characteristics:  
the thing calls itself,  
the thing has a base case that stops the recursion

# ES6
ECMA - European Computer Manufacture Association

Provides JS specs. First spec = 1997
