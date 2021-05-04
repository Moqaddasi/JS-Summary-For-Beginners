# 07-Advanced working with functions

# Recursion and stack

Terms:

- *Recursion* is a programming term that means calling a function from itself. Recursive functions can be used to solve tasks in elegant ways.

    When a function calls itself, that’s called a *recursion step*. The *basis* of recursion is function arguments that make the task so simple that the function does not make further calls.

- A [recursively-defined](https://en.wikipedia.org/wiki/Recursive_data_type) data structure is a data structure that can be defined using itself.

    For instance, the linked list can be defined as a data structure consisting of an object referencing a list (or null).

    `list = { value, next -> list }`

    Trees like HTML elements tree or the department tree from this chapter are also naturally recursive: they branch and every branch can have other branches.

    Recursive functions can be used to walk them as we’ve seen in the `sumSalary` example.

Any recursive function can be rewritten into an iterative one. And that’s sometimes required to optimize stuff. But for many tasks a recursive solution is fast enough and easier to write and support.

# Rest parameters and spread syntax

## Rest parameters ...

The rest of the parameters can be included in the function definition by using three dots `...` followed by the name of the array that will contain them. The dots literally mean “gather the remaining parameters into an array”.

For instance, to gather all arguments into array `args`:

```jsx
function sumAll(...args) { // args is the name for the array
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}

alert( sumAll(1) ); // 1
alert( sumAll(1, 2) ); // 3
alert( sumAll(1, 2, 3) ); // 6
```

**The rest parameters must be at the end**

The rest parameters gather all remaining arguments, so the following does not make sense and causes an error:

```jsx
function f(arg1, ...rest, arg2) { // arg2 after ...rest ?!
  // error
}
```

The `...rest` must always be last.

## The “arguments” variable

**Arrow functions do not have `"arguments"`**

If we access the `arguments` object from an arrow function, it takes them from the outer “normal” function.

Here’s an example:

```jsx
function f() {
  let showArg = () => alert(arguments[0]);
  showArg();
}

f(1); // 1
```

As we remember, arrow functions don’t have their own `this`. Now we know they don’t have the special `arguments` object either.

## At the end

When we see `"..."` in the code, it is either rest parameters or the spread syntax.

There’s an easy way to distinguish between them:

- When `...` is at the end of function parameters, it’s “rest parameters” and gathers the rest of the list of arguments into an array.
- When `...` occurs in a function call or alike, it’s called a “spread syntax” and expands an array into a list.

Use patterns:

- Rest parameters are used to create functions that accept any number of arguments.
- The spread syntax is used to pass an array to functions that normally require a list of many arguments.

Together they help to travel between a list and an array of parameters with ease.

All arguments of a function call are also available in “old-style” `arguments`: array-like iterable object.

# Variable scope, closure

## Code blocks

If a variable is declared inside a code block {...}, it’s only visible inside that block.

```jsx
{
  // do some job with local variables that should not be seen outside

  let message = "Hello"; // only visible in this block

  alert(message); // Hello
}

alert(message); // Error: message is not defined
```

There’d be an error without blocks

Please note, without separate blocks there would be an error, if we use `let` with the existing variable name:

```jsx
// show message
let message = "Hello";
alert(message);

// show another message
*let message = "Goodbye"; // Error: variable already declared*alert(message);
```

## Nested functions

A function is called “nested” when it is created inside another function.

It is easily possible to do this with JavaScript.

We can use it to organize our code, like this:

```jsx
function sayHiBye(firstName, lastName) {

  // helper nested function to use below
  function getFullName() {
    return firstName + " " + lastName;
  }

  alert( "Hello, " + getFullName() );
  alert( "Bye, " + getFullName() );

}
```

### Closure

There is a general programming term “closure”, that developers generally should know.

A [closure](https://en.wikipedia.org/wiki/Closure_(computer_programming)) is a function that remembers its outer variables and can access them. In some languages, that’s not possible, or a function should be written in a special way to make it happen. But as explained above, in JavaScript, all functions are naturally closures (there is only one exception, to be covered in [The "new Function" syntax](https://javascript.info/new-function)).

That is: they automatically remember where they were created using a hidden `[[Environment]]` property, and then their code can access outer variables.

When on an interview, a frontend developer gets a question about “what’s a closure?”, a valid answer would be a definition of the closure and an explanation that all functions in JavaScript are closures, and maybe a few more words about technical details: the `[[Environment]]` property and how Lexical Environments work.

# Function object, NFE

Functions are objects.

Here we covered their properties:

- `name` – the function name. Usually taken from the function definition, but if there’s none, JavaScript tries to guess it from the context (e.g. an assignment).
- `length` – the number of arguments in the function definition. Rest parameters are not counted.

If the function is declared as a Function Expression (not in the main code flow), and it carries the name, then it is called a Named Function Expression. The name can be used inside to reference itself, for recursive calls or such.

Also, functions may carry additional properties. Many well-known JavaScript libraries make great use of this feature.

They create a “main” function and attach many other “helper” functions to it. For instance, the [jQuery](https://jquery.com/) library creates a function named `$`. The [lodash](https://lodash.com/) library creates a function `_`, and then adds `_.clone`, `_.keyBy` and other properties to it (see the [docs](https://lodash.com/docs) when you want to learn more about them). Actually, they do it to lessen their pollution of the global space, so that a single library gives only one global variable. That reduces the possibility of naming conflicts.

So, a function can do a useful job by itself and also carry a bunch of other functionality in properties.

# Arrow functions revisited

### Arrow functions have no “this”

As we remember from the chapter Object methods, "this", arrow functions do not have this. If this is accessed, it is taken from the outside.

For instance, we can use it to iterate inside an object method:

```jsx
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    *this.students.forEach(
      student => alert(this.title + ': ' + student)
    );*}
};

group.showList();
```

Here in `forEach`, the arrow function is used, so `this.title` in it is exactly the same as in the outer method `showList`. That is: `group.title`.

If we used a “regular” function, there would be an error:

```jsx
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    *this.students.forEach(function(student) {
      // Error: Cannot read property 'title' of undefined
      alert(this.title + ': ' + student);
    });*}
};

group.showList();
```

The error occurs because `forEach` runs functions with `this=undefined` by default, so the attempt to access `undefined.title` is made.

That doesn’t affect arrow functions, because they just don’t have `this`.

**Arrow functions can’t run with `new`**

Not having `this` naturally means another limitation: arrow functions can’t be used as constructors. They can’t be called with `new`.

**Arrow functions VS bind**

There’s a subtle difference between an arrow function `=>` and a regular function called with `.bind(this)`:

- `.bind(this)` creates a “bound version” of the function.
- The arrow `=>` doesn’t create any binding. The function simply doesn’t have `this`. The lookup of `this` is made exactly the same way as a regular variable search: in the outer lexical environment.

## Arrows have no “arguments”

Arrow functions also have no `arguments` variable.

That’s great for decorators, when we need to forward a call with the current `this` and `arguments`.

For instance, `defer(f, ms)` gets a function and returns a wrapper around it that delays the call by `ms` milliseconds:

```jsx
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms);
  };
}

function sayHi(who) {
  alert('Hello, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("John"); // Hello, John after 2 seconds
```

The same without an arrow function would look like:

```jsx
function defer(f, ms) {
  return function(...args) {
    let ctx = this;
    setTimeout(function() {
      return f.apply(ctx, args);
    }, ms);
  };
}
```

Here we had to create additional variables `args` and `ctx` so that the function inside `setTimeout` could take them.