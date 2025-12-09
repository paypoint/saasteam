# 8. React Fundamentals

Today you will start building modern user interfaces with React:

- Understanding components and JSX
- Props and component composition
- Styling components (CSS Modules, Tailwind, Styled-components)
- Building reusable UI components
- React project setup

This is where you start building real applications.

---

## 1. What is React?

React is a **JavaScript library for building user interfaces**.

### Why React?

```html
<!-- Vanilla JavaScript - Lots of DOM manipulation -->
<div id="root"></div>

<script>
  const root = document.getElementById("root");
  const button = document.createElement("button");
  button.textContent = "Click me";
  button.addEventListener("click", () => {
    alert("Clicked!");
  });
  root.appendChild(button);
</script>
```

```jsx
// React - Declarative and simple
function App() {
  return <button onClick={() => alert("Clicked!")}>Click me</button>;
}
```

**Benefits:**

- Component-based architecture
- Reusable UI pieces
- Virtual DOM for performance
- Huge ecosystem
- Used by Facebook, Netflix, Instagram, Airbnb

---

## 2. Try React Online

**No installation needed! Try these:**

- **CodeSandbox**: https://codesandbox.io/s/react-new
- **StackBlitz**: https://stackblitz.com/fork/react
- **React Playground**: https://playcode.io/react

_Note: For this tutorial, you can practice in any of these online editors._

---

## 3. JSX - JavaScript XML

JSX lets you write HTML-like code in JavaScript.

### 3.1 Basic JSX

```jsx
// Regular JavaScript
const element = React.createElement("h1", null, "Hello World");

// JSX - Much cleaner!
const element = <h1>Hello World</h1>;
```

### 3.2 JSX Rules

```jsx
// 1. Must return single parent element
// ❌ Wrong
function App() {
  return (
    <h1>Title</h1>
    <p>Paragraph</p>
  );
}

// ✅ Correct - wrap in parent
function App() {
  return (
    <div>
      <h1>Title</h1>
      <p>Paragraph</p>
    </div>
  );
}

// ✅ Or use Fragment
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Paragraph</p>
    </>
  );
}
```

### 3.3 JavaScript in JSX

```jsx
function App() {
  const name = "John";
  const age = 25;

  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>You are {age} years old</p>
      <p>Next year: {age + 1}</p>
    </div>
  );
}
```

### 3.4 JSX Attributes

```jsx
function App() {
  return (
    <div>
      {/* className instead of class */}
      <h1 className="title">Hello</h1>

      {/* camelCase for attributes */}
      <button onClick={handleClick}>Click</button>

      {/* Style as object */}
      <p style={{ color: "blue", fontSize: "20px" }}>Styled text</p>

      {/* Self-closing tags need / */}
      <img src="logo.png" alt="Logo" />
      <input type="text" />
    </div>
  );
}
```

---

## 4. Components

Components are **reusable pieces of UI**.

### 4.1 Function Components

```jsx
// Simple component
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// Using the component
function App() {
  return (
    <div>
      <Welcome />
      <Welcome />
      <Welcome />
    </div>
  );
}
```

### 4.2 Component with Logic

```jsx
function Greeting() {
  const hour = new Date().getHours();
  const greeting = hour < 12 ? "Good Morning" : "Good Evening";

  return <h1>{greeting}!</h1>;
}
```

### 4.3 Conditional Rendering

```jsx
function UserStatus({ isLoggedIn }) {
  // Using if statement
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  }
  return <h1>Please log in</h1>;
}

// OR using ternary
function UserStatus({ isLoggedIn }) {
  return <h1>{isLoggedIn ? "Welcome back!" : "Please log in"}</h1>;
}

// OR using &&
function Notification({ hasMessage }) {
  return <div>{hasMessage && <p>You have new messages!</p>}</div>;
}
```

---

## 5. Props - Passing Data to Components

Props are like **function parameters** for components.

### 5.1 Basic Props

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

function App() {
  return (
    <div>
      <Greeting name="John" />
      <Greeting name="Jane" />
      <Greeting name="Bob" />
    </div>
  );
}
```

### 5.2 Destructuring Props (Cleaner)

```jsx
// Instead of props.name, props.age
function UserCard({ name, age, email }) {
  return (
    <div className="card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}

function App() {
  return <UserCard name="John Doe" age={30} email="john@example.com" />;
}
```

### 5.3 Children Prop

Special prop for nested content:

```jsx
function Card({ children }) {
  return (

      {children}

  );
}

function App() {
  return (

      Title
      This is content inside the card!
      Click Me

  );
}
```

**Why use children?**

- Makes components flexible
- Allows nesting any content
- Common in wrapper components (Card, Modal, Layout)

### 5.4 Props with Arrays

```jsx
function UserList({ users }) {
  return (
    <div>
      <h2>Users</h2>
      {users.map((user) => (
        <div key={user.id} className="user-card">
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ))}
    </div>
  );
}

function App() {
  const users = [
    { id: 1, name: "John", email: "john@example.com" },
    { id: 2, name: "Jane", email: "jane@example.com" },
    { id: 3, name: "Bob", email: "bob@example.com" },
  ];

  return <UserList users={users} />;
}
```

**Important:** Always add `key` prop when mapping arrays!

---

## 6. Styling in React

### 6.1 Inline Styles

```jsx
function Button() {
  const buttonStyle = {
    backgroundColor: "#007bff",
    color: "white",
    padding: "10px 20px",
    border: "none",
    borderRadius: "5px",
    cursor: "pointer",
  };

  return <button style={buttonStyle}>Click Me</button>;
}
```

### 6.2 CSS Modules (Recommended for Learning)

**Button.module.css:**

```css
.button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.button:hover {
  background-color: #0056b3;
}
```

**Button.jsx:**

```jsx
import styles from "./Button.module.css";

function Button({ children }) {
  return <button className={styles.button}>{children}</button>;
}
```

**Why CSS Modules?**

- Scoped styles (no conflicts)
- Works with Vite/Create React App
- Easy to learn

### 6.3 Tailwind CSS (Most Popular)

**Setup with Vite:**

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

**Usage:**

```jsx
function Button({ children }) {
  return (
    <button className="bg-blue-500 text-white px-6 py-2 rounded hover:bg-blue-600">
      {children}
    </button>
  );
}

function Card({ title, description }) {
  return (
    <div className="bg-white rounded-lg shadow-md p-6 max-w-sm">
      <h2 className="text-2xl font-bold mb-2">{title}</h2>
      <p className="text-gray-600">{description}</p>
    </div>
  );
}
```

**Why Tailwind?**

- Fast development
- No CSS file needed
- Responsive utilities
- Industry standard

### 6.4 Styled-components (CSS-in-JS)

**Installation:**

```bash
npm install styled-components
```

**Usage:**

```jsx
import styled from "styled-components";

const Button = styled.button`
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: #0056b3;
  }
`;

const Card = styled.div`
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
`;

function App() {
  return (
    <Card>
      <h2>Hello</h2>
      <Button>Click Me</Button>
    </Card>
  );
}
```

**Why Styled-components?**

- Dynamic styling with props
- Automatic vendor prefixing
- Popular in large apps

### Comparison

| Method                | Pros               | Cons           | Use When        |
| --------------------- | ------------------ | -------------- | --------------- |
| **CSS Modules**       | Scoped, simple     | More files     | Learning React  |
| **Tailwind**          | Fast, no CSS files | Long className | Production apps |
| **Styled-components** | Dynamic, scoped    | Extra bundle   | Complex styling |

**Recommendation for beginners:** Start with **CSS Modules**, then try **Tailwind**.

---

## 7. Building Reusable Components

### 7.1 Button Component

```jsx
// Button.jsx
import styles from "./Button.module.css";

function Button({ children, variant = "primary", onClick }) {
  return (
    <button className={`${styles.button} ${styles[variant]}`} onClick={onClick}>
      {children}
    </button>
  );
}

export default Button;
```

**Button.module.css:**

```css
.button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.primary {
  background-color: #007bff;
  color: white;
}

.secondary {
  background-color: #6c757d;
  color: white;
}

.danger {
  background-color: #dc3545;
  color: white;
}
```

**Usage:**

```jsx
function App() {
  return (
    <div>
      <Button variant="primary">Save</Button>
      <Button variant="secondary">Cancel</Button>
      <Button variant="danger">Delete</Button>
    </div>
  );
}
```

### 7.2 Card Component

```jsx
// Card.jsx
import styles from "./Card.module.css";

function Card({ title, description, image, buttonText, onButtonClick }) {
  return (
    <div className={styles.card}>
      {image && <img src={image} alt={title} className={styles.image} />}
      <div className={styles.content}>
        <h3 className={styles.title}>{title}</h3>
        <p className={styles.description}>{description}</p>
        {buttonText && (
          <button className={styles.button} onClick={onButtonClick}>
            {buttonText}
          </button>
        )}
      </div>
    </div>
  );
}

export default Card;
```

**Card.module.css:**

```css
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  max-width: 300px;
}

.image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.content {
  padding: 20px;
}

.title {
  font-size: 20px;
  margin-bottom: 10px;
  color: #333;
}

.description {
  color: #666;
  margin-bottom: 15px;
}

.button {
  background-color: #007bff;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
```

**Usage:**

```jsx
function App() {
  return (
    <div>
      <Card
        title="Product Name"
        description="This is a great product"
        image="/product.jpg"
        buttonText="Buy Now"
        onButtonClick={() => alert("Added to cart!")}
      />
    </div>
  );
}
```

### 7.3 Input Component

```jsx
// Input.jsx
import styles from "./Input.module.css";

function Input({ label, type = "text", placeholder, value, onChange, error }) {
  return (
    <div className={styles.inputGroup}>
      {label && <label className={styles.label}>{label}</label>}
      <input
        type={type}
        placeholder={placeholder}
        value={value}
        onChange={onChange}
        className={`${styles.input} ${error ? styles.error : ""}`}
      />
      {error && <span className={styles.errorText}>{error}</span>}
    </div>
  );
}

export default Input;
```

**Input.module.css:**

```css
.inputGroup {
  margin-bottom: 15px;
}

.label {
  display: block;
  margin-bottom: 5px;
  color: #333;
  font-weight: 500;
}

.input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.input:focus {
  outline: none;
  border-color: #007bff;
}

.input.error {
  border-color: #dc3545;
}

.errorText {
  color: #dc3545;
  font-size: 12px;
  margin-top: 5px;
  display: block;
}
```

---

## 8. Setting Up React Project

### 8.1 Using Vite (Recommended - Fast!)

```bash
# Create new project
npm create vite@latest my-app -- --template react

# Navigate to project
cd my-app

# Install dependencies
npm install

# Start development server
npm run dev
```

**Project Structure:**

```
my-app/
├── src/
│   ├── App.jsx          # Main component
│   ├── App.css          # Main styles
│   ├── main.jsx         # Entry point
│   └── components/      # Your components
├── index.html
├── package.json
└── vite.config.js
```

### 8.2 Using Create React App (Classic)

```bash
# Create new project
npx create-react-app my-app

# Navigate to project
cd my-app

# Start development server
npm start
```

**Vite vs Create React App:**

| Feature        | Vite         | Create React App |
| -------------- | ------------ | ---------------- |
| Speed          | ⚡ Very Fast | Slower           |
| Build Size     | Smaller      | Larger           |
| Learning Curve | Easy         | Easy             |
| Popularity     | Growing      | Established      |

**Recommendation:** Use **Vite** for new projects.

---

## 9. Complete Example - Product Catalog

**App.jsx:**

```jsx
import { useState } from "react";
import ProductCard from "./components/ProductCard";
import "./App.css";

function App() {
  const products = [
    {
      id: 1,
      name: "Wireless Headphones",
      price: 99,
      image: "https://via.placeholder.com/300x200",
      description: "High-quality wireless headphones",
    },
    {
      id: 2,
      name: "Smart Watch",
      price: 199,
      image: "https://via.placeholder.com/300x200",
      description: "Feature-rich smart watch",
    },
    {
      id: 3,
      name: "Laptop Stand",
      price: 49,
      image: "https://via.placeholder.com/300x200",
      description: "Ergonomic laptop stand",
    },
  ];

  return (
    <div className="app">
      <header className="header">
        <h1>Product Catalog</h1>
      </header>

      <div className="products-grid">
        {products.map((product) => (
          <ProductCard
            key={product.id}
            name={product.name}
            price={product.price}
            image={product.image}
            description={product.description}
          />
        ))}
      </div>
    </div>
  );
}

export default App;
```

**components/ProductCard.jsx:**

```jsx
import styles from "./ProductCard.module.css";

function ProductCard({ name, price, image, description }) {
  const handleAddToCart = () => {
    alert(`Added ${name} to cart!`);
  };

  return (
    <div className={styles.card}>
      <img src={image} alt={name} className={styles.image} />
      <div className={styles.content}>
        <h3 className={styles.title}>{name}</h3>
        <p className={styles.description}>{description}</p>
        <div className={styles.footer}>
          <span className={styles.price}>${price}</span>
          <button className={styles.button} onClick={handleAddToCart}>
            Add to Cart
          </button>
        </div>
      </div>
    </div>
  );
}

export default ProductCard;
```

**components/ProductCard.module.css:**

```css
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.content {
  padding: 20px;
}

.title {
  font-size: 20px;
  margin-bottom: 10px;
  color: #333;
}

.description {
  color: #666;
  margin-bottom: 15px;
  font-size: 14px;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.price {
  font-size: 24px;
  font-weight: bold;
  color: #007bff;
}

.button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
}

.button:hover {
  background-color: #0056b3;
}
```

**App.css:**

```css
.app {
  min-height: 100vh;
  background: #f5f5f5;
  padding: 20px;
}

.header {
  text-align: center;
  margin-bottom: 40px;
}

.header h1 {
  color: #333;
  font-size: 36px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 30px;
  max-width: 1200px;
  margin: 0 auto;
}
```

---

## Quick Reference

### Component Structure

```jsx
function ComponentName({ prop1, prop2 }) {
  return (
    <div>
      <h1>{prop1}</h1>
      <p>{prop2}</p>
    </div>
  );
}
```

### Mapping Arrays

```jsx
{
  items.map((item) => <div key={item.id}>{item.name}</div>);
}
```

### Conditional Rendering

```jsx
// Ternary
{
  condition ? <ComponentA /> : <ComponentB />;
}

// AND operator
{
  condition && <Component />;
}

// If statement
if (condition) return <ComponentA />;
return <ComponentB />;
```

### Props

```jsx
// Passing props
<Component name="John" age={25} />;

// Receiving props
function Component({ name, age }) {
  return (
    <p>
      {name} is {age}
    </p>
  );
}
```

---

## Common Mistakes to Avoid

### 1. Forgetting key prop in lists

```jsx
// ❌ Bad
{
  items.map((item) => <div>{item.name}</div>);
}

// ✅ Good
{
  items.map((item) => <div key={item.id}>{item.name}</div>);
}
```

### 2. Using class instead of className

```jsx
// ❌ Bad
<div class="container">Content</div>

// ✅ Good
<div className="container">Content</div>
```

### 3. Not wrapping JSX in parentheses

```jsx
// ❌ Bad - Hard to read
function Component() {
  return (
    <div>
      <h1>Title</h1>
    </div>
  );
}

// ✅ Good
function Component() {
  return (
    <div>
      <h1>Title</h1>
    </div>
  );
}
```

### 4. Multiple root elements

```jsx
// ❌ Bad
function Component() {
  return (
    <h1>Title</h1>
    <p>Text</p>
  );
}

// ✅ Good
function Component() {
  return (
    <>
      <h1>Title</h1>
      <p>Text</p>
    </>
  );
}
```

---

## Resources

**Online Playgrounds:**

- CodeSandbox: https://codesandbox.io/s/react-new
- StackBlitz: https://stackblitz.com/fork/react
- React Playground: https://playcode.io/react

**Documentation:**

- Official React Docs: https://react.dev
- React Beta Docs: https://react.dev/learn

**Styling:**

- Tailwind CSS: https://tailwindcss.com
- Styled-components: https://styled-components.com
- CSS Modules: https://github.com/css-modules/css-modules

**Learning:**

- React Tutorial: https://react.dev/learn/tutorial-tic-tac-toe
- Scrimba React Course: https://scrimba.com/learn/learnreact

---

## Summary

Today you learned:

- **JSX** - HTML-like syntax in JavaScript
- **Components** - reusable UI pieces
- **Props** - passing data to components
- **Styling** - CSS Modules, Tailwind, Styled-components
- **Reusable Components** - Button, Card, Input
- **Project Setup** - Vite and Create React App

**Key Takeaway:** React = Components + Props + JSX = Reusable UI
