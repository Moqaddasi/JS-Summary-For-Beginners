# 04-DOM

# attention :

You should read more about DOM

# Walking the DOM

Given a DOM node, we can go to its immediate neighbors using navigation properties.

There are two main sets of them:

- For all nodes: `parentNode`, `childNodes`, `firstChild`, `lastChild`, `previousSibling`, `nextSibling`.
- For element nodes only: `parentElement`, `children`, `firstElementChild`, `lastElementChild`, `previousElementSibling`, `nextElementSibling`.

# Searching: getElement*, querySelector*

## document.getElementById or just id

If an element has the `id` attribute, we can get the element using the method `document.getElementById(id)`, no matter where it is.

For instance:

```jsx
<div id="elem"><div id="elem-content">Element</div></div><script>
  // get the element
  *let elem = document.getElementById('elem');*// make its background red
  elem.style.background = 'red';
</script>
```

Also, there’s a global variable named by `id` that references the element:

```jsx
<div id="*elem*"><div id="*elem-content*">Element</div></div><script>
  // elem is a reference to DOM-element with id="elem"
  elem.style.background = 'red';

  // id="elem-content" has a hyphen inside, so it can't be a variable name
  // ...but we can access it using square brackets: window['elem-content']
</script>
```

## querySelectorAll

By far, the most versatile method, `elem.querySelectorAll(css)` returns all elements inside `elem` matching the given CSS selector.

Here we look for all `<li>` elements that are last children:

```jsx
<ul><li>The</li><li>test</li></ul><ul><li>has</li><li>passed</li></ul><script>
  *let elements = document.querySelectorAll('ul > li:last-child');*for (let elem of elements) {
    alert(elem.innerHTML); // "test", "passed"
  }
</script>
```

## querySelector

The call to `elem.querySelector(css)` returns the first element for the given CSS selector.

In other words, the result is the same as `elem.querySelectorAll(css)[0]`, but the latter is looking for *all* elements and picking one, while `elem.querySelector` just looks for one. So it’s faster and also shorter to write.

## getElementsBy*

There are also other methods to look for nodes by a tag, class, etc.

Today, they are mostly history, as `querySelector` is more powerful and shorter to write.

So here we cover them mainly for completeness, while you can still find them in the old scripts.

- `elem.getElementsByTagName(tag)` looks for elements with the given tag and returns the collection of them. The `tag` parameter can also be a star `"*"` for “any tags”.
- `elem.getElementsByClassName(className)` returns elements that have the given CSS class.
- `document.getElementsByName(name)` returns elements with the given `name` attribute, document-wide. Very rarely used.

Let’s find all `input` tags inside the table:

```jsx
<table id="table"><tr><td>Your age:</td><td><label><input type="radio" name="age" value="young" checked> less than 18
      </label><label><input type="radio" name="age" value="mature"> from 18 to 50
      </label><label><input type="radio" name="age" value="senior"> more than 60
      </label></td></tr></table><script>
  *let inputs = table.getElementsByTagName('input');*for (let input of inputs) {
    alert( input.value + ': ' + input.checked );
  }
</script>
```

## At The End

There are 6 main methods to search for nodes in DOM:

