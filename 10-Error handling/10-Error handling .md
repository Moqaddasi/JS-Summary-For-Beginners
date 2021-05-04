# 10-Error handling

# Error handling, "try...catch"

## The “try…catch” syntax

The `try...catch` construct has two main blocks: `try`, and then `catch`:

```jsx
try {

  // code...

} catch (err) {

  // error handling

}
```

It works like this:

1. First, the code in `try {...}` is executed.
2. If there were no errors, then `catch (err)` is ignored: the execution reaches the end of `try` and goes on, skipping `catch`.
3. If an error occurs, then the `try` execution is stopped, and control flows to the beginning of `catch (err)`. The `err` variable (we can use any name for it) will contain an error object with details about what happened.

## Using “try…catch”

Let’s use `try...catch` to handle the error:

```jsx
let json = "{ bad json }";

try {

  *let user = JSON.parse(json); // <-- when an error occurs...*alert( user.name ); // doesn't work

} catch (err) {
  *// ...the execution jumps here
  alert( "Our apologies, the data has errors, we'll try to request it one more time." );
  alert( err.name );
  alert( err.message );*}
```

Error objects have following properties:

- `message` – the human-readable error message.
- `name` – the string with error name (error constructor name).
- `stack` (non-standard, but well-supported) – the stack at the moment of error creation.

If an error object is not needed, we can omit it by using `catch {` instead of `catch (err) {`.

We can also generate our own errors using the `throw` operator. Technically, the argument of `throw` can be anything, but usually it’s an error object inheriting from the built-in `Error` class. More on extending errors in the next chapter.

*Rethrowing* is a very important pattern of error handling: a `catch` block usually expects and knows how to handle the particular error type, so it should rethrow errors it doesn’t know.

Even if we don’t have `try...catch`, most environments allow us to setup a “global” error handler to catch errors that “fall out”. In-browser, that’s `window.onerror`.