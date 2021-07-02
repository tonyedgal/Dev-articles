# Closures in Javascript: All the basics you need to know about `Closures`,

the article will go over the following

- Functions and scopes
- What are closures in JavaScript
- How closures are handled in memory
- Why it is named `closure`

#### FUNCTIONS

A function is similar to a procedure or a set of statements that is used to perform a specific task. For a procedure to qualify as a function, it should take some input, perform various actions on that data and return a result.

Generally speaking, there are several ways to define functions

- Function declaration
- Function expression
- Arrow syntax

```javascript
// Function daclaration - Uses the function keyword
function myFunc() = {};

// Function expression - the name can be omitted, giving an anonymous function
var a = function() {}; // name omitted
var b = function myFuncTwo() {}; //function name included

// Arrow functions - arrow function syntax is a shorter syntax for a function expression
const c = () => {};
```

#### SCOPES

A scope is a policy that manages the availability of variables. A variable defined inside a scope is accessible only within that scope, but inaccessible outside.

The scope where a variable is located decides if it is accessible or inaccessible from certain parts of the program.

There are two types of scopes

- Global Scope
- Block or Local scope

```javascript
// Global scopes are variables that are accessible from any part of the program

var e = 2 // variable declared in the global scope

const square = () => {
  return e * e
}
console.log(square()) // outputs 4

// Block/local scope refers to variables declared within a block '{}'

var f = 5 // variable declared in the global scope

const times = () => {
  let g = 5 // variable declared in the block/local scope
  return f * g
}
console.log(times()) // Outputs 25
console.log(g) // outputs undefined, because it was defined within the times function.
```

#### CLOSURE

`Closure` - A function that is a first-class object, that has access to variables defined in the same local scope in which it was defined.

In other words, a closure gives you access to an outer function’s scope from an inner function.

Lets look at closure with three examples

```javascript
// 1
function extFunc() {
  // Define a variable local to extFunc
  const extVar = "I used a closure"
  function intFunc() {
    // We can access the variable defined in the scope of extFunc within inFunc
    console.log(extVar)
  }
  // Return the inner function. Note that we're not calling it here!
  return intFunc
}
// Call the outer function, which returns the inner function
const closure = extFunc()
// Call the returned function that we stored as a variable
closure()
// outputs 'I used a closure'

// 2
const seconds = 60
const text = "minutes is equal to"
function timeConversion() {
  let minutes = 2
  return function minutesToSeconds() {
    const minToSec = `${minutes} ${text} ${seconds * minutes} seconds`
    return minToSec
  }
}
const convert = timeConversion()
console.log(convert()) // outputs "2 minutes is equal to 120 seconds"
console.log(timeConversion()()) // outputs "2 minutes is equal to 120 seconds"

// 3
function scores() {
  var score = 85
  function displayScore() {
    alert(score)
  }
  displayScore()
}
const showScore = scores()
showScore()
```

**_in example 1_**
`extFunc()` creates a local variable called `extVar` and a function called `intFunc()`. The `intFunc()` function is an inner function that is defined inside `extFunc()` and is available only within the body of the `extFunc()` function. Note that the `intFunc()` function has no local variables of its own. However, since inner functions have access to the variables of outer functions, `intFunc()` can access the variable name declared in the parent function, `extFunc()`.

**_in example 2_**
the `return intFunc` line in **_1_** can be avoided by returning the internal function at the time of declaration.

**_in example 3_**
in **_3_** the internal function is not returned (only called) because of the alert command within its block.

```javascript
// switching the code in 3 from alert to console.log
function scores() {
  var score = 85
  function displayScore() {
    console.log(score)
  }
  displayScore()
}
const showScore = scores()
showScore() // outputs 85 to the console
// get TypeError showScore is not a function
```

At first glance, it might seem unintuitive that this code still works. In some programming languages, the local variables within a function exist for just the duration of that function's execution. Once `scores()` finishes executing, you might expect that the name variable would no longer be accessible. However, because the code still works as expected, this is obviously not the case in JavaScript.

The reason is that functions in JavaScript form closures. A closure is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time the closure was created. In this case, `showScore` is a reference to the instance of the function `displayScore` that is created when `scores()` is run. The instance of `displayScore` maintains a reference to its lexical environment, within which the variable name exists. For this reason, when `showScore` is invoked, the variable `score` remains available for use, and "85" is passed to console, followed by a TypeError.

When the internal function is created, the Javascript engine detects that for the function to be executed in the future, a reference will be needed to variable declared in the external function scope.

To solve this, the engine keeps a link to this variable for later use, and stores that link in a special function scoped execution context.

Such a function with 'memory' about the environment where it was created is simply known as: `a Closure`.

#### HOW CLOSURES ARE HANDLED IN MEMORY

When a pure function that depends on its own arguments and data is called, its gets pushed to the `**CALL STACK**`, where it is executed and it data is kept in memory until it is removed.

But when a function references data outside it's own scope, i.e. from its lexical environment or an external function, for the interpreter to call this function or know the value of this free variables, it creates a `closure` to store them in place in memory where they can be accessed later. That area in memory is called a `**HEAP MEMORY**`.

Now unlike the `call stack` which is short-lived, the `heap memory` can store data indefinitely and decide when it's ready to be discarded.

Closures require more memory and processing power than regular functions but has many important practical uses, e.g. `Data Encapsulation`.

Data encapsulation is simply a method of protecting data to prevent it from leaking to where it is not needed.

##### WHY THE NAME CLOSURES THEN?

This is because the internal function inspects it's environment and closes over the variables in the lexical scope in which it is defined, and that it needs to remember for future use. The references to the variables are closed in a special data structure that can only be accessed by the Javascript runtime itself.
