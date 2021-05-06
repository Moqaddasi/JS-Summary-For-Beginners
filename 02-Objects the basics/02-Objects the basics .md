# 02-Objects: the basics

# 2.1 Objects

In contrast, objects are used to store keyed collections of various data and more complex entities. In JavaScript, objects penetrate almost every aspect of the language. So we must understand them first before going in-depth anywhere else.

An object can be created with figure brackets `{…}` with an optional list of *properties*. A property is a “key: value” pair, where `key` is a string (also called a “property name”), and `value` can be anything.

### Literals and properties

```jsx
let user = {
  // an object
  name: "John", // by key "name" store value "John"
  age: 30, // by key "age" store value 30
};
```

In the `user` object, there are two properties:

1. The first property has the name `"name"` and the value `"John"`.
2. The second one has the name `"age"` and the value `30`.

Property values are accessible using the dot notation:

```jsx
// get property values of the object:
alert(user.name); // John
alert(user.age); // 30
```

To remove a property, we can use `delete` operator:

`delete user.age;`

### Square brackets

```jsx
// this would give a syntax error
user.likes birds = true
```

JavaScript doesn’t understand that. It thinks that we address user.likes, and then gives a syntax error when comes across unexpected birds.

The dot requires the key to be a valid variable identifier. That implies: contains no spaces, doesn’t start with a digit and doesn’t include special characters ($ and \_ are allowed).

There’s an alternative “square bracket notation” that works with any string:

```jsx
let user = {};

// set
user["likes birds"] = true;

// get
alert(user["likes birds"]); // true

// delete
delete user["likes birds"];
```

## Computed properties

We can use square brackets in an object literal, when creating an object. That’s called *computed properties*.

For instance:

```jsx
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  *[fruit]: 5, // the name of the property is taken from the variable fruit*};

alert( bag.apple ); // 5 if fruit="apple"
```

Square brackets are much more powerful than the dot notation. They allow any property names and variables. But they are also more cumbersome to write.

So most of the time, when property names are known and simple, the dot is used. And if we need something more complex, then we switch to square brackets.

## Property value shorthand

In real code we often use existing variables as values for property names.

For instance:

```jsx
function makeUser(name, age) {
  return {
    name: name,
    age: age,
    // ...other properties
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

In the example above, properties have the same names as variables. The use-case of making a property from a variable is so common, that there’s a special *property value shorthand* to make it shorter.

## Property names limitations

As we already know, a variable cannot have a name equal to one of language-reserved words like “for”, “let”, “return” etc.

But for an object property, there’s no such restriction

```jsx
// these properties are all right
let obj = {
  for: 1,
  let: 2,
  return: 3,
};

alert(obj.for + obj.let + obj.return); // 6
```

### Property existence test, “in” operator

```jsx
let user = { name: "John", age: 30 };

alert("age" in user); // true, user.age exists
alert("blabla" in user); // false, user.blabla doesn't exist
```

Please note that on the left side of in there must be a *property name*. That’s usually a quoted string.

Why does the `in` operator exist? Isn’t it enough to compare against `undefined`?

Well, most of the time the comparison with `undefined` works fine. But there’s a special case when it fails, but `"in"` works correctly.

It’s when an object property exists, but stores `undefined`:

```jsx
let obj = {
  test: undefined,
};

alert(obj.test); // it's undefined, so - no such property?

alert("test" in obj); // true, the property does exist!
```

### The “for…in” loop

To walk over all keys of an object, there exists a special form of the loop: for..in. This is a completely different thing from the for(;;) construct that we studied before.

```jsx
let user = {
  name: "John",
  age: 30,
  isAdmin: true,
};

