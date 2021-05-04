# 08-Object properties configuration

# Property getters and setters

```jsx
let obj = {
  *get propName()* {
    // getter, the code executed on getting obj.propName
  },

  *set propName(value)* {
    // setter, the code executed on setting obj.propName = value
  }
};
```

The getter works when `obj.propName` is read, the setter – when it is assigned.

---

we want to add a `fullName` property, that should be `"John Smith"`. Of course, we don’t want to copy-paste existing information, so we can implement it as an accessor:

```jsx
let user = {
  name: "John",
  surname: "Smith",

  *get fullName() {
    return `${this.name} ${this.surname}`;
  }*};

*alert(user.fullName); // John Smith*
```

---

```jsx
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  *set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }*};

// set fullName is executed with the given value.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

As the result, we have a “virtual” property `fullName`. It is readable and writable.