[Untitled](https://www.notion.so/f92e596a1a5b46e8abf8956661e5ce17)

By far the most used are `querySelector` and `querySelectorAll`, but `getElement(s)By*` can be sporadically helpful or found in the old scripts.

Besides that:

- There is `elem.matches(css)` to check if `elem` matches the given CSS selector.
- There is `elem.closest(css)` to look for the nearest ancestor that matches the given CSS-selector. The `elem` itself is also checked.

And let’s mention one more method here to check for the child-parent relationship, as it’s sometimes useful:

- `elemA.contains(elemB)` returns true if `elemB` is inside `elemA` (a descendant of `elemA`) or when `elemA==elemB`.

# Node properties: type, tag and contents

Each DOM node belongs to a certain class. The classes form a hierarchy. The full set of properties and methods come as the result of inheritance.

Main DOM node properties are:

**`nodeType`**We can use it to see if a node is a text or an element node. It has a numeric value: `1` for elements,`3` for text nodes, and a few others for other node types. Read-only.

**`nodeName/tagName`**For elements, tag name (uppercased unless XML-mode). For non-element nodes `nodeName` describes what it is. Read-only.

**`innerHTML`**The HTML content of the element. Can be modified.

**`outerHTML`**The full HTML of the element. A write operation into `elem.outerHTML` does not touch `elem` itself. Instead it gets replaced with the new HTML in the outer context.

**`nodeValue/data`**The content of a non-element node (text, comment). These two are almost the same, usually we use `data`. Can be modified.

**`textContent`**The text inside the element: HTML minus all `<tags>`. Writing into it puts the text inside the element, with all special characters and tags treated exactly as text. Can safely insert user-generated text and protect from unwanted HTML insertions.

**`hidden`**When set to `true`, does the same as CSS `display:none`.

DOM nodes also have other properties depending on their class. For instance, `<input>` elements (`HTMLInputElement`) support `value`, `type`, while `<a>` elements (`HTMLAnchorElement`) support `href` etc. Most standard HTML attributes have a corresponding DOM property.

20However, HTML attributes and DOM properties are not always the same, as we’ll see in the next chapter.

# Attributes and properties

- Attributes – is what’s written in HTML.
- Properties – is what’s in DOM objects.

A small comparison:

[Untitled](https://www.notion.so/a6212ce001724ee7a0645ab1abb0982b)

Methods to work with attributes are:

- `elem.hasAttribute(name)` – to check for existence.
- `elem.getAttribute(name)` – to get the value.
- `elem.setAttribute(name, value)` – to set the value.
- `elem.removeAttribute(name)` – to remove the attribute.
- `elem.attributes` is a collection of all attributes.

For most situations using DOM properties is preferable. We should refer to attributes only when DOM properties do not suit us, when we need exactly attributes, for instance:

- We need a non-standard attribute. But if it starts with `data-`, then we should use `dataset`.
- We want to read the value “as written” in HTML. The value of the DOM property may be different, for instance the `href` property is always a full URL, and we may want to get the “original” value.

# Modifying the document

- Methods to create new nodes:
    - `document.createElement(tag)` – creates an element with the given tag,
    - `document.createTextNode(value)` – creates a text node (rarely used),
    - `elem.cloneNode(deep)` – clones the element, if `deep==true` then with all descendants.
- Insertion and removal:
    - `node.append(...nodes or strings)` – insert into `node`, at the end,
    - `node.prepend(...nodes or strings)` – insert into `node`, at the beginning,
    - `node.before(...nodes or strings)` –- insert right before `node`,
    - `node.after(...nodes or strings)` –- insert right after `node`,
    - `node.replaceWith(...nodes or strings)` –- replace `node`.
    - `node.remove()` –- remove the `node`.

    Text strings are inserted “as text”.

- There are also “old school” methods:
    - `parent.appendChild(node)`
    - `parent.insertBefore(node, nextSibling)`
    - `parent.removeChild(node)`
    - `parent.replaceChild(newElem, node)`

    All these methods return `node`.

- Given some HTML in `html`, `elem.insertAdjacentHTML(where, html)` inserts it depending on the value of `where`:
    - `"beforebegin"` – insert `html` right before `elem`,
    - `"afterbegin"` – insert `html` into `elem`, at the beginning,
    - `"beforeend"` – insert `html` into `elem`, at the end,
    - `"afterend"` – insert `html` right after `elem`.

    Also there are similar methods, `elem.insertAdjacentText` and `elem.insertAdjacentElement`, that insert text strings and elements, but they are rarely used.

- To append HTML to the page before it has finished loading:
    - `document.write(html)`

    After the page is loaded such a call erases the document. Mostly seen in old scripts.

    # Styles and classes

    To manage classes, there are two DOM properties:

    - `className` – the string value, good to manage the whole set of classes.
    - `classList` – the object with methods `add/remove/toggle/contains`, good for individual classes.

    To change the styles:

    - The `style` property is an object with camelCased styles. Reading and writing to it has the same meaning as modifying individual properties in the `"style"` attribute. To see how to apply `important` and other rare stuff – there’s a list of methods at [MDN](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleDeclaration).
    - The `style.cssText` property corresponds to the whole `"style"` attribute, the full string of styles.

    To read the resolved styles (with respect to all classes, after all CSS is applied and final values are calculated):

    - The `getComputedStyle(elem, [pseudo])` returns the style-like object with them. Read-only.