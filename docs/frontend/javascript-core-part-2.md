# 4. JavaScript Core - Part 2

DOM Manipulation, Events, Form Validation & a Mini Project

Today you will start interacting with the browser:

- selecting elements
- changing UI using JavaScript
- handling events
- validating forms
- understanding bubbling → capture → preventDefault → stopPropagation

This is where JS becomes _visible_ in the UI.

---

# 1. DOM Manipulation

The **DOM (Document Object Model)** is how JavaScript "sees" your webpage.

Every HTML element becomes a JavaScript object that you can:

- select
- modify
- style
- delete
- listen for events on

---

## 1.1 Selecting Elements

### Most common methods:

```js
document.getElementById("title");
document.querySelector(".btn");
document.querySelectorAll("p");
```

### Example:

```html
<h1 id="title">Welcome</h1>
<button id="change">Change Title</button>
```

```js
const title = document.getElementById("title");
const btn = document.getElementById("change");

btn.addEventListener("click", () => {
  title.textContent = "Title Changed!";
});
```

## 1.2 Changing Styles

```js
title.style.color = "blue";
title.style.fontSize = "30px";
```

## 1.3 Changing HTML

```js
title.innerHTML = "<span style='color:red'>Updated!</span>";
```

---

# 2. DOM Events

Events are actions — clicks, typing, submitting forms, scrolling, etc.

## 2.1 Adding Event Listeners

### Example: Click Event

```js
btn.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

## 2.2 Input Event (useful for forms)

```js
const input = document.getElementById("username");

input.addEventListener("input", () => {
  console.log("You typed:", input.value);
});
```

---

# 3. Event Bubbling (Important!)

## What is bubbling?

When you click an element:

1. event → runs on the element
2. then travels upward to its parent
3. then to the parent's parent
4. until `<body>`

## Simple Example:

```html
<div id="parent">
  <button id="child">Click</button>
</div>
```

```js
document.getElementById("parent").addEventListener("click", () => {
  console.log("Parent clicked");
});

document.getElementById("child").addEventListener("click", () => {
  console.log("Child clicked");
});
```

Clicking the button logs:

```
Child clicked
Parent clicked
```

That's bubbling.

## 3.1 How to STOP bubbling

Use:

```js
event.stopPropagation();
```

### Example:

```js
document.getElementById("child").addEventListener("click", (event) => {
  event.stopPropagation();
  console.log("Only child clicked");
});
```

## 3.2 preventDefault (Very Important for Forms)

### What preventDefault does:

Stops browser's default action:

- form submission
- page refresh
- link navigation
- right-click menu

### Example:

```js
form.addEventListener("submit", (event) => {
  event.preventDefault();
  console.log("Form stopped — will validate first");
});
```

---

# 4. Form Handling & Validation (Vanilla JS)

We will create a simple and minimal form.

## 4.1 Example Form (HTML)

```html
<form id="loginForm">
  <input type="text" id="username" placeholder="Username" />
  <input type="password" id="password" placeholder="Password" />
  <button type="submit">Login</button>
</form>
```

## 4.2 Validate Inputs (Native Alert Approach)

```js
const form = document.getElementById("loginForm");

form.addEventListener("submit", function (event) {
  event.preventDefault(); // stop page refresh

  const user = document.getElementById("username").value.trim();
  const pass = document.getElementById("password").value.trim();

  if (!user || !pass) {
    alert("All fields are required.");
    return;
  }

  if (pass.length < 6) {
    alert("Password must be at least 6 characters.");
    return;
  }

  alert("Login successful!");
});
```

### Why this is good for beginners?

- native alert = simple
- immediate feedback
- minimal code
- no UI distractions

---

# 5. Mini Project — Simple Calculator (+ Form Validation)

A small, clean project to connect DOM + events + user input.

## 5.1 HTML

```html
<div class="calculator">
  <input type="number" id="num1" placeholder="Number 1" />
  <input type="number" id="num2" placeholder="Number 2" />
  <button id="add">Add</button>
  <p id="result">Result:</p>
</div>
```

## 5.2 JavaScript (With Validation)

```js
const n1 = document.getElementById("num1");
const n2 = document.getElementById("num2");
const btn = document.getElementById("add");
const res = document.getElementById("result");

btn.addEventListener("click", function () {
  const a = n1.value.trim();
  const b = n2.value.trim();

  if (!a || !b) {
    alert("Please enter both numbers.");
    return;
  }

  const sum = Number(a) + Number(b);
  res.textContent = "Result: " + sum;
});
```

## What You Learned Here:

- selecting DOM elements
- listening to click events
- reading input values
- validating input
- updating DOM text dynamically
