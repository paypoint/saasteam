# 6. Browser Fundamentals & Modern Tools

Today you will learn how browsers work and optimize your code:

- Using localStorage and sessionStorage
- Understanding browser architecture
- Performance optimization techniques
- Debouncing and throttling
- Lazy loading

This is where you make your apps faster and smarter.

---

## 1. Browser Storage

Browsers let you store data locally (on the user's computer). Two main types:

### 1.1 localStorage (Permanent Storage)

Data stays **forever** (until manually deleted).

```javascript
// Save data
localStorage.setItem("username", "John");
localStorage.setItem("theme", "dark");

// Get data
const username = localStorage.getItem("username");
console.log(username); // 'John'

// Remove data
localStorage.removeItem("username");

// Clear all data
localStorage.clear();
```

### 1.2 sessionStorage (Temporary Storage)

Data stays until the **tab is closed**.

```javascript
// Save data (same syntax as localStorage)
sessionStorage.setItem("cartItems", "5");

// Get data
const items = sessionStorage.getItem("cartItems");
console.log(items); // '5'

// Remove data
sessionStorage.removeItem("cartItems");

// Clear all
sessionStorage.clear();
```

### Key Differences:

| Feature  | localStorage     | sessionStorage      |
| -------- | ---------------- | ------------------- |
| Duration | Forever          | Until tab closes    |
| Scope    | All tabs         | Single tab only     |
| Use Case | User preferences | Temporary cart data |

### Important: You can only store strings!

```javascript
// ❌ Wrong (storing object directly)
localStorage.setItem("user", { name: "John" });

// ✅ Correct (convert to JSON string)
const user = { name: "John", age: 30 };
localStorage.setItem("user", JSON.stringify(user));

// Getting it back
const savedUser = JSON.parse(localStorage.getItem("user"));
console.log(savedUser.name); // 'John'
```

---

## 2. Practical Example: Dark Mode Toggle

Let's save user's theme preference.

### HTML

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Dark Mode Demo</title>
    <style>
      body {
        margin: 0;
        padding: 40px;
        font-family: Arial, sans-serif;
        transition: all 0.3s;
      }

      body.light {
        background: white;
        color: black;
      }

      body.dark {
        background: #1a1a1a;
        color: white;
      }

      button {
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
        border: none;
        border-radius: 5px;
      }

      .light button {
        background: #333;
        color: white;
      }

      .dark button {
        background: white;
        color: #333;
      }
    </style>
  </head>
  <body class="light">
    <h1>Dark Mode Example</h1>
    <p>Your preference is saved in localStorage!</p>
    <button id="themeToggle">Toggle Dark Mode</button>

    <script>
      const body = document.body;
      const toggleBtn = document.getElementById("themeToggle");

      // Load saved theme on page load
      const savedTheme = localStorage.getItem("theme") || "light";
      body.className = savedTheme;
      updateButtonText();

      // Toggle theme on button click
      toggleBtn.addEventListener("click", function () {
        if (body.className === "light") {
          body.className = "dark";
          localStorage.setItem("theme", "dark");
        } else {
          body.className = "light";
          localStorage.setItem("theme", "light");
        }
        updateButtonText();
      });

      function updateButtonText() {
        toggleBtn.textContent =
          body.className === "light"
            ? "Switch to Dark Mode"
            : "Switch to Light Mode";
      }
    </script>
  </body>
</html>
```

### What's happening:

1. Load saved theme from localStorage on page load
2. Apply the saved theme to body
3. When button clicked → toggle theme → save to localStorage
4. Theme persists across page refreshes!

---

## 3. Practical Example: Shopping Cart

Using localStorage to persist cart items.

### HTML + JavaScript

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Shopping Cart</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        max-width: 600px;
        margin: 50px auto;
        padding: 20px;
      }

      .product {
        border: 1px solid #ddd;
        padding: 15px;
        margin-bottom: 10px;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }

      button {
        padding: 8px 15px;
        background: #007bff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
      }

      button:hover {
        background: #0056b3;
      }

      .cart {
        background: #f8f9fa;
        padding: 20px;
        border-radius: 8px;
        margin-top: 30px;
      }

      .cart-item {
        padding: 10px;
        border-bottom: 1px solid #ddd;
        display: flex;
        justify-content: space-between;
      }

      .remove-btn {
        background: #dc3545;
        padding: 5px 10px;
        font-size: 12px;
      }
    </style>
  </head>
  <body>
    <h1>Products</h1>

    <div class="product">
      <div>
        <h3>Laptop</h3>
        <p>$999</p>
      </div>
      <button onclick="addToCart('Laptop', 999)">Add to Cart</button>
    </div>

    <div class="product">
      <div>
        <h3>Phone</h3>
        <p>$599</p>
      </div>
      <button onclick="addToCart('Phone', 599)">Add to Cart</button>
    </div>

    <div class="product">
      <div>
        <h3>Headphones</h3>
        <p>$199</p>
      </div>
      <button onclick="addToCart('Headphones', 199)">Add to Cart</button>
    </div>

    <div class="cart">
      <h2>Shopping Cart</h2>
      <div id="cartItems"></div>
      <h3 id="total">Total: $0</h3>
      <button onclick="clearCart()">Clear Cart</button>
    </div>

    <script>
      // Load cart from localStorage
      let cart = JSON.parse(localStorage.getItem("cart")) || [];

      // Add item to cart
      function addToCart(name, price) {
        const item = { name, price, id: Date.now() };
        cart.push(item);
        saveCart();
        displayCart();
      }

      // Remove item from cart
      function removeFromCart(id) {
        cart = cart.filter((item) => item.id !== id);
        saveCart();
        displayCart();
      }

      // Clear entire cart
      function clearCart() {
        cart = [];
        saveCart();
        displayCart();
      }

      // Save cart to localStorage
      function saveCart() {
        localStorage.setItem("cart", JSON.stringify(cart));
      }

      // Display cart items
      function displayCart() {
        const cartItems = document.getElementById("cartItems");
        const totalEl = document.getElementById("total");

        if (cart.length === 0) {
          cartItems.innerHTML = "<p>Cart is empty</p>";
          totalEl.textContent = "Total: $0";
          return;
        }

        cartItems.innerHTML = cart
          .map(
            (item) => `
        <div class="cart-item">
          <span>${item.name} - $${item.price}</span>
          <button class="remove-btn" onclick="removeFromCart(${item.id})">Remove</button>
        </div>
      `
          )
          .join("");

        const total = cart.reduce((sum, item) => sum + item.price, 0);
        totalEl.textContent = `Total: $${total}`;
      }

      // Display cart on page load
      displayCart();
    </script>
  </body>
</html>
```

### What You Learned:

- Storing arrays in localStorage (using JSON.stringify)
- Loading cart data on page load
- Adding/removing items
- Persisting data across page refreshes
- Calculating totals from array

---

## 4. Browser Architecture (Quick Overview)

Understanding how browsers work helps you write better code.

### Browser has multiple processes:

```
Browser Process (UI, tabs, navigation)
    ↓
Renderer Process (HTML, CSS, JS execution)
    ↓
GPU Process (graphics rendering)
    ↓
Network Process (HTTP requests)
```

### Critical Rendering Path:

```
1. HTML → DOM Tree
2. CSS → CSSOM Tree
3. DOM + CSSOM → Render Tree
4. Layout (calculate positions)
5. Paint (draw pixels)
```

### Why this matters:

- Large DOM = slower rendering
- Many CSS rules = slower styling
- Heavy JavaScript = blocked rendering

---

## 5. Performance Optimization

### 5.1 Debouncing

**Problem:** User types in search box → API call on every keystroke → too many requests!

**Solution:** Wait until user stops typing.

```javascript
// Without debouncing (BAD)
const searchInput = document.getElementById("search");

searchInput.addEventListener("input", function () {
  searchAPI(this.value); // Called on EVERY keystroke!
});
```

```javascript
// With debouncing (GOOD)
function debounce(func, delay) {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), delay);
  };
}

const searchInput = document.getElementById("search");

const debouncedSearch = debounce(function (value) {
  searchAPI(value); // Called only after user stops typing
}, 500);

searchInput.addEventListener("input", function () {
  debouncedSearch(this.value);
});
```

### Complete Search Example:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Debounced Search</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        max-width: 600px;
        margin: 50px auto;
        padding: 20px;
      }

      input {
        width: 100%;
        padding: 12px;
        font-size: 16px;
        border: 2px solid #ddd;
        border-radius: 5px;
      }

      #results {
        margin-top: 20px;
      }

      .result-item {
        padding: 10px;
        border-bottom: 1px solid #eee;
      }

      #apiCallCount {
        margin-top: 10px;
        color: #666;
        font-size: 14px;
      }
    </style>
  </head>
  <body>
    <h1>Search Products (Debounced)</h1>
    <input type="text" id="search" placeholder="Type to search..." />
    <p id="apiCallCount">API calls: 0</p>
    <div id="results"></div>

    <script>
      let apiCallCount = 0;
      const searchInput = document.getElementById("search");
      const resultsDiv = document.getElementById("results");
      const countDiv = document.getElementById("apiCallCount");

      // Debounce function
      function debounce(func, delay) {
        let timeout;
        return function (...args) {
          clearTimeout(timeout);
          timeout = setTimeout(() => func.apply(this, args), delay);
        };
      }

      // Fake API search
      async function searchAPI(query) {
        if (!query.trim()) {
          resultsDiv.innerHTML = "";
          return;
        }

        apiCallCount++;
        countDiv.textContent = `API calls: ${apiCallCount}`;

        resultsDiv.innerHTML = "Searching...";

        // Simulate API delay
        await new Promise((resolve) => setTimeout(resolve, 300));

        // Fake results
        const results = [
          `Result 1 for "${query}"`,
          `Result 2 for "${query}"`,
          `Result 3 for "${query}"`,
        ];

        resultsDiv.innerHTML = results
          .map((r) => `<div class="result-item">${r}</div>`)
          .join("");
      }

      // Debounced search (waits 500ms after user stops typing)
      const debouncedSearch = debounce(searchAPI, 500);

      searchInput.addEventListener("input", function () {
        debouncedSearch(this.value);
      });
    </script>
  </body>
