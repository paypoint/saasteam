# 3. JavaScript Core - Part 1

Build a strong foundation in JavaScript fundamentals

Today you will learn the building blocks of JavaScript — variables, data types, operators, control flow, functions, arrays, and objects.

This is the _core logic layer_ behind everything in frontend development.

Follow the examples and practice each concept as you read.

---

# 1. JavaScript Fundamentals

JavaScript is the language of the browser. Everything — UI interactions, data processing, DOM updates — comes from JS.

---

## 1.1 Variables — let, const, var

In modern JavaScript:

### ✔ Use `let` for variables that change

### ✔ Use `const` when the value should not change

### ❌ Avoid `var` (old, unpredictable)

### Example:

```js
let age = 25;
age = 26; // valid

const name = "Chetan";
// name = "New" ❌ error
```

## 1.2 Data Types

Primitive Types:

- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `bigint`
- `symbol`

### Example:

```js
let title = "Frontend Bootcamp"; // string
let score = 90; // number
let isActive = true; // boolean
let noValue = null; // null
let notSet; // undefined
```

## 1.3 Operators

### Arithmetic

```js
let sum = 10 + 5;
let diff = 10 - 5;
let mul = 10 * 5;
let div = 10 / 5;
```

### Comparison

```js
5 > 2; // true
5 === "5"; // false (strict equality)
5 == "5"; // true  (avoid)
```

### Logical

```js
true && false; // false
true || false; // true
!true; // false
```

---

# 2. Control Flow

Control flow decides how your program executes.

## 2.1 If / Else

```js
let marks = 85;

if (marks >= 90) {
  console.log("A Grade");
} else if (marks >= 75) {
  console.log("B Grade");
} else {
  console.log("C Grade");
}
```

## 2.2 Switch (when multiple conditions relate to one value)

```js
let role = "admin";

switch (role) {
  case "admin":
    console.log("Full access");
    break;
  case "user":
    console.log("Limited access");
    break;
  default:
    console.log("No access");
}
```

## 2.3 Loops

### For Loop

```js
for (let i = 1; i <= 5; i++) {
  console.log("Step", i);
}
```

### While Loop

```js
let count = 1;
while (count <= 3) {
  console.log("Count:", count);
  count++;
}
```

---

# 3. Functions

Functions are reusable blocks of code.

## 3.1 Function Declaration

```js
function greet() {
  console.log("Hello Developer!");
}

greet();
```

## 3.2 Function with Parameters

```js
function add(a, b) {
  return a + b;
}

console.log(add(10, 20));
```

## 3.3 Arrow Functions (modern JS)

```js
const multiply = (x, y) => x * y;

console.log(multiply(5, 4));
```

---

# 4. Arrays & Objects

These are the most important data structures in JavaScript.

## 4.1 Arrays (ordered list of values)

### Example:

```js
const fruits = ["Apple", "Banana", "Mango"];

console.log(fruits[0]); // Apple
```

### 4.1.1 Array Methods (you'll use daily)

```js
fruits.push("Orange"); // add to end
fruits.pop(); // remove last
fruits.shift(); // remove first
fruits.unshift("Kiwi"); // add first
fruits.includes("Mango"); // true
```

More powerful methods come on Day 4.

## 4.2 Objects (key-value pairs)

Objects describe anything with properties.

### Example:

```js
const student = {
  name: "Arjun",
  age: 22,
  course: "Frontend",
};

console.log(student.name); // Arjun
```

### 4.2.1 Add / Update / Delete keys

```js
student.city = "Mumbai"; // add
student.age = 23; // update
delete student.course; // delete
```

### 4.2.2 Nested Objects

```js
const user = {
  name: "Riya",
  address: {
    city: "Delhi",
    pin: 110001,
  },
};

console.log(user.address.city);
```

---

# 🧪 Mini Practice Tasks

Try these one by one:

1. Create variables of all types
2. Write a function `isEven` that returns true/false
3. Create an array of 5 movies and print first & last
4. Create an object `car` with model, year, and price
5. Write a loop that prints numbers 1 to 50
6. Add a new key `color` to the car object
