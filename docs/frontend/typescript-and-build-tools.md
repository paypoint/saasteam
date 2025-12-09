# 7. TypeScript and Build Tools

Today you will add type safety to your JavaScript:

- Understanding TypeScript and why it matters
- Basic types, interfaces, and type annotations
- Writing type-safe functions
- Working with DOM in TypeScript
- Setting up a TypeScript project

This is where JavaScript becomes safer and more maintainable.

---

## 1. What is TypeScript?

TypeScript is **JavaScript with types**.

### Why TypeScript?

```javascript
// JavaScript - no errors until runtime
function addNumbers(a, b) {
  return a + b;
}

addNumbers(5, "10"); // Returns "510" - unexpected!
addNumbers(5); // Returns NaN - breaks your app!
```

```typescript
// TypeScript - catches errors before running
function addNumbers(a: number, b: number): number {
  return a + b;
}

addNumbers(5, "10"); // ❌ Error: Argument of type 'string' is not assignable to type 'number'
addNumbers(5); // ❌ Error: Expected 2 arguments, but got 1
```

**Benefits:**

- Catch bugs before runtime
- Better autocomplete in VS Code
- Self-documenting code
- Easier refactoring
- Used by big companies (Google, Microsoft, Airbnb)

---

## 2. Try TypeScript Online

**No installation needed! Try these:**

- **TypeScript Playground** (Official): https://www.typescriptlang.org/play
- **StackBlitz**: https://stackblitz.com/fork/typescript
- **CodeSandbox**: https://codesandbox.io/s/typescript

_Note: For the rest of this tutorial, you can practice in any of these online editors._

---

## 3. Basic Types

### 3.1 Primitive Types

```typescript
// String
let username: string = "John";
username = "Jane"; // ✅ OK
username = 123; // ❌ Error

// Number
let age: number = 25;
age = 30; // ✅ OK
age = "30"; // ❌ Error

// Boolean
let isActive: boolean = true;
isActive = false; // ✅ OK
isActive = "true"; // ❌ Error

// Array
let numbers: number[] = [1, 2, 3];
numbers.push(4); // ✅ OK
numbers.push("5"); // ❌ Error

let names: string[] = ["Alice", "Bob"];

// Alternative array syntax
let scores: Array<number> = [90, 85, 95];
```

### 3.2 Any (Avoid this!)

```typescript
let data: any = "hello";
data = 123; // ✅ OK
data = true; // ✅ OK
// any defeats the purpose of TypeScript!
```

### 3.3 Union Types

A variable can be one of several types:

```typescript
let id: string | number;
id = "ABC123"; // ✅ OK
id = 123; // ✅ OK
id = true; // ❌ Error

function printId(id: string | number) {
  console.log("ID:", id);
}

printId("ABC"); // ✅ OK
printId(123); // ✅ OK
printId(true); // ❌ Error
```

### 3.4 Literal Types

```typescript
let status: "active" | "inactive" | "pending";
status = "active"; // ✅ OK
status = "inactive"; // ✅ OK
status = "archived"; // ❌ Error

let direction: "up" | "down" | "left" | "right";
direction = "up"; // ✅ OK
direction = "forward"; // ❌ Error
```

---

## 4. Objects and Interfaces

### 4.1 Object Type Annotation

```typescript
let user: { name: string; age: number; email: string } = {
  name: "John",
  age: 30,
  email: "john@example.com",
};

// Missing property
let user2: { name: string; age: number } = {
  name: "Jane", // ❌ Error: Property 'age' is missing
};
```

### 4.2 Interfaces (Better Way)

```typescript
interface User {
  name: string;
  age: number;
  email: string;
}

let user: User = {
  name: "John",
  age: 30,
  email: "john@example.com",
};

function greetUser(user: User) {
  console.log(`Hello, ${user.name}!`);
}

greetUser(user); // ✅ OK
```

### 4.3 Optional Properties

```typescript
interface User {
  name: string;
  age: number;
  email?: string; // Optional (notice the ?)
}

let user1: User = {
  name: "John",
  age: 30,
  email: "john@example.com",
};

let user2: User = {
  name: "Jane",
  age: 25,
  // email is optional, so this is OK
};
```

### 4.4 Readonly Properties

```typescript
interface User {
  readonly id: number; // Cannot be changed after creation
  name: string;
}

let user: User = {
  id: 1,
  name: "John",
};

user.name = "Jane"; // ✅ OK
user.id = 2; // ❌ Error: Cannot assign to 'id' because it is read-only
```

