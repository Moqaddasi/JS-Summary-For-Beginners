# 03-Data types

# Methods of primitives

There are 7 primitive types: string, number, bigint, boolean, symbol, null and undefined.

## At the end

- Primitives except `null` and `undefined` provide many helpful methods. We will study those in the upcoming chapters.
- Formally, these methods work via temporary objects, but JavaScript engines are well tuned to optimize that internally, so they are not expensive to call.

# Numbers

To write numbers with many zeroes:

- Append `"e"` with the zeroes count to the number. Like: `123e6` is the same as `123` with 6 zeroes `123000000`.
- A negative number after `"e"` causes the number to be divided by 1 with given zeroes. E.g. `123e-6` means `0.000123` (`123` millionths).

For different numeral systems:

- Can write numbers directly in hex (`0x`), octal (`0o`) and binary (`0b`) systems.
- `parseInt(str, base)` parses the string `str` into an integer in numeral system with given `base`, `2 ≤ base ≤ 36`.
- `num.toString(base)` converts a number to a string in the numeral system with the given `base`.

For converting values like `12pt` and `100px` to a number:

- Use `parseInt/parseFloat` for the “soft” conversion, which reads a number from a string and then returns the value they could read before the error.

For fractions:

- Round using `Math.floor`, `Math.ceil`, `Math.trunc`, `Math.round` or `num.toFixed(precision)`.
- Make sure to remember there’s a loss of precision when working with fractions.

More mathematical functions:

- See the [Math](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math) object when you need them. The library is very small, but can cover basic needs.

# Strings

- There are 3 types of quotes. Backticks allow a string to span multiple lines and embed expressions `${…}`.
- Strings in JavaScript are encoded using UTF-16.
- We can use special characters like `\n` and insert letters by their Unicode using `\u...`.
- To get a character, use: `[]`.
- To get a substring, use: `slice` or `substring`.
- To lowercase/uppercase a string, use: `toLowerCase/toUpperCase`.
- To look for a substring, use: `indexOf`, or `includes/startsWith/endsWith` for simple checks.
- To compare strings according to the language, use: `localeCompare`, otherwise they are compared by character codes.

There are several other helpful methods in strings:

- `str.trim()` – removes (“trims”) spaces from the beginning and end of the string.
- `str.repeat(n)` – repeats the string `n` times.
- …and more to be found in the [manual](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String).

# Arrays

There are two syntaxes for creating an empty array:

```jsx
let arr = new Array();
let arr = [];
```

We can get an element by its number in square brackets:

```jsx
let fruits = ["Apple", "Orange", "Plum"];

alert( fruits[0] ); // Apple
alert( fruits[1] ); // Orange
alert( fruits[2] ); // Plum
```

The total count of the elements in the array is its length

## Methods pop/push, shift/unshift

A [queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) is one of the most common uses of an array. In computer science, this means an ordered collection of elements which supports two operations:

- `push` appends an element to the end.
- `shift` get an element from the beginning, advancing the queue, so that the 2nd element becomes the 1st.

There’s another use case for arrays – the data structure named [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)).

It supports two operations:

- `push` adds an element to the end.
- `pop` takes an element from the end.

So new elements are added or taken always from the “end”.

**Methods that work with the end of the array:**

**pop** 

Extracts the last element of the array and returns it:

```jsx

let fruits = ["Apple", "Orange", "Pear"];

alert( fruits.pop() ); // remove "Pear" and alert it

alert( fruits ); // Apple, Orange
```

**`push`**

Append the element to the end of the array:

```jsx

let fruits = ["Apple", "Orange"];

fruits.push("Pear");

alert( fruits ); // Apple, Orange, Pear
```

Methods that work with the beginning of the array:

**`shift`**

Extracts the first element of the array and returns it:

```jsx
let fruits = ["Apple", "Orange", "Pear"];

alert( fruits.shift() ); // remove Apple and alert it

alert( fruits ); // Orange, Pear
```

**`unshift`**

Add the element to the beginning of the array:

```jsx
let fruits = ["Orange", "Pear"];

fruits.unshift('Apple');

alert( fruits ); // Apple, Orange, Pear
```

Methods `push` and `unshift` can add multiple elements at once:

```jsx
let fruits = ["Apple"];

fruits.push("Orange", "Peach");
fruits.unshift("Pineapple", "Lemon");

// ["Pineapple", "Lemon", "Apple", "Orange", "Peach"]
alert( fruits );
```

## Performance

Methods push/pop run fast, while shift/unshift are slow.

## Loops

for arrays there is another form of loop, `for..of`:

```jsx
let fruits = ["Apple", "Orange", "Plum"];

// iterates over array elements
for (let fruit of fruits) {
  alert( fruit );
}
```

### new Array()

There is one more syntax to create an array:

```jsx
let arr = *new Array*("Apple", "Pear", "etc");
```

It’s rarely used, because square brackets `[]` are shorter. Also there’s a tricky feature with it.

If `new Array` is called with a single argument which is a number, then it creates an array *without items, but with the given length*.

Let’s see how one can shoot themself in the foot:

```jsx
let arr = new Array(2); // will it create an array of [2] ?

alert( arr[0] ); // undefined! no elements.

alert( arr.length ); // length 2
```

In the code above, `new Array(number)` has all elements `undefined`.

To evade such surprises, we usually use square brackets, unless we really know what we’re doing.

## toString

Arrays have their own implementation of `toString` method that returns a comma-separated list of elements.

For instance:

```jsx
let arr = [1, 2, 3];

alert( arr ); // 1,2,3
alert( String(arr) === '1,2,3' ); // true
```

## At The End

Array is a special kind of object, suited to storing and managing ordered data items.

- The declaration:

    ```jsx
    // square brackets (usual)
    let arr = [item1, item2...];

    // new Array (exceptionally rare)
    let arr = new Array(item1, item2...);
    ```

    The call to `new Array(number)` creates an array with the given length, but without elements.

- The `length` property is the array length or, to be precise, its last numeric index plus one. It is auto-adjusted by array methods.
- If we shorten `length` manually, the array is truncated.

We can use an array as a deque with the following operations:

- `push(...items)` adds `items` to the end.
- `pop()` removes the element from the end and returns it.
- `shift()` removes the element from the beginning and returns it.
- `unshift(...items)` adds `items` to the beginning.

To loop over the elements of the array:

- `for (let i=0; i<arr.length; i++)` – works fastest, old-browser-compatible.
- `for (let item of arr)` – the modern syntax for items only,
- `for (let i in arr)` – never use.

To compare arrays, don’t use the `==` operator (as well as `>`, `<` and others), as they have no special treatment for arrays. They handle them as any objects, and it’s not what we usually want.

Instead you can use `for..of` loop to compare arrays item-by-item.

We will continue with arrays and study more methods to add, remove, extract elements and sort arrays in the next chapter [Array methods](https://javascript.info/array-methods).

# Array methods

### Add/remove items

We already know methods that add and remove items from the beginning or the end:

- `arr.push(...items)` – adds items to the end,
- `arr.pop()` – extracts an item from the end,
- `arr.shift()` – extracts an item from the beginning,
- `arr.unshift(...items)` – adds items to the beginning.
- 

## splice

Let’s start with the deletion:

```jsx
let arr = ["I", "study", "JavaScript"];

*arr.splice(1, 1); // from index 1 remove 1 element*alert( arr ); // ["I", "JavaScript"]
```

In the next example we remove 3 elements and replace them with the other two:

```jsx
let arr = [*"I", "study", "JavaScript",* "right", "now"];

// remove 3 first elements and replace them with another
arr.splice(0, 3, "Let's", "dance");

alert( arr ) // now [*"Let's", "dance"*, "right", "now"]
```

## slice

The method [arr.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) is much simpler than similar-looking `arr.splice`.

```jsx
arr.slice([start], [end])
```

It returns a new array copying to it all items from index `start` to `end` (not including `end`). Both `start` and `end` can be negative, in that case position from array end is assumed.

It’s similar to a string method `str.slice`, but instead of substrings it makes subarrays.

For instance:

```jsx
let arr = ["t", "e", "s", "t"];

alert( arr.slice(1, 3) ); // e,s (copy from 1 to 3)

alert( arr.slice(-2) ); // s,t (copy from -2 till the end)
```

## concat

The method [arr.concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) creates a new array that includes values from other arrays and additional items.

The syntax is:

```jsx
arr.concat(arg1, arg2...)
```

The result is a new array containing items from arr, then arg1, arg2 etc.

## Iterate: forEach

The arr.forEach method allows to run a function for every element of the array.

```jsx
arr.forEach(function(item, index, array) {
  // ... do something with item
});
```

## Searching in array

## indexOf/lastIndexOf and includes

The methods [arr.indexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf), [arr.lastIndexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf) and [arr.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) have the same syntax and do essentially the same as their string counterparts, but operate on items instead of characters:

- `arr.indexOf(item, from)` – looks for `item` starting from index `from`, and returns the index where it was found, otherwise `1`.
- `arr.lastIndexOf(item, from)` – same, but looks for from right to left.
- `arr.includes(item, from)` – looks for `item` starting from index `from`, returns `true` if found.

```jsx
let arr = [1, 0, false];

alert( arr.indexOf(0) ); // 1
alert( arr.indexOf(false) ); // 2
alert( arr.indexOf(null) ); // -1

alert( arr.includes(1) ); // true
```

## find and findIndex

Imagine we have an array of objects. How do we find an object with the specific condition?

Here the [arr.find(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) method comes in handy.

The syntax is:

```jsx
let result = arr.find(function(item, index, array) {
  // if true is returned, item is returned and iteration is stopped
  // for falsy scenario returns undefined
});
```

The function is called for elements of the array, one after another:

- `item` is the element.
- `index` is its index.
- `array` is the array itself.

If it returns `true`, the search is stopped, the `item` is returned. If nothing found, `undefined` is returned.

For example, we have an array of users, each with the fields `id` and `name`. Let’s find the one with `id == 1`:

```jsx
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John
```

## filter

The `find` method looks for a single (first) element that makes the function return `true`.

If there may be many, we can use [arr.filter(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

The syntax is similar to `find`, but `filter` returns an array of all matching elements:

```jsx
let results = arr.filter(function(item, index, array) {
  // if true item is pushed to results and the iteration continues
  // returns empty array if nothing found
});
```

For instance:

```jsx
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

// returns array of the first two users
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2
```

## Transform an array

## map

The [arr.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method is one of the most useful and often used.

It calls the function for each element of the array and returns the array of results.

The syntax is:

```jsx
let result = arr.map(function(item, index, array) {
  // returns the new value instead of item
});
```

For instance, here we transform each element into its length:

```jsx
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6
```

## sort(fn)

The call to [arr.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) sorts the array *in place*, changing its element order.

It also returns the sorted array, but the returned value is usually ignored, as `arr` itself is modified.

For instance:

```jsx
let arr = [ 1, 2, 15 ];

// the method reorders the content of arr
arr.sort();

alert( arr );  // *1, 15, 2*
```

The items are sorted as strings by default.

For instance, to sort as numbers:

```jsx
function compareNumeric(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}

let arr = [ 1, 2, 15 ];

*arr.sort(compareNumeric);*alert(arr);  // *1, 2, 15*
```

## reverse

The method [arr.reverse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) reverses the order of elements in `arr`.

For instance:

```jsx
let arr = [1, 2, 3, 4, 5];
arr.reverse();

alert( arr ); // 5,4,3,2,1
```

## split and join

we split by a comma followed by space:

```jsx
let names = 'Bilbo, Gandalf, Nazgul';

let arr = names.split(', ');

for (let name of arr) {
  alert( `A message to ${name}.` ); // A message to Bilbo  (and other names)
```

## At the end

A cheat sheet of array methods:

- To add/remove elements:
    - `push(...items)` – adds items to the end,
    - `pop()` – extracts an item from the end,
    - `shift()` – extracts an item from the beginning,
    - `unshift(...items)` – adds items to the beginning.
    - `splice(pos, deleteCount, ...items)` – at index `pos` deletes `deleteCount` elements and inserts `items`.
    - `slice(start, end)` – creates a new array, copies elements from index `start` till `end` (not inclusive) into it.
    - `concat(...items)` – returns a new array: copies all members of the current one and adds `items` to it. If any of `items` is an array, then its elements are taken.
- To search among elements:
    - `indexOf/lastIndexOf(item, pos)` – look for `item` starting from position `pos`, return the index or `1` if not found.
    - `includes(value)` – returns `true` if the array has `value`, otherwise `false`.
    - `find/filter(func)` – filter elements through the function, return first/all values that make it return `true`.
    - `findIndex` is like `find`, but returns the index instead of a value.
- To iterate over elements:
    - `forEach(func)` – calls `func` for every element, does not return anything.
- To transform the array:
    - `map(func)` – creates a new array from results of calling `func` for every element.
    - `sort(func)` – sorts the array in-place, then returns it.
    - `reverse()` – reverses the array in-place, then returns it.
    - `split/join` – convert a string to array and back.
    - `reduce/reduceRight(func, initial)` – calculate a single value over the array by calling `func` for each element and passing an intermediate result between the calls.
- Additionally:
    - `Array.isArray(arr)` checks `arr` for being an array.

Please note that methods `sort`, `reverse` and `splice` modify the array itself.

These methods are the most used ones, they cover 99% of use cases. But there are few others:

- [arr.some(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)/[arr.every(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every) check the array.

    The function `fn` is called on each element of the array similar to `map`. If any/all results are `true`, returns `true`, otherwise `false`.

    These methods behave sort of like `||` and `&&` operators: if `fn` returns a truthy value, `arr.some()` immediately returns `true` and stops iterating over the rest of items; if `fn` returns a falsy value, `arr.every()` immediately returns `false` and stops iterating over the rest of items as well.

    We can use `every` to compare arrays:

    ```jsx
    function arraysEqual(arr1, arr2) {
      return arr1.length === arr2.length && arr1.every((value, index) => value === arr2[index]);
    }

    alert( arraysEqual([1, 2], [1, 2])); // true
    ```

- [arr.fill(value, start, end)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) – fills the array with repeating `value` from index `start` to `end`.
- [arr.copyWithin(target, start, end)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) – copies its elements from position `start` till position `end` into *itself*, at position `target` (overwrites existing).
- [arr.flat(depth)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)/[arr.flatMap(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) create a new flat array from a multidimensional array.

For the full list, see the [manual](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).

From the first sight it may seem that there are so many methods, quite difficult to remember. But actually that’s much easier.

Look through the cheat sheet just to be aware of them. Then solve the tasks of this chapter to practice, so that you have experience with array methods.

Afterwards whenever you need to do something with an array, and you don’t know how – come here, look at the cheat sheet and find the right method. Examples will help you to write it correctly. Soon you’ll automatically remember the methods, without specific efforts from your side.

# iterable

Objects that can be used in `for..of` are called *iterable*.

- Technically, iterables must implement the method named `Symbol.iterator`.
    - The result of `obj[Symbol.iterator]()` is called an *iterator*. It handles further iteration process.
    - An iterator must have the method named `next()` that returns an object `{done: Boolean, value: any}`, here `done:true` denotes the end of the iteration process, otherwise the `value` is the next value.
- The `Symbol.iterator` method is called automatically by `for..of`, but we also can do it directly.
- Built-in iterables like strings or arrays, also implement `Symbol.iterator`.
- String iterator knows about surrogate pairs.

Objects that have indexed properties and `length` are called *array-like*. Such objects may also have other properties and methods, but lack the built-in methods of arrays.

If we look inside the specification – we’ll see that most built-in methods assume that they work with iterables or array-likes instead of “real” arrays, because that’s more abstract.

`Array.from(obj[, mapFn, thisArg])` makes a real `Array` from an iterable or array-like `obj`, and we can then use array methods on it. The optional arguments `mapFn` and `thisArg` allow us to apply a function to each item.

# Map and Set

## Map

[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) is a collection of keyed data items, just like an `Object`. But the main difference is that `Map` allows keys of any type.

Methods and properties are:

- `new Map()` – creates the map.
- `map.set(key, value)` – stores the value by the key.
- `map.get(key)` – returns the value by the key, `undefined` if `key` doesn’t exist in map.
- `map.has(key)` – returns `true` if the `key` exists, `false` otherwise.
- `map.delete(key)` – removes the value by the key.
- `map.clear()` – removes everything from the map.
- `map.size` – returns the current element count.

For instance:

```jsx
let map = new Map();

map.set('1', 'str1');   // a string key
map.set(1, 'num1');     // a numeric key
map.set(true, 'bool1'); // a boolean key

// remember the regular Object? it would convert keys to string
// Map keeps the type, so these two are different:
alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```

Map can also use objects as keys.

```jsx
let john = { name: "John" };

// for every user, let's store their visits count
let visitsCountMap = new Map();

// john is the key for the map
visitsCountMap.set(john, 123);

alert( visitsCountMap.get(john) ); // 123
```

Every `map.set` call returns the map itself, so we can “chain” the calls:

```jsx
map.set('1', 'str1')
  .set(1, 'num1')
  .set(true, 'bool1');
```

## Iteration over Map

For looping over a `map`, there are 3 methods:

- `map.keys()` – returns an iterable for keys,
- `map.values()` – returns an iterable for values,
- `map.entries()` – returns an iterable for entries `[key, value]`, it’s used by default in `for..of`.

For instance:

```jsx
let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',    50]
]);

// iterate over keys (vegetables)
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // cucumber, tomatoes, onion
}

// iterate over values (amounts)
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// iterate over [key, value] entries
for (let entry of recipeMap) { // the same as of recipeMap.entries()
  alert(entry); // cucumber,500 (and so on)
}
```

# Object.keys, values, entries

For plain objects, the following methods are available:

- [Object.keys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) – returns an array of keys.
- [Object.values(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values) – returns an array of values.
- [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) – returns an array of `[key, value]` pairs.

```jsx
let user = {
  name: "John",
  age: 30
};
```

```jsx
Object.keys(user) = ["name", "age"]
```

```jsx
Object.values(user) = ["John", 30]
```

```jsx
	Object.entries(user) = [ ["name","John"], ["age",30] ]
```

# Destructuring assignment

## The rest ‘…’

.If we’d like also to gather all that follows – we can add one more parameter that gets “the rest” using three dots "...":

```jsx
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

// rest is array of items, starting from the 3rd one
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
```

## The rest pattern “…”

We can use the rest pattern, just like we did with arrays. It’s not supported by some older browsers (IE, use Babel to polyfill it), but works in modern ones.

```jsx
let options = {
  title: "Menu",
  height: 200,
  width: 100
};

// title = property named title
// rest = object with the rest of properties
let {title, ...rest} = options;

// now title="Menu", rest={height: 200, width: 100}
alert(rest.height);  // 200
alert(rest.width);   // 100
```

# Date and time

- Date and time in JavaScript are represented with the [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object. We can’t create “only date” or “only time”: `Date` objects always carry both.
- Months are counted from zero (yes, January is a zero month).
- Days of week in `getDay()` are also counted from zero (that’s Sunday).
- `Date` auto-corrects itself when out-of-range components are set. Good for adding/subtracting days/months/hours.
- Dates can be subtracted, giving their difference in milliseconds. That’s because a `Date` becomes the timestamp when converted to a number.
- Use `Date.now()` to get the current timestamp fast.

# JSON methods, toJSON

```jsx
JSON.stringify to convert objects into JSON.
JSON.parse to convert JSON back into an object.
For instance, here we JSON.stringify a student:

let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

let json = JSON.stringify(student);

alert(typeof json); // we've got a string!

alert(json);
/* JSON-encoded object:
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/
```

`JSON.stringify` can be applied to primitives as well.

JSON supports following data types:

- Objects `{ ... }`
- Arrays `[ ... ]`
- Primitives:
    - strings,
    - numbers,
    - boolean values `true/false`,
    - `null`.

    ## Excluding and transforming: replacer

    The full syntax of `JSON.stringify` is:

    ```jsx
    let json = JSON.stringify(value[, replacer, space])
    ```

    **value**A value to encode.**replacer**Array of properties to encode or a mapping function `function(key, value)`.**space**Amount of space to use for formatting

    ```jsx
    let room = {
      number: 23
    };

    let meetup = {
      title: "Conference",
      participants: [{name: "John"}, {name: "Alice"}],
      place: room // meetup references room
    };

    room.occupiedBy = meetup; // room references meetup

    alert( JSON.stringify(meetup, ['title', 'participants']) );
    // {"title":"Conference","participants":[{},{}]}
    ```

    ## Custom “toJSON”

    Like toString for string conversion, an object may provide method toJSON for to-JSON conversion. JSON.stringify automatically calls it if available.

    ```jsx
    let room = {
      number: 23,
      toJSON() {
        return this.number;
      }
    };

    let meetup = {
      title: "Conference",
      room
    };

    alert( JSON.stringify(room) ); // 23

    alert( JSON.stringify(meetup) );
    /*
      {
        "title":"Conference",
        "room": 23
      }
    */
    ```

    ## JSON.parse

    To decode a JSON-string, we need another method named [JSON.parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse).

    The syntax:

    ```jsx
    let value = JSON.parse(str, [reviver]);
    ```

    **str**

    JSON-string to parse.

    **reviver**

    Optional function(key,value) that will be called for each `(key, value)` pair and can transform the value.

    ```jsx
    // stringified array
    let numbers = "[0, 1, 2, 3]";

    numbers = JSON.parse(numbers);

    alert( numbers[1] ); // 1
    ```