</html>
```

**Without debouncing:** Type "laptop" → 6 API calls

**With debouncing:** Type "laptop" → 1 API call

---

### 5.2 Throttling

**Problem:** User scrolls page → event fires hundreds of times → performance issues!

**Solution:** Limit how often function runs.

```javascript
function throttle(func, limit) {
  let inThrottle;
  return function (...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

// Usage
const handleScroll = throttle(function () {
  console.log("Scroll position:", window.scrollY);
}, 1000); // Run at most once per second

window.addEventListener("scroll", handleScroll);
```

### When to use what:

- **Debouncing:** Search boxes, form validation, window resize
- **Throttling:** Scroll events, mouse movements, animations

---

### 5.3 Using Lodash (Industry Standard)

In real projects, developers use **Lodash** library for debounce and throttle.

**Why Lodash?**

- Battle-tested and optimized
- Used by millions of developers
- Handles edge cases
- Simple to use

#### Installing Lodash

```html
<!-- Add this in your HTML <head> -->
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
```

Or via npm:

```bash
npm install lodash
```

#### Lodash Debounce Example

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Lodash Debounce</title>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    <style>
      body {
        font-family: Arial, sans-serif;
        max-width: 600px;
        margin: 50px auto;
        padding: 20px;
      }

      input {
        width: 100%;
        padding: 12px;
        font-size: 16px;
        border: 2px solid #ddd;
        border-radius: 5px;
        margin-bottom: 10px;
      }

      .stats {
        background: #f8f9fa;
        padding: 15px;
        border-radius: 5px;
        margin-top: 20px;
      }
    </style>
  </head>
  <body>
    <h1>Lodash Debounce Example</h1>

    <label>Without Debounce (calls on every keystroke):</label>
    <input type="text" id="normal" placeholder="Type something..." />

    <label>With Lodash Debounce (waits 500ms):</label>
    <input type="text" id="debounced" placeholder="Type something..." />

    <div class="stats">
      <p>Normal Input - API calls: <strong id="normalCount">0</strong></p>
      <p>Debounced Input - API calls: <strong id="debouncedCount">0</strong></p>
    </div>

    <script>
      let normalCount = 0;
      let debouncedCount = 0;

      const normalInput = document.getElementById("normal");
      const debouncedInput = document.getElementById("debounced");
      const normalCountEl = document.getElementById("normalCount");
      const debouncedCountEl = document.getElementById("debouncedCount");

      // Without debounce
      normalInput.addEventListener("input", function () {
        normalCount++;
        normalCountEl.textContent = normalCount;
        console.log("Normal search:", this.value);
      });

      // With Lodash debounce
      const debouncedSearch = _.debounce(function (value) {
        debouncedCount++;
        debouncedCountEl.textContent = debouncedCount;
        console.log("Debounced search:", value);
      }, 500);

      debouncedInput.addEventListener("input", function () {
        debouncedSearch(this.value);
      });
    </script>
  </body>
</html>
```

#### Lodash Throttle Example

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Lodash Throttle</title>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    <style>
      body {
        font-family: Arial, sans-serif;
        height: 3000px;
        padding: 20px;
      }

      .sticky {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        background: white;
        padding: 15px;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        z-index: 1000;
      }

      .stats {
        background: #f8f9fa;
        padding: 10px;
        border-radius: 5px;
        margin-top: 10px;
      }
    </style>
  </head>
  <body>
    <div class="sticky">
      <h1>Lodash Throttle Example</h1>
      <p>Scroll down to see the difference!</p>

      <div class="stats">
        <p>Normal scroll events: <strong id="normalCount">0</strong></p>
        <p>
          Throttled events (1 per second):
          <strong id="throttledCount">0</strong>
        </p>
        <p>Current scroll position: <strong id="scrollPos">0</strong>px</p>
      </div>
    </div>

    <div style="margin-top: 200px;">
      <h2>Keep scrolling...</h2>
      <p>Notice how throttled events fire much less frequently!</p>
    </div>

    <script>
      let normalCount = 0;
      let throttledCount = 0;

      const normalCountEl = document.getElementById("normalCount");
      const throttledCountEl = document.getElementById("throttledCount");
      const scrollPosEl = document.getElementById("scrollPos");

      // Without throttle (fires constantly)
      window.addEventListener("scroll", function () {
        normalCount++;
        normalCountEl.textContent = normalCount;
        scrollPosEl.textContent = Math.round(window.scrollY);
      });

      // With Lodash throttle (fires max once per second)
      const throttledScroll = _.throttle(function () {
        throttledCount++;
        throttledCountEl.textContent = throttledCount;
      }, 1000);

      window.addEventListener("scroll", throttledScroll);
    </script>
  </body>
</html>
```

#### Lodash Methods Comparison

```javascript
// Debounce - waits until user stops
const debouncedFn = _.debounce(myFunction, 500);

// Throttle - limits execution rate
const throttledFn = _.throttle(myFunction, 1000);

// Cancel debounce/throttle if needed
debouncedFn.cancel();
throttledFn.cancel();
```

#### Real-World Usage

```javascript
// Search API call (use debounce)
const searchProducts = _.debounce(async (query) => {
  const results = await fetch(`/api/search?q=${query}`);
  displayResults(results);
}, 300);

searchInput.addEventListener("input", (e) => {
  searchProducts(e.target.value);
});

// Scroll tracking (use throttle)
const trackScroll = _.throttle(() => {
  const scrollPercent = (window.scrollY / document.body.scrollHeight) * 100;
  analytics.track("scroll", { percent: scrollPercent });
}, 2000);

window.addEventListener("scroll", trackScroll);

// Window resize (use debounce)
const handleResize = _.debounce(() => {
  console.log("Window resized to:", window.innerWidth);
  adjustLayout();
}, 250);

window.addEventListener("resize", handleResize);
```

---

### 5.4 Lazy Loading Images

Load images only when they're about to appear on screen.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Lazy Loading</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
      }

      img {
        width: 100%;
        height: 400px;
        object-fit: cover;
        margin-bottom: 20px;
        background: #f0f0f0;
      }

      .placeholder {
        background: linear-gradient(
          90deg,
          #f0f0f0 25%,
          #e0e0e0 50%,
          #f0f0f0 75%
        );
        background-size: 200% 100%;
        animation: loading 1.5s infinite;
      }

      @keyframes loading {
        0% {
          background-position: 200% 0;
        }
        100% {
          background-position: -200% 0;
        }
      }
    </style>
  </head>
  <body>
    <h1>Lazy Loading Images</h1>
    <p>Scroll down to load images</p>

    <!-- Images with data-src instead of src -->
    <img
      class="lazy placeholder"
      data-src="https://picsum.photos/800/400?random=1"
      alt="Image 1"
    />
    <img
      class="lazy placeholder"
      data-src="https://picsum.photos/800/400?random=2"
      alt="Image 2"
    />
    <img
      class="lazy placeholder"
      data-src="https://picsum.photos/800/400?random=3"
      alt="Image 3"
    />
    <img
      class="lazy placeholder"
      data-src="https://picsum.photos/800/400?random=4"
      alt="Image 4"
    />
    <img
      class="lazy placeholder"
      data-src="https://picsum.photos/800/400?random=5"
      alt="Image 5"
    />

    <script>
      // Intersection Observer (modern way)
      const lazyImages = document.querySelectorAll(".lazy");

      const imageObserver = new IntersectionObserver((entries, observer) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            const img = entry.target;
            img.src = img.dataset.src;
            img.classList.remove("placeholder");
            observer.unobserve(img);
          }
        });
      });

      lazyImages.forEach((img) => imageObserver.observe(img));
    </script>
  </body>
</html>
```

### How it works:

1. Images start with `data-src` (not loaded yet)
2. Intersection Observer watches when image enters viewport
3. When visible → set `src` attribute → image loads
4. Stop observing that image

**Result:** Page loads faster, uses less bandwidth!

---

## Common Mistakes to Avoid

### 1. Storing objects directly

```javascript
// ❌ Wrong
localStorage.setItem("user", { name: "John" });

// ✅ Correct
localStorage.setItem("user", JSON.stringify({ name: "John" }));
```

### 2. Not parsing JSON when getting

```javascript
// ❌ Wrong
const user = localStorage.getItem("user");

// ✅ Correct
const user = JSON.parse(localStorage.getItem("user"));
```

### 3. Not handling null values

```javascript
// ✅ Always provide fallback
const notes = JSON.parse(localStorage.getItem("notes")) || [];
```

### 4. Too many API calls without debouncing

```javascript
// ❌ Bad performance
input.addEventListener("input", () => {
  fetchAPI(); // Called on every keystroke!
});

// ✅ Good performance
const debouncedFetch = debounce(fetchAPI, 500);
input.addEventListener("input", debouncedFetch);
```

---

## Summary

Today you learned:

- **localStorage** - permanent storage for user data
- **sessionStorage** - temporary storage for session data
- **Browser Architecture** - how browsers render pages
- **Debouncing** - delay function until user stops action
- **Throttling** - limit how often function runs
- **Lazy Loading** - load images only when needed
