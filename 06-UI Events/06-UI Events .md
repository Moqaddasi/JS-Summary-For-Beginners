# 06-UI Events

attention : You should read more about events and practice more

# 6.1 Mouse events

Mouse events have the following properties:

- Button: `button`.
- Modifier keys (`true` if pressed): `altKey`, `ctrlKey`, `shiftKey` and `metaKey` (Mac).

  - If you want to handle , then don’t forget Mac users, they usually use , so it’s better to check `if (e.metaKey || e.ctrlKey)`.

    Ctrl

    Cmd

- Window-relative coordinates: `clientX/clientY`.
- Document-relative coordinates: `pageX/pageY`.

The default browser action of `mousedown` is text selection, if it’s not good for the interface, then it should be prevented.

In the next chapter we’ll see more details about events that follow pointer movement and how to track element changes under it.

# 6.2 Moving the mouse: mouseover/out, mouseenter/leave

We covered events `mouseover`, `mouseout`, `mousemove`, `mouseenter` and `mouseleave`.

These things are good to note:

- A fast mouse move may skip intermediate elements.
- Events `mouseover/out` and `mouseenter/leave` have an additional property: `relatedTarget`. That’s the element that we are coming from/to, complementary to `target`.

Events `mouseover/out` trigger even when we go from the parent element to a child element. The browser assumes that the mouse can be only over one element at one time – the deepest one.

Events `mouseenter/leave` are different in that aspect: they only trigger when the mouse comes in and out the element as a whole. Also they do not bubble.
