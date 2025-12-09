# 5. Async JavaScript and APIs

Today you will connect your JavaScript to the internet:

- Understanding async vs sync code
- Using `async/await` for API calls
- Fetching data with the Fetch API
- Sending form data with POST requests
- Handling loading states and errors

This is where JavaScript connects to the outside world.

---

## 1. Understanding Synchronous vs Asynchronous Code

JavaScript runs **one thing at a time** (single-threaded). But some operations take time:

- Fetching data from a server
- Reading files
- Waiting for timers

### Synchronous = Blocking

```javascript
console.log("First");
console.log("Second");
console.log("Third");
// Output: First, Second, Third (in order)
```

### Asynchronous = Non-Blocking

```javascript
console.log("First");

setTimeout(() => {
  console.log("Second (after 2 seconds)");
}, 2000);

console.log("Third");

// Output:
// First
// Third
// Second (after 2 seconds)
```

### Why does this matter?

Without async, your page would **freeze** while waiting for data.

With async, your page stays **responsive**.

---

## 2. Async/Await (Modern Way)

The cleanest way to handle asynchronous code.

### Basic Syntax

```javascript
async function fetchData() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();
  return data;
}
```

### Key Rules:

1. Use `async` before the function
2. Use `await` before the async operation
3. Always wrap in `try/catch` for errors

### Example with Error Handling

```javascript
async function getUser() {
  try {
    const response = await fetch("https://api.example.com/user");
    const user = await response.json();
    console.log(user);
  } catch (error) {
    console.error("Failed to fetch user:", error);
  }
}
```

---

## 3. The Fetch API

The modern way to make HTTP requests.

### 3.1 GET Request (Fetching Data)

```javascript
async function getUsers() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");

    if (!response.ok) {
      throw new Error("Request failed");
    }

    const users = await response.json();
    console.log(users);
  } catch (error) {
    console.error("Error:", error);
  }
}

getUsers();
```

### 3.2 POST Request (Sending Data)

```javascript
async function createUser(userData) {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(userData),
    });

    const result = await response.json();
    console.log("User created:", result);
  } catch (error) {
    console.error("Error:", error);
  }
}

// Usage
createUser({
  name: "John Doe",
  email: "john@example.com",
});
```

### Important Parts of POST:

- `method: 'POST'` - tells the server we're sending data
- `headers` - tells the server what format we're sending
- `body: JSON.stringify(data)` - converts JavaScript object to JSON string

---

## 4. HTTP Status Codes (Quick Reference)

When you make a request, the server responds with a status code:

```javascript
// 200 - Success
// 201 - Created (for POST requests)
// 400 - Bad Request (your data is wrong)
// 404 - Not Found (resource doesn't exist)
// 500 - Server Error (server is broken)
```

### Checking Status:

```javascript
const response = await fetch(url);

if (!response.ok) {
  throw new Error(`Error: ${response.status}`);
}
```

---

## 5. Form Submission with API (Practical Example)

Let's build a **contact form** that sends data to an API.

### 5.1 HTML

```html
<form id="contactForm">
  <input type="text" id="name" placeholder="Name" required />
  <input type="email" id="email" placeholder="Email" required />
  <textarea id="message" placeholder="Message" required></textarea>
  <button type="submit">Submit</button>
  <p id="status"></p>
</form>
```

### 5.2 JavaScript (With Loading State)

```javascript
const form = document.getElementById("contactForm");
const status = document.getElementById("status");

form.addEventListener("submit", async function (event) {
  event.preventDefault(); // stop page refresh

  // Get form data
  const name = document.getElementById("name").value.trim();
  const email = document.getElementById("email").value.trim();
  const message = document.getElementById("message").value.trim();

  // Validate
  if (!name || !email || !message) {
    status.textContent = "All fields are required";
    return;
  }

  // Show loading
  status.textContent = "Sending...";

  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ name, email, message }),
    });

    if (!response.ok) {
      throw new Error("Failed to submit");
    }

    const result = await response.json();

    // Success
    status.textContent = "Message sent successfully!";
    form.reset();
  } catch (error) {
    // Error
    status.textContent = "Failed to send message. Try again.";
    console.error("Error:", error);
  }
});
```

### What's happening here:

1. `preventDefault()` - stops page refresh
2. Collect form data
3. Validate inputs
4. Show "Sending..." message
5. Make POST request
6. Show success or error message

---

## 6. Mini Project: Lead Capture Form

Build a professional lead capture form like the one companies use.