for (let key in user) {
  // keys
  alert(key); // name, age, isAdmin
  // values for the keys
  alert(user[key]); // John, 30, true
}
```

Note that all “for” constructs allow us to declare the looping variable inside the loop, like `let key` here.

Also, we could use another variable name here instead of `key`. For instance, `"for (let prop in obj)"` is also widely used.

## At the end

Objects are associative arrays with several special features.

They store properties (key-value pairs), where:

- Property keys must be strings or symbols (usually strings).
- Values can be of any type.

To access a property, we can use:

- The dot notation: `obj.property`.
- Square brackets notation `obj["property"]`. Square brackets allow to take the key from a variable, like `obj[varWithKey]`.

Additional operators:

- To delete a property: `delete obj.prop`.
- To check if a property with the given key exists: `"key" in obj`.
- To iterate over an object: `for (let key in obj)` loop.

What we’ve studied in this chapter is called a “plain object”, or just `Object`.

There are many other kinds of objects in JavaScript:

- `Array` to store ordered data collections,
- `Date` to store the information about the date and time,
- `Error` to store the information about an error.
- …And so on.

They have their special features that we’ll study later. Sometimes people say something like “Array type” or “Date type”, but formally they are not types of their own, but belong to a single “object” data type. And they extend it in various ways.

Objects in JavaScript are very powerful. Here we’ve just scratched the surface of a topic that is really huge. We’ll be closely working with objects and learning more about them in further parts of the tutorial.

# 2.2 Object references and copying

When an object variable is copied, the reference is copied, but the object itself is not duplicated.

```jsx
let user = { name: "John" };

let admin = user;
```

We can use either variable to access the object and modify its contents:

```jsx
let user = { name: 'John' };

let admin = user;

*admin.name = 'Pete'; /* changed by the "admin" reference*alert(*user.name*); // \'Pete',
changes are seen from the "user" reference*/
```

## Comparison by reference

For instance, here `a` and `b` reference the same object, thus they are equal:

```jsx
let a = {};
let b = a; // copy the reference

alert(a == b); // true, both variables reference the same object
alert(a === b); // true
```

And here two independent objects are not equal, even though they look alike (both are empty):

```jsx
let a = {};
let b = {}; // two independent objects

alert(a == b); // false
```

For comparisons like `obj1 > obj2` or for a comparison against a primitive `obj == 5`, objects are converted to primitives. We’ll study how object conversions work very soon, but to tell the truth, such comparisons are needed very rarely – usually they appear as a result of a programming mistake.

### Cloning and merging, Object.assign

So, copying an object variable creates one more reference to the same object.

But what if we need to duplicate an object? Create an independent copy, a clone?

if we really want that, then we need to create a new object and replicate the structure of the existing one by iterating over its properties and copying them on the primitive level.

Like this:

```jsx
let user = {
  name: "John",
  age: 30
};

*let clone = {}; // the new empty object

// let's copy all user properties into it
for (let key in user) {
  clone[key] = user[key];
}*// now clone is a fully independent object with the same content
clone.name = "Pete"; // changed the data in it

alert( user.name ); // still John in the original object
```

Also we can use the method [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) for that.

The syntax is:

```jsx
Object.assign(dest, [src1, src2, src3...])
```

- The first argument `dest` is a target object.
- Further arguments `src1, ..., srcN` (can be as many as needed) are source objects.
- It copies the properties of all source objects `src1, ..., srcN` into the target `dest`. In other words, properties of all arguments starting from the second are copied into the first object.
- The call returns `dest`.

```jsx
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// copies all properties from permissions1 and permissions2 into user
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```

We also can use Object.assign to replace for..in loop for simple cloning

## Nested cloning

```jsx
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

alert(user.sizes.height); // 182
```

Now it’s not enough to copy `clone.sizes = user.sizes`, because the `user.sizes` is an object, it will be copied by reference. So `clone` and `user` will share the same sizes:

Like this:

```jsx
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

let clone = Object.assign({}, user);

alert(user.sizes === clone.sizes); // true, same object

// user and clone share sizes
user.sizes.width++; // change a property from one place
alert(clone.sizes.width); // 51, see the result from the other one
```

## At The End

Objects are assigned and copied by reference. In other words, a variable stores not the “object value”, but a “reference” (address in memory) for the value. So copying such a variable or passing it as a function argument copies that reference, not the object itself.

All operations via copied references (like adding/removing properties) are performed on the same single object.

To make a “real copy” (a clone) we can use `Object.assign` for the so-called “shallow copy” (nested objects are copied by reference) or a “deep cloning” function, such as [\_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).

# 2.3 Object methods, "this"

To access the object, a method can use the this keyword.

The value of `this` is the object “before dot”, the one used to call the method.

For instance:

```jsx
let user = {
  name: "John",
  age: 30,

  sayHi() {
    *// "this" is the "current object"
    alert(this.name);*}

};