---

## 5. Functions

### 5.1 Function Type Annotations

```typescript
// Parameter types and return type
function add(a: number, b: number): number {
  return a + b;
}

add(5, 10); // ✅ OK
add(5, "10"); // ❌ Error

// Arrow function
const multiply = (a: number, b: number): number => {
  return a * b;
};

// No return value (void)
function logMessage(message: string): void {
  console.log(message);
}
```

### 5.2 Optional Parameters

```typescript
function greet(name: string, greeting?: string): string {
  if (greeting) {
    return `${greeting}, ${name}!`;
  }
  return `Hello, ${name}!`;
}

greet("John"); // ✅ OK - "Hello, John!"
greet("John", "Welcome"); // ✅ OK - "Welcome, John!"
```

### 5.3 Default Parameters

```typescript
function createUser(name: string, role: string = "user"): void {
  console.log(`Creating ${role}: ${name}`);
}

createUser("John"); // "Creating user: John"
createUser("Jane", "admin"); // "Creating admin: Jane"
```

### 5.4 Rest Parameters

```typescript
function sum(...numbers: number[]): number {
  return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3); // 6
sum(1, 2, 3, 4, 5); // 15
```

---

## 6. Type Aliases

Create reusable type definitions:

```typescript
// Instead of repeating the type
type ID = string | number;

let userId: ID = "ABC123";
let productId: ID = 456;

// Complex types
type Point = {
  x: number;
  y: number;
};

let point: Point = { x: 10, y: 20 };

// Function types
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;
const subtract: MathOperation = (a, b) => a - b;
```

### Interface vs Type Alias

```typescript
// Interface (preferred for objects)
interface User {
  name: string;
  age: number;
}

// Type alias (more flexible)
type UserType = {
  name: string;
  age: number;
};

// Both work the same for objects!
// Use interface by default, type for unions/primitives
```

---

## 7. Working with DOM in TypeScript

### 7.1 Selecting Elements

```typescript
// Need to specify element type
const button = document.getElementById("btn") as HTMLButtonElement;
const input = document.querySelector("#username") as HTMLInputElement;
const div = document.querySelector(".container") as HTMLDivElement;

// Handle null case
const element = document.getElementById("myElement");
if (element) {
  element.textContent = "Hello";
}

// Or use non-null assertion (!)
const button2 = document.getElementById("btn")!; // I'm sure it exists!
```

### 7.2 Event Listeners

```typescript
const button = document.getElementById("btn") as HTMLButtonElement;

button.addEventListener("click", (event: MouseEvent) => {
  console.log("Button clicked!");
  console.log("X position:", event.clientX);
});

const input = document.getElementById("username") as HTMLInputElement;

input.addEventListener("input", (event: Event) => {
  const target = event.target as HTMLInputElement;
  console.log("Value:", target.value);
});
```

### 7.3 Complete Form Example

```typescript
// HTML:
// <form id="userForm">
//   <input type="text" id="name" placeholder="Name">
//   <input type="email" id="email" placeholder="Email">
//   <button type="submit">Submit</button>
// </form>
// <div id="output"></div>

interface User {
  name: string;
  email: string;
}

const form = document.getElementById("userForm") as HTMLFormElement;
const nameInput = document.getElementById("name") as HTMLInputElement;
const emailInput = document.getElementById("email") as HTMLInputElement;
const output = document.getElementById("output") as HTMLDivElement;

form.addEventListener("submit", (event: Event) => {
  event.preventDefault();

  const user: User = {
    name: nameInput.value,
    email: emailInput.value,
  };

  displayUser(user);
});

function displayUser(user: User): void {
  output.innerHTML = `
    <p>Name: ${user.name}</p>
    <p>Email: ${user.email}</p>
  `;
}
```

---

## 8. Arrays and Objects

### 8.1 Array of Objects

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

const products: Product[] = [
  { id: 1, name: "Laptop", price: 999 },
  { id: 2, name: "Phone", price: 599 },
  { id: 3, name: "Tablet", price: 399 },
];

// TypeScript knows the type!
products.forEach((product: Product) => {
  console.log(product.name); // Autocomplete works!
});

// Filter with types
const affordable = products.filter((p: Product) => p.price < 500);
```

### 8.2 Object Methods

```typescript
interface Calculator {
  add: (a: number, b: number) => number;
  subtract: (a: number, b: number) => number;
}

const calc: Calculator = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
};