### 6.1 Complete Example (HTML + CSS + JS)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Register Your Interest</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      body {
        font-family: Arial, sans-serif;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 20px;
      }

      .container {
        background: white;
        border-radius: 10px;
        box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
        max-width: 500px;
        width: 100%;
        padding: 40px;
      }

      h1 {
        color: #333;
        margin-bottom: 10px;
      }

      .subtitle {
        color: #666;
        margin-bottom: 30px;
        font-size: 14px;
      }

      .form-group {
        margin-bottom: 20px;
      }

      label {
        display: block;
        color: #333;
        font-weight: bold;
        margin-bottom: 5px;
        font-size: 14px;
      }

      input,
      select,
      textarea {
        width: 100%;
        padding: 12px;
        border: 1px solid #ddd;
        border-radius: 5px;
        font-size: 14px;
      }

      textarea {
        resize: vertical;
        min-height: 100px;
      }

      button {
        width: 100%;
        padding: 15px;
        background: #667eea;
        color: white;
        border: none;
        border-radius: 5px;
        font-size: 16px;
        cursor: pointer;
        font-weight: bold;
      }

      button:hover {
        background: #5568d3;
      }

      button:disabled {
        background: #ccc;
        cursor: not-allowed;
      }

      .message {
        margin-top: 15px;
        padding: 10px;
        border-radius: 5px;
        text-align: center;
        display: none;
      }

      .message.success {
        background: #d4edda;
        color: #155724;
        display: block;
      }

      .message.error {
        background: #f8d7da;
        color: #721c24;
        display: block;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>Register Your Interest</h1>
      <p class="subtitle">
        Fill out the form and our team will contact you shortly.
      </p>

      <form id="leadForm">
        <div class="form-group">
          <label>Name *</label>
          <input type="text" id="name" required />
        </div>

        <div class="form-group">
          <label>Contact Number *</label>
          <input type="tel" id="phone" required />
        </div>

        <div class="form-group">
          <label>Email</label>
          <input type="email" id="email" />
        </div>

        <div class="form-group">
          <label>Product</label>
          <select id="product">
            <option value="">Select a Product</option>
            <option value="product-a">Product A</option>
            <option value="product-b">Product B</option>
            <option value="product-c">Product C</option>
          </select>
        </div>

        <div class="form-group">
          <label>Message</label>
          <textarea
            id="message"
            placeholder="Tell us about your requirements..."
          ></textarea>
        </div>

        <button type="submit" id="submitBtn">Submit Registration</button>

        <div id="statusMessage" class="message"></div>
      </form>
    </div>

    <script>
      const form = document.getElementById("leadForm");
      const submitBtn = document.getElementById("submitBtn");
      const statusMessage = document.getElementById("statusMessage");

      form.addEventListener("submit", async function (event) {
        event.preventDefault();

        // Get form values
        const name = document.getElementById("name").value.trim();
        const phone = document.getElementById("phone").value.trim();
        const email = document.getElementById("email").value.trim();
        const product = document.getElementById("product").value;
        const message = document.getElementById("message").value.trim();

        // Validate required fields
        if (!name || !phone) {
          showMessage("Name and Contact Number are required", "error");
          return;
        }

        // Validate email if provided
        if (email && !isValidEmail(email)) {
          showMessage("Please enter a valid email", "error");
          return;
        }

        // Prepare data
        const formData = {
          name: name,
          phone: phone,
          email: email,
          product: product,
          message: message,
          timestamp: new Date().toISOString(),
        };

        // Disable button and show loading
        submitBtn.disabled = true;
        submitBtn.textContent = "Submitting...";

        try {
          const response = await fetch(
            "https://jsonplaceholder.typicode.com/posts",
            {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify(formData),
            }
          );

          if (!response.ok) {
            throw new Error("Submission failed");
          }

          const result = await response.json();
          console.log("Success:", result);

          // Show success message
          showMessage("Registration submitted successfully!", "success");
          form.reset();
        } catch (error) {
          console.error("Error:", error);
          showMessage("Failed to submit. Please try again.", "error");
        } finally {
          // Re-enable button
          submitBtn.disabled = false;
          submitBtn.textContent = "Submit Registration";
        }
      });

      function showMessage(text, type) {
        statusMessage.textContent = text;
        statusMessage.className = `message ${type}`;

        // Auto-hide after 5 seconds
        setTimeout(() => {
          statusMessage.className = "message";
        }, 5000);
      }

      function isValidEmail(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
      }
    </script>
  </body>
</html>
```

### What You Learned Here:

- Preventing form submission
- Collecting form data
- Validating inputs (required fields, email format)
- Making POST requests
- Handling loading states (disable button, show message)
- Displaying success/error messages
- Resetting form after success

---

## Common Mistakes to Avoid

### 1. Forgetting `await`

```javascript
// ❌ Wrong
const data = fetch(url).json();

// ✅ Correct
const response = await fetch(url);
const data = await response.json();
```

### 2. Not checking `response.ok`

```javascript
// ✅ Always check
if (!response.ok) {
  throw new Error("Request failed");
}
```

### 3. Forgetting `JSON.stringify()`

```javascript
// ❌ Wrong
body: data;

// ✅ Correct
body: JSON.stringify(data);
```

### 4. Not using `try/catch`

```javascript
// ✅ Always wrap in try/catch
try {
  await fetch(url);
} catch (error) {
  console.error(error);
}
```

---

## Summary

Today you learned:

- **Async/Await** - cleaner way to handle async code
- **Fetch API** - GET data and POST data
- **Form Handling** - prevent default, collect data, validate
- **Loading States** - disable buttons, show messages
- **Error Handling** - try/catch and user feedback