user.sayHi(); // John
```

Here during the execution of `user.sayHi()`, the value of `this` will be `user`.

## “this” is not bound

In JavaScript, keyword `this` behaves unlike most other programming languages. It can be used in any function, even if it’s not a method of an object.

There’s no syntax error in the following example:

```jsx
function sayHi() {
  alert( *this*.name );
}
```

The value of `this` is evaluated during the run-time, depending on the context.

For instance, here the same function is assigned to two different objects and has different “this” in the calls:

```jsx
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

*// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;*// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

## Arrow functions have no “this”

Arrow functions are special: they don’t have their “own” `this`. If we reference `this` from such a function, it’s taken from the outer “normal” function.

For instance, here `arrow()` uses `this` from the outer `user.sayHi()` method:

```jsx
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  },
};

user.sayHi(); // Ilya
```

That’s a special feature of arrow functions, it’s useful when we actually do not want to have a separate `this`, but rather to take it from the outer context. Later in the chapter [Arrow functions revisited](https://javascript.info/arrow-functions) we’ll go more deeply into arrow functions.

## At The End

- Functions that are stored in object properties are called “methods”.
- Methods allow objects to “act” like `object.doSomething()`.
- Methods can reference the object as `this`.

The value of `this` is defined at run-time.

- When a function is declared, it may use `this`, but that `this` has no value until the function is called.
- A function can be copied between objects.
- When a function is called in the “method” syntax: `object.method()`, the value of `this` during the call is `object`.

Please note that arrow functions are special: they have no `this`. When `this` is accessed inside an arrow function, it is taken from outside.

# 2.4 Constructor, operator "new"

## Constructor function

Constructor functions technically are regular functions. There are two conventions though:

1. They are named with capital letter first.
2. They should be executed only with `"new"` operator.

For instance:

```jsx
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

*let user = new User("Jack");*alert(user.name); // Jack
alert(user.isAdmin); // false
```

When a function is executed with `new`, it does the following steps:

1. A new empty object is created and assigned to `this`.
2. The function body executes. Usually it modifies `this`, adds new properties to it.
3. The value of `this` is returned.

### Return from constructors

# 2.5 Optional chaining '?.'

The optional chaining ?. is a safe way to access nested object properties, even if an intermediate property doesn’t exist.

The obvious solution would be to check the value using `if` or the conditional operator `?`, before accessing its property, like this:

```jsx
let user = {};

alert(user.address ? user.address.street : undefined);
```

### Optional chaining

The optional chaining `?.` stops the evaluation if the value before `?.` is `undefined` or `null` and returns `undefined`.

**Further in this article, for brevity, we’ll be saying that something “exists” if it’s not `null` and not `undefined`.**

In other words, `value?.prop`:

- works as `value.prop`, if `value` exists,
- otherwise (when `value` is `undefined/null`) it returns `undefined`.

Here’s the safe way to access `user.address.street` using `?.`:

```jsx
let user = {}; // user has no address

alert(user?.address?.street); // undefined (no error)
```

### Short-circuiting

As it was said before, the `?.` immediately stops (“short-circuits”) the evaluation if the left part doesn’t exist.

So, if there are any further function calls or side effects, they don’t occur.

For instance:

```jsx
let user = null;
let x = 0;

user?.sayHi(x++); // no "sayHi", so the execution doesn't reach x++

alert(x); // 0, value not incremented
```

### At the End

he optional chaining `?.` syntax has three forms:

1. `obj?.prop` – returns `obj.prop` if `obj` exists, otherwise `undefined`.
2. `obj?.[prop]` – returns `obj[prop]` if `obj` exists, otherwise `undefined`.
3. `obj.method?.()` – calls `obj.method()` if `obj.method` exists, otherwise returns `undefined`.

As we can see, all of them are straightforward and simple to use. The `?.` checks the left part for `null/undefined` and allows the evaluation to proceed if it’s not so.

A chain of `?.` allows to safely access nested properties.

Still, we should apply `?.` carefully, only where it’s acceptable that the left part doesn’t exist. So that it won’t hide programming errors from us, if they occur.
