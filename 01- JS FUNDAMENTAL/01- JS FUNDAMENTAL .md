# 01- JS FUNDAMENTAL

# use strict

1. **'use strict'. When it is located at the top of a script, the whole script works the “modern” way.**
2. Please make sure that "use strict" is at the top of your scripts, otherwise strict mode may not be enabled.

### -  Variables

1. `let` – is a modern variable declaration.
2. `var` – is an old-school variable declaration. Normally we don’t use it at all, but we’ll cover subtle differences from `let` in the chapter [The old "var"](https://javascript.info/var), just in case you need them.
3. `const` – is like `let`, but the value of the variable can’t be changed.

# type OF

- `number` for numbers of any kind: integer or floating-point, integers are limited by `±(2531)`.
- `bigint` is for integer numbers of arbitrary length.
- `string` for strings. A string may have zero or more characters, there’s no separate single-character type.
- `boolean` for `true`/`false`.
- `null` for unknown values – a standalone type that has a single value `null`.
- `undefined` for unassigned values – a standalone type that has a single value `undefined`.
- `object` for more complex data structures.
- `symbol` for unique identifiers.

## Interaction: alert, prompt, confirm

**`alert`**

shows a message.

**`prompt`**

shows a message asking the user to input text. It returns the text or, if Cancel button or Esc is clicked, `null`.

**`confirm`**

shows a message and waits for the user to press “OK” or “Cancel”. It returns `true` for OK and `false` for Cancel/Esc.

### - operators

The operators `++` and `--` can be placed either before or after a variable.

- When the operator goes after the variable, it is in “postfix form”: `counter++`.
- The “prefix form” is when the operator goes before the variable: `++counter`.

## comparison

- Comparison operators return a boolean value.
- Strings are compared letter-by-letter in the “dictionary” order.
- When values of different types are compared, they get converted to numbers (with the exclusion of a strict equality check).
- The values `null` and `undefined` equal `==` each other and do not equal any other value.
- Be careful when using comparisons like `>` or `<` with variables that can occasionally be `null/undefined`. Checking for `null/undefined` separately is a good idea.

# IF

- The if(...) statement evaluates a condition in parentheses and, if the result is true, executes a block of code.
- The operator is represented by a question mark `?`. Sometimes it’s called “ternary”, because the operator has three operands. It is actually the one and only operator in JavaScript which has that many.

The syntax is:

`let result = condition ? value1 : value2;`

## operators

JavaScript supports the following operators:

**Arithmetical**Regular: `* + - /`, also `%` for the remainder and `**` for power of a number.
The binary plus `+` concatenates strings. And if any of the operands is a string, the other one is converted to string too:

`alert( '1' + 2 ); // '12', string
alert( 1 + '2' ); // '12', string`**Assignments**There is a simple assignment: `a = b` and combined ones like `a *= 2`.**Bitwise**Bitwise operators work with 32-bit integers at the lowest, bit-level: see the [docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Bitwise) when they are needed.**Conditional**The only operator with three parameters: `cond ? resultA : resultB`. If `cond` is truthy, returns `resultA`, otherwise `resultB`.**Logical operators**Logical AND `&&` and OR `||` perform short-circuit evaluation and then return the value where it stopped (not necessary `true`/`false`). Logical NOT `!` converts the operand to boolean type and returns the inverse value.**Nullish coalescing operator**The `??` operator provides a way to choose a defined value from a list of variables. The result of `a ?? b` is `a` unless it’s `null/undefined`, then `b`.**Comparisons**Equality check `==` for values of different types converts them to a number (except `null` and `undefined` that equal each other and nothing else), so these are equal:

`alert( 0 == false ); // true
alert( 0 == '' ); // true`
Other comparisons convert to a number as well.
The strict equality operator `===` doesn’t do the conversion: different types always mean different values for it.
Values `null` and `undefined` are special: they equal `==` each other and don’t equal anything else.
Greater/less comparisons compare strings character-by-character, other types are converted to a number.**Other operators**There are few others, like a comma operator.

More in: [Basic operators, maths](https://javascript.info/operators), [Comparisons](https://javascript.info/comparison), [Logical operators](https://javascript.info/logical-operators), [Nullish coalescing operator '??'](https://javascript.info/nullish-coalescing-operator).

# Logical operators

## OR || :

In classical programming, the logical OR is meant to manipulate boolean values only. If any of its arguments are true, it returns true, otherwise it returns false.

alert( true || true );   // true
alert( false || true );  // true
alert( true || false );  // true
alert( false || false ); // false

### OR "||" finds the first truthy value:

- Evaluates operands from left to right.
- For each operand, converts it to boolean. If the result is `true`, stops and returns the original value of that operand.
- If all operands have been evaluated (i.e. all were `false`), returns the last operand.

## (AND) &&

In classical programming, AND returns `true` if both operands are truthy and `false` otherwise:

```jsx
alert( true && true );   // true
alert( false && true );  // false
alert( true && false );  // false
alert( false && false ); // false
```

### AND “&&” finds the first falsy value

The AND `&&` operator does the following:

- Evaluates operands from left to right.
- For each operand, converts it to a boolean. If the result is `false`, stops and returns the original value of that operand.
- If all operands have been evaluated (i.e. all were truthy), returns the last operand.

## NOT !

The operator accepts a single argument and does the following:

1. Converts the operand to boolean type: `true/false`.
2. Returns the inverse value.

```jsx
alert( !true ); // false
alert( !0 ); // true
```

# Nullish coalescing operator '??'

The nullish coalescing operator is written as two question marks `??`.

As it treats `null` and `undefined` similarly, we’ll use a special term here, in this article. We’ll say that an expression is “defined” when it’s neither `null` nor `undefined`.

The result of `a ?? b` is:

- if `a` is defined, then `a`,
- if `a` isn’t defined, then `b`.

- The nullish coalescing operator `??` provides a short way to choose the first “defined” value from a list.

    It’s used to assign default values to variables:

    ```jsx
    // set height=100, if height is null or undefined
    height = height ?? 100;
    ```

- The operator `??` has a very low precedence, only a bit higher than `?` and `=`, so consider adding parentheses when using it in an expression.
- It’s forbidden to use it with `||` or `&&` without explicit parentheses.

# Loops

- We covered 3 types of loops:

    ```jsx
    // 1
    while (condition) {
      ...
    }

    // 2
    do {
      ...
    } while (condition);

    // 3
    for(let i = 0; i < 10; i++) {
      ...
    }
    ```

- The variable declared in `for(let...)` loop is visible only inside the loop. But we can also omit `let` and reuse an existing variable.
- Directives `break/continue` allow to exit the whole loop/current iteration. Use labels to break nested loops.

Details in: [Loops: while and for](https://javascript.info/while-for).

Later we’ll study more types of loops to deal with objects.

We covered 3 types of loops:

- `while` – The condition is checked before each iteration.
- `do..while` – The condition is checked after each iteration.
- `for (;;)` – The condition is checked before each iteration, additional settings available.

To make an “infinite” loop, usually the `while(true)` construct is used. Such a loop, just like any other, can be stopped with the `break` directive.

If we don’t want to do anything in the current iteration and would like to forward to the next one, we can use the `continue` directive.

`break/continue` support labels before the loop. A label is the only way for `break/continue` to escape a nested loop to go to an outer one.

# The “switch” construct

The “switch” construct can replace multiple `if` checks. It uses `===` (strict equality) for comparisons.

For instance:

```jsx
let age = prompt('Your age?', 18);

switch (age) {
  case 18:
    alert("Won't work"); // the result of prompt is a string, not a number
    break;

  case "18":
    alert("This works!");
    break;

  default:
    alert("Any value not equal to one above");
}
```

Details in: [The "switch" statement](https://javascript.info/switch).

# Functions

We covered three ways to create a function in JavaScript:

1. Function Declaration: the function in the main code flow

    ```jsx
    function sum(a, b) {
      let result = a + b;

      return result;
    }
    ```

2. Function Expression: the function in the context of an expression

    ```jsx
    let sum = function(a, b) {
      let result = a + b;

      return result;
    };
    ```

3. Arrow functions:

    ```jsx
    // expression at the right side
    let sum = (a, b) => a + b;

    // or multi-line syntax with { ... }, need return here:
    let sum = (a, b) => {
      // ...
      return a + b;
    }

    // without arguments
    let sayHi = () => alert("Hello");

    // with a single argument
    let double = n => n * 2;
    ```

- Functions may have local variables: those declared inside its body or its parameter list. Such variables are only visible inside the function.
- Parameters can have default values: `function sum(a = 1, b = 2) {...}`.
- Functions always return something. If there’s no `return` statement, then the result is `undefined`.

A function declaration looks like this:

```jsx
function name(parameters, delimited, by, comma) {
  /* code */
}
```

- Values passed to a function as parameters are copied to its local variables.
- A function may access outer variables. But it works only from inside out. The code outside of the function doesn’t see its local variables.
- A function can return a value. If it doesn’t, then its result is `undefined`.

To make the code clean and easy to understand, it’s recommended to use mainly local variables and parameters in the function, not outer variables.

It is always easier to understand a function which gets parameters, works with them and returns a result than a function which gets no parameters, but modifies outer variables as a side-effect.

Function naming:

- A name should clearly describe what the function does. When we see a function call in the code, a good name instantly gives us an understanding what it does and returns.
- A function is an action, so function names are usually verbal.
- There exist many well-known function prefixes like `create…`, `show…`, `get…`, `check…` and so on. Use them to hint what a function does.

Functions are the main building blocks of scripts. Now we’ve covered the basics, so we actually can start creating and using them. But that’s only the beginning of the path. We are going to return to them many times, going more deeply into their advanced features.

# Function expressions

- Functions are values. They can be assigned, copied or declared in any place of the code.
- If the function is declared as a separate statement in the main code flow, that’s called a “Function Declaration”.
- If the function is created as a part of an expression, it’s called a “Function Expression”.
- Function Declarations are processed before the code block is executed. They are visible everywhere in the block.
- Function Expressions are created when the execution flow reaches them.

In most cases when we need to declare a function, a Function Declaration is preferable, because it is visible prior to the declaration itself. That gives us more flexibility in code organization, and is usually more readable.

So we should use a Function Expression only when a Function Declaration is not fit for the task. We’ve seen a couple of examples of that in this chapter, and will see more in the future.

# Arrow functions

Arrow functions are handy for one-liners. They come in two flavors:

1. Without curly braces: `(...args) => expression` – the right side is an expression: the function evaluates it and returns the result.
2. With curly braces: `(...args) => { body }` – brackets allow us to write multiple statements inside the function, but we need an explicit `return` to return something.