console.log(calc.add(5, 3)); // 8
console.log(calc.subtract(5, 3)); // 2
```

---

## 9. Common Patterns

### 9.1 API Response Types

```typescript
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

interface User {
  id: number;
  name: string;
  email: string;
}

async function fetchUser(id: number): Promise<ApiResponse<User>> {
  const response = await fetch(`/api/users/${id}`);
  return await response.json();
}

// Usage
const result = await fetchUser(1);
console.log(result.data.name); // TypeScript knows this is a User!
```

### 9.2 State Management

```typescript
interface AppState {
  user: User | null;
  isLoggedIn: boolean;
  theme: "light" | "dark";
}

let state: AppState = {
  user: null,
  isLoggedIn: false,
  theme: "light",
};

function login(user: User): void {
  state.user = user;
  state.isLoggedIn = true;
}

function toggleTheme(): void {
  state.theme = state.theme === "light" ? "dark" : "light";
}
```

---

## 10. Setting Up TypeScript Locally

### 10.1 Installation

```bash
# Install TypeScript globally
npm install -g typescript

# Check version
tsc --version
```

### 10.2 Create TypeScript File

```bash
# Create a file
touch app.ts
```

```typescript
// app.ts
function greet(name: string): void {
  console.log(`Hello, ${name}!`);
}

greet("TypeScript");
```

### 10.3 Compile TypeScript to JavaScript

```bash
# Compile single file
tsc app.ts

# This creates app.js
```

### 10.4 Initialize TypeScript Project

```bash
# Create tsconfig.json
tsc --init
```

**Basic tsconfig.json:**

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### 10.5 Project Structure

```
my-project/
├── src/
│   └── app.ts
├── dist/
│   └── app.js (generated)
├── tsconfig.json
└── package.json
```

### 10.6 Watch Mode

```bash
# Automatically recompile on changes
tsc --watch
```

---

## Quick Reference

### Basic Types

```typescript
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
let numbers: number[] = [1, 2, 3];
let id: string | number = "ABC";
```

### Interface

```typescript
interface User {
  name: string;
  age: number;
  email?: string; // Optional
  readonly id: number; // Read-only
}
```

### Function

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

### Type Alias

```typescript
type ID = string | number;
type User = {
  name: string;
  age: number;
};
```

### DOM

```typescript
const btn = document.getElementById("btn") as HTMLButtonElement;
btn.addEventListener("click", (e: MouseEvent) => {
  console.log("Clicked!");
});
```

---

## Common Mistakes to Avoid

### 1. Using `any` everywhere

```typescript
// ❌ Bad - defeats purpose of TypeScript
let data: any = "hello";

// ✅ Good - specific type
let data: string = "hello";
```

### 2. Not handling null/undefined

```typescript
// ❌ Bad
const button = document.getElementById("btn");
button.textContent = "Click"; // Might be null!

// ✅ Good
const button = document.getElementById("btn");
if (button) {
  button.textContent = "Click";
}
```

### 3. Forgetting return type

```typescript
// ❌ Bad - unclear what it returns
function add(a: number, b: number) {
  return a + b;
}

// ✅ Good - explicit return type
function add(a: number, b: number): number {
  return a + b;
}
```

### 4. Wrong element types

```typescript
// ❌ Bad - generic Element type
const input = document.querySelector("#name") as Element;
input.value = "John"; // Error: Property 'value' does not exist

// ✅ Good - specific element type
const input = document.querySelector("#name") as HTMLInputElement;
input.value = "John"; // Works!
```

---

## Resources

**Online Playgrounds:**

- TypeScript Playground: https://www.typescriptlang.org/play
- StackBlitz: https://stackblitz.com/fork/typescript
- CodeSandbox: https://codesandbox.io/s/typescript

**Documentation:**

- Official TypeScript Docs: https://www.typescriptlang.org/docs/
- TypeScript Handbook: https://www.typescriptlang.org/docs/handbook/intro.html

**Learning:**

- TypeScript Deep Dive: https://basarat.gitbook.io/typescript/
- Execute Program: https://www.executeprogram.com/courses/typescript

---

## Summary

Today you learned:

- **TypeScript Basics** - adding types to JavaScript
- **Basic Types** - string, number, boolean, arrays
- **Interfaces** - defining object shapes
- **Functions** - typed parameters and return values
- **DOM with TypeScript** - working with HTML elements safely
- **Setup** - installing and configuring TypeScript

**Key Takeaway:** TypeScript = JavaScript + Type Safety = Fewer Bugs
