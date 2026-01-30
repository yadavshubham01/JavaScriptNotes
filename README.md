# JavaScript Notes

---

## Things learned

1. map method is used when we want transformation of whole array.  
2. filter is used when we want to filter the arrar to obtain required value.  
3. reduce is used when we want to reduce the array to single value eg (max, min, avg, sum, difference etc).  
4. reduce passes two arguments one function(which includes accumulator and initial value as argument itself) and another initial value of accumulator.  

### Homework
```js
const output = user.reduce(function(acc, curr){
    if (curr.age < 30){
        acc.push(curr.firstName);
    }
    return acc;
}, [ ])

console.log(output);
````

---

## Things learned

1. Follow DRY(Don't Repeat Yourself) principle while coding.
2. Use function to stop writing repeating line of codes.
3. Function that takes another function as argument(callback function) is known as Higher order functions.
4. It is this ability that function can be stored, passed and returned, they are called first class citizens.
5. If we use Array.property.function-name. This function is accessible to any array in your code.

---

## Things learned

1. The setTimeout function stores it in the callback queue which is executed only after call stack is empty, even if setTimeout is set to 0ms.
2. setTimeout ensures that minimum it will take the time mentioned because it may be paused due to call stack not empty.

---

## Things learned

1. Browser has superpowers that are lent to JS engine to execute some tasks, these superpowers include web API's such as console, location, DOM API, setTimeout, fetch, local storage.
2. Callback functions and event handers are first stored in Web API environment and then transferred to callback queue.
3. Promises and mutation observer are stored in API environment and then transferred to microtask queue.
4. Event loop continuously observes call stack and when it is empty it transfers task to call stack.
5. Micro task is given priority over callback tasks.
6. Too many micro tasks generated can cause Starvation (nit giving time to callback tasks to execute).

---

## Closures

### Eg

```js
function x() {
    var a = 7;
    function y() {
        console.log(a);
    }
    y();
}
x();
```

Output : 7

Closure :
Scope
Closure(x)
a : 7

Closure basically means that a function bind together with its lexical environment.
Or function along with its lexical scope is known as Closure.

The function y() was bonded to the variables of function x(). That is, y() forms a closure and it has access to it's parent's lexical scope.

Closure in JS is a form of lexical scoping used to preserve variables from the outer scope (x()) of a function in the inner scope of a function (y()).

A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment).

In other words, a closure gives you access to an outer function's scope from an inner function.

In JS, closures are created every time a function is created, at function creation time.

---

### In JS, we can assign function to variables

```js
function a() {
    var x = 9;
    var b = function y() {
        console.log(x);
    }
    b();
}
a();
```

Output : 9

---

### Functions can also be passed as arguments

```js
function x(y) {
    var a = 9;
    console.log(y);
    y();
}

x(function y() {
    console.log("hello world");
});
```

---

### Functions can also be returned

```js
function x() {
    var a=7;
    function y () {
        console.log(a);
    }
    return y;
}

var z = x();
```

---

### Eg

```js
function x() {
    var a=7;
    function y () {
        console.log(a);
    }
    return y;
}

var z = x();
console.log(z);
```

Output :

```
f y() {
 console.log(a);
}
```

---

### Eg

```js
function x() {
    var a=7;
    function y () {
        console.log(a);
    }
    return y;
}

x();
var z = x();
z();
```

Output : 7

Even though, after the function x() finishes executing and it is nowhere residing in the call stack, it's execution context is also deleted.

So, how is "z" able to find the value of "a".

It is because when functions are returned, they still remember where they came from, they still remember the values of variables they have access to.

We know that y() in its closure contains the reference to "a".

So, when y() was returned, not only a function is returned, it's closure or lexical scope is also returned.

---

### Eg

```js
function x() {
    var a=7;
    function y () {
        console.log(a);
    }
    a = 100;
    return y;
}

var z = x();
z();
```

Output : 100

---

### Eg

```js
function z() {
    var b = 100;
    function x() {
        var a = 7;
        function y () {
            console.log(a,b);
        }
        y();
    }
    x();
}

z();
```

Output : 7 100

Memory Picture :
Scope :
Closure (x) → a : 7
Closure (z) → b : 100

---

### Eg

```js
function z() {
    var b = 100;
    function x() {
        var a = 7;
        function y () {
            console.log(a,b);
        }
        return y;
    }
    return x;
}

var p = z();
var t = p();
t();
```

Output : 7 100

---

### Uses of closures

A. Module Design Pattern
B. Currying
C. Maintaining state in async world
D. setTimeouts
E. Iterators

---

## Blocks & Scope

1. Code inside curly bracket is called block.
2. Multiple statements are grouped inside a block so it can be written where JS expects single statements like in if, else, loop, function etc.
3. Block values are stored inside separate memory than global. They are stored in block. (the reason let and const are called block scope)
4. Shadowing of variables using var, let and const.
5. The shadow should not cross the scope of original otherwise it will give error.
6. shadowing let with var is illegal shadowing and gives error.
7. var value is stored in nearest outer function or global scope and hence can be accessed outside block as well whereas same is not the case with let and const.

---

## let / const / var

* let and const are hoisted. we cant use them before initialization is result of "temporal dead zone".
* js use diff memory than global execution context to store let and cost. which is reason behind "temporal dead zone".
* level of strictness ... var<<let<<const.

var → no temporal dead zone, can redeclare and re-initialize, stored in GES
let → use TDZ, can't re-declare, can re-initialize, stored in separate memory
const → use TDZ, can't re-declare, can't re-initialize, stored in separate memory

syntax error ... violation of JS syntax
type error ... while trying to re-initialize const variable
reference error ... while trying to access variable which is not there in global memory

---

## Scope & Lexical Environment

1. Scope of a variable is directly dependent on the lexical environment.
2. Whenever an execution context is created, a lexical environment is created. Lexical environment is the local memory along with the lexical environment of its parent.
3. Having the reference of parent's lexical environment means, the child or the local function can access all the variables and functions defined in the memory space of its lexical parent.
4. The JS engine first searches for a variable in the current local memory space, if its not found here it searches for the variable in the lexical environment of its parent, and if its still not found, then it searches that variable in the subsequent lexical environments, and the sequence goes on until the variable is found in some lexical environment or the lexical environment becomes NULL.
5. The mechanism of searching variables in the subsequent lexical environments is known as Scope Chain. If a variable is not found anywhere, then we say that the variable is not present in the scope chain.

---

## Undefined

1. Undefined is like a placeholder till a variable is not assigned a value.
2. undefined !== not defined
3. JS- weakly typed language since it doesn't depend on data type declarations.

---

## Execution Context

1. We learnt how functions work in JS.
2. At first a global execution context is created, which consists of Memory and code and has 2 phases: Memory allocation phase and code execution phase.
3. In the first phase, the variables are assigned "undefined" while functions have their own code.
4. Whenever there is a function declaration in the code, a separate local execution context gets created having its own phases and is pushed into the call stack.
5. Once the function ends, the EC is removed from the call stack.
6. When the program ends, even the global EC is pulled out of the call stack.

---

## Hoisting

1. In JS, before the code is executed, the variables get initialized to undefined.
2. Arrow functions enact as variables and get "undefined" during the memory creation phase while functions actually get run.
3. Hoisting: Mechanism in JS where the variable declarations are moved to the top of the scope before execution. Therefore it is possible to call a function before initializing it.
4. Whenever a JS program is run, a global execution block is created, which comprises of 2: Memory creation and Code execution.
5. Variable declarations are scanned and are made undefined
6. Function declarations are scanned and are made available

---

## Callbacks

1. Function that is passed on as argument to another function is called callback function.
2. setTimeout helps turn JS which is sinhlethreaded and synchronous into asynchronous.
3. Event listeners can also invoke closures with scope.
4. Event listeners consume a lot of memory which can potentially slow down the website therefore it is good practice to remove if it is not used.

---

## Function Types

### Function Statement

```js
function xyz(){
    console.log("Function Statement");
}
```

---

### Function Expression

```js
var a = function(){
    console.log("Function Expression");
}
```

---

### Anonymous Function

```js
function(){
}
```

---

### Named Function Expression

```js
var a = function xyx(){
    console.log("Names Function Expression");
}
```

---

### Parameters vs Arguments

Parameters → When we creating a function & put some variabels in this ( )
Arguments → When we call this function & pass a variabel in this ( )

---

## First Class Function

The Ability of use function as value

* Can be passed as an Argument
* Can be executed inside a closured function
* Can be taken as return form

```js
var b = function(param){
    return function xyz(){
        console.log(" F C F ");
    }
}
```

---

Functions are heart of JS.
They are called first class citizens or first class functions because they have the ability to be stored in the variables, passed as parameters and arguments. They can also be returned in the function.
