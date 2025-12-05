---
theme: default
background: https://images.unsplash.com/photo-1633356122544-f134324a6cee?w=1920
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## Session 1: React Fundamentals + Basic App
  Build a functional todo list application
drawings:
  persist: false
transition: slide-left
title: React Fundamentals
titleTemplate: '%s'
favicon: '/favicon.svg'
mdc: true
download: true
exportFilename: react-fundamentals-session-1
# Hide default pagination, use custom dot indicator
css: unocss
---

# Session 1: React Fundamentals + Basic App

Building Modern Web Applications with React

---
layout: default
---

# Today's Goal

Build a functional todo list application with:

- Component-based architecture
- State management
- Event handling
- localStorage persistence

---

# Learning Objectives

By the end of today, you will:

- Understand component-based architecture
- Master React core concepts (JSX, props, state, hooks)
- Learn modern JavaScript (ES6+, destructuring, array methods)
- Build a functional single-page application
- Understand unidirectional data flow

---
layout: center
class: text-center
---

# Part 1: Foundations

---

# What is React and Why Use It?

## The Problem

- Traditional web development: Direct DOM manipulation
- Complex UIs become hard to maintain
- State and UI easily get out of sync
- Code becomes repetitive and error-prone

---

# The React Solution

- **Declarative**: Describe what you want, not how to get it
- **Component-Based**: Break UI into reusable pieces
- **Unidirectional Data Flow**: Predictable state management
- **Virtual DOM**: Fast and efficient updates

---

# Problem vs Solution: Direct DOM Manipulation

### Problem: Imperative (telling HOW to do it)

````md magic-move
```javascript
// Traditional: Step-by-step instructions
const button = document.createElement("button");
button.textContent = "Click me";
button.className = "btn-primary";
```

```javascript
// Traditional: Step-by-step instructions
const button = document.createElement("button");
button.textContent = "Click me";
button.className = "btn-primary";
button.onclick = function () {
  const div = document.getElementById("message");
  div.textContent = "Button clicked!";
  div.style.display = "block";
};
```

```javascript
// Traditional: Step-by-step instructions
const button = document.createElement("button");
button.textContent = "Click me";
button.className = "btn-primary";
button.onclick = function () {
  const div = document.getElementById("message");
  div.textContent = "Button clicked!";
  div.style.display = "block";
};
document.getElementById("container").appendChild(button);
```
````

---

# Solution: Declarative (describing WHAT you want)

````md magic-move
```jsx
// React: Describe the final result
function App() {
  const [message, setMessage] = useState("");
}
```

```jsx
// React: Describe the final result
function App() {
  const [message, setMessage] = useState("");

  return (
    <div>
      <button onClick={() => setMessage("Button clicked!")}>Click me</button>
    </div>
  );
}
```

```jsx
// React: Describe the final result
function App() {
  const [message, setMessage] = useState("");

  return (
    <div>
      <button onClick={() => setMessage("Button clicked!")}>Click me</button>
      {message && <div>{message}</div>}
    </div>
  );
}
```
````

---

# Problem: Hard to Maintain

```javascript {all|4-8|9-13|17}
// Traditional: Everything in one place, repeated everywhere
function renderUserList() {
  const html = `
    <div class="user-card">
      <img src="${user1.avatar}" />
      <h3>${user1.name}</h3>
      <p>${user1.email}</p>
      <button onclick="followUser(${user1.id})">Follow</button>
    </div>
    <div class="user-card">
      <img src="${user2.avatar}" />
      <h3>${user2.name}</h3>
      <p>${user2.email}</p>
      <button onclick="followUser(${user2.id})">Follow</button>
    </div>
  `;
  document.getElementById("users").innerHTML = html;
}
```

---

# Solution: Reusable Components

```jsx {all|2-10|13-20|16}
// React: Break into reusable pieces
function UserCard({ user, onFollow }) {
  return (
    <div className="user-card">
      <img src={user.avatar} alt={user.name} />
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onFollow(user.id)}>Follow</button>
    </div>
  );
}

function UserList({ users }) {
  return (
    <div>
      {users.map((user) => (
        <UserCard key={user.id} user={user} onFollow={followUser} />
      ))}
    </div>
  );
}
```

---

# Problem: State Out of Sync

````md magic-move
```javascript
// Traditional: State can change from anywhere
let cartItems = [];
let totalPrice = 0;
let itemCount = 0;
```

```javascript
// Traditional: State can change from anywhere
let cartItems = [];
let totalPrice = 0;
let itemCount = 0;

function addToCart(item) {
  cartItems.push(item);
  totalPrice += item.price; // Manual calculation
  itemCount++; // Manual update
}
```

```javascript
// Traditional: State can change from anywhere
let cartItems = [];
let totalPrice = 0;
let itemCount = 0;

function addToCart(item) {
  cartItems.push(item);
  totalPrice += item.price; // Manual calculation
  itemCount++; // Manual update

  // Update UI in multiple places
  document.getElementById("cart-count").textContent = itemCount;
  document.getElementById("total").textContent = "$" + totalPrice;
  renderCartItems();
}
```

```javascript
// Traditional: State can change from anywhere
let cartItems = [];
let totalPrice = 0;
let itemCount = 0;

function addToCart(item) {
  cartItems.push(item);
  totalPrice += item.price; // Manual calculation
  itemCount++; // Manual update

  // Update UI in multiple places
  document.getElementById("cart-count").textContent = itemCount;
  document.getElementById("total").textContent = "$" + totalPrice;
  renderCartItems();
}

function removeFromCart(itemId) {
  const item = cartItems.find((i) => i.id === itemId);
  cartItems = cartItems.filter((i) => i.id !== itemId);
  // BUG: Forgot to update totalPrice and itemCount!
}
```
````

---

# Solution: Single Source of Truth

````md magic-move
```jsx
// React: State flows down, changes flow up
function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);
}
```

```jsx
// React: State flows down, changes flow up
function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);

  // Derived values - always in sync
  const totalPrice = cartItems.reduce((sum, item) => sum + item.price, 0);
  const itemCount = cartItems.length;
}
```

```jsx
// React: State flows down, changes flow up
function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);

  // Derived values - always in sync
  const totalPrice = cartItems.reduce((sum, item) => sum + item.price, 0);
  const itemCount = cartItems.length;

  const addToCart = (item) => {
    setCartItems([...cartItems, item]);
    // Everything else updates automatically!
  };
}
```

```jsx
// React: State flows down, changes flow up
function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);

  // Derived values - always in sync
  const totalPrice = cartItems.reduce((sum, item) => sum + item.price, 0);
  const itemCount = cartItems.length;

  const addToCart = (item) => {
    setCartItems([...cartItems, item]);
    // Everything else updates automatically!
  };

  const removeFromCart = (itemId) => {
    setCartItems(cartItems.filter((item) => item.id !== itemId));
    // totalPrice and itemCount automatically recalculate
  };
}
```

```jsx
// React: State flows down, changes flow up
function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);

  // Derived values - always in sync
  const totalPrice = cartItems.reduce((sum, item) => sum + item.price, 0);
  const itemCount = cartItems.length;

  const addToCart = (item) => {
    setCartItems([...cartItems, item]);
    // Everything else updates automatically!
  };

  const removeFromCart = (itemId) => {
    setCartItems(cartItems.filter((item) => item.id !== itemId));
    // totalPrice and itemCount automatically recalculate
  };

  return (
    <div>
      <CartHeader count={itemCount} total={totalPrice} />
      <CartItems items={cartItems} onRemove={removeFromCart} />
    </div>
  );
}
```
````

---

# What is the Virtual DOM?

**Virtual DOM** is a lightweight JavaScript representation of the actual DOM.

Think of it like this:

- **Real DOM**: The actual HTML elements in the browser (slow to update)
- **Virtual DOM**: A JavaScript object that represents the DOM (fast to update)

---

# How Virtual DOM Works

1. React keeps a virtual copy of the DOM in memory
2. When state changes, React creates a NEW virtual DOM
3. React compares (diffs) the new virtual DOM with the old one
4. React calculates the minimal changes needed
5. React updates ONLY those specific parts in the real DOM

**Why it matters:**

- Updating the real DOM is expensive (slow)
- JavaScript operations are cheap (fast)
- By computing changes in memory first, React minimizes real DOM updates
- Result: Better performance, especially for complex UIs

---

# Virtual DOM Example

```jsx {all|2-5|8-20}
// Your JSX
<div className="container">
  <h1>Hello</h1>
  <p>Welcome!</p>
</div>

// Gets converted to Virtual DOM (JavaScript object)
{
  type: 'div',
  props: {
    className: 'container',
    children: [
      {
        type: 'h1',
        props: { children: 'Hello' }
      },
      {
        type: 'p',
        props: { children: 'Welcome!' }
      }
    ]
  }
}
```

---

# Problem: Manual, Inefficient Updates

```javascript {all|3|5-6|8-13}
// Traditional: Update everything, even unchanged parts
function updateTodoList(todos) {
  const list = document.getElementById("todo-list");

  // Nuclear option: Destroy and rebuild everything
  list.innerHTML = "";

  todos.forEach((todo) => {
    const li = document.createElement("li");
    li.className = todo.completed ? "completed" : "";
    // ... create all elements
    list.appendChild(li);
  });

  // Problem: Rebuilding everything is slow
  // - Lost focus on inputs
  // - Lost scroll position
  // - Wasteful for large lists
}
```

---

# Solution: Virtual DOM - Only Update What Changed

```jsx {all|2-5|7-16|19-28|23-24}
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: "Learn React", completed: false },
    { id: 2, text: "Build app", completed: false },
  ]);

  const toggleTodo = (id) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo,
      ),
    );
    // React's Virtual DOM:
    // 1. Creates new virtual tree
    // 2. Diffs with old tree
    // 3. Only updates the ONE checkbox that changed
  };

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          <input type="checkbox" checked={todo.completed}
            onChange={() => toggleTodo(todo.id)} />
          <span>{todo.text}</span>
        </li>
      ))}
    </ul>
  );
}
```

---

# Quick Comparison Summary

| Problem | Traditional Approach | React Solution |
|---------|---------------------|----------------|
| **Direct DOM manipulation** | `document.getElementById()`, manual updates | **Declarative**: Describe UI with JSX |
| **Hard to maintain** | Copy-paste HTML/JS everywhere | **Component-Based**: Reusable pieces |
| **State out of sync** | Variables scattered, manual sync | **Unidirectional Flow**: Single source of truth |
| **Repetitive & slow** | Rebuild everything on change | **Virtual DOM**: Only update what changed |

---

# Component-Based Architecture

## What is a Component?

A component is a **reusable, self-contained piece of UI**

Think of it like LEGO blocks:

- Each block has a specific purpose
- Blocks can be combined to build complex structures
- Blocks can be reused in different places

---

# Example Component Hierarchy

```text
App
├── Header
│   ├── Logo
│   └── Navigation
├── TaskList
│   ├── TaskItem
│   ├── TaskItem
│   └── TaskItem
└── Footer
```

---

# Single Responsibility Principle

**Each component should do ONE thing well**

```jsx {all|2-6|9-16}
// Bad: Component doing too much
function UserDashboard({ userId }) {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);
  const [comments, setComments] = useState([]);
  // ... 200 lines of mixed UI code
}

// Good: Separated concerns
function UserDashboard({ userId }) {
  return (
    <div>
      <UserProfile userId={userId} />
      <UserPosts userId={userId} />
      <UserComments userId={userId} />
    </div>
  );
}
```

---

# Composition Over Inheritance

```jsx {all|2-4|7-11|13-17|20-22}
// Bad: Trying to use inheritance
class Button extends Component { }
class PrimaryButton extends Button { }
class DangerButton extends Button { }

// Good: Use composition
function Button({ variant = 'primary', children, ...props }) {
  const styles = {
    primary: 'bg-blue-500 text-white',
    secondary: 'bg-gray-500 text-white',
    danger: 'bg-red-500 text-white'
  };

  return (
    <button className={styles[variant]} {...props}>
      {children}
    </button>
  );
}

// Usage
<Button variant="primary">Save</Button>
<Button variant="danger">Delete</Button>
```

---

# When to Create a New Component?

**Questions to ask yourself:**

1. **Is it used in multiple places?** → Make it a component
2. **Is it complex (>100 lines)?** → Break it down
3. **Does it have a clear purpose?** → Good candidate
4. **Can you name it clearly?** → If yes, extract it
5. **Would you explain it separately?** → It should be a component

---

# JavaScript Fundamentals Review

## Arrow Functions

```javascript {all|2-4|7|10-14}
// Traditional function
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => a + b;

// Arrow function with body
const addAndLog = (a, b) => {
  const result = a + b;
  console.log(result);
  return result;
};
```

---

# Array Methods: map()

Transform each element

```javascript {all|1-3|5-8|9}
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((num) => num * 2);
// [2, 4, 6, 8, 10]

const tasks = [
  { id: 1, title: "Learn React" },
  { id: 2, title: "Build App" },
];
const titles = tasks.map((task) => task.title);
// ['Learn React', 'Build App']
```

---

# Array Methods: filter()

Keep elements that match condition

```javascript {all|1-3|5-8|9}
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter((num) => num % 2 === 0);
// [2, 4]

const tasks = [
  { id: 1, completed: true },
  { id: 2, completed: false },
];
const activeTasks = tasks.filter((task) => !task.completed);
```

---

# Array Methods: reduce()

Combine elements into single value

```javascript {all|1-3|5-13}
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((total, num) => total + num, 0);
// 15

const tasks = [
  { id: 1, completed: true },
  { id: 2, completed: false },
  { id: 3, completed: true },
];
const completedCount = tasks.reduce(
  (count, task) => (task.completed ? count + 1 : count),
  0,
);
// 2
```

---

# Destructuring

## Object Destructuring

```javascript {all|1-5|8-9|12|15}
const task = {
  id: 1,
  title: "Learn React",
  completed: false,
};

// Instead of:
const title = task.title;
const completed = task.completed;

// Use destructuring:
const { title, completed } = task;

// With renaming:
const { title: taskTitle, completed: isDone } = task;
```

---

# Array Destructuring

```javascript {all|1|4-5|8|11}
const colors = ["red", "green", "blue"];

// Instead of:
const first = colors[0];
const second = colors[1];

// Use destructuring:
const [first, second, third] = colors;

// Skip elements:
const [first, , third] = colors;
```

---

# Spread Operator

## Array Spread

```javascript {all|1-2|5-6|9|12-13}
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Combine arrays
const combined = [...arr1, ...arr2];
// [1, 2, 3, 4, 5, 6]

// Copy array
const copy = [...arr1];

// Add to beginning
const withNew = [0, ...arr1];
// [0, 1, 2, 3]
```

---

# Object Spread

```javascript {all|1-5|8|11-14|17-20}
const task = {
  id: 1,
  title: "Learn React",
  completed: false,
};

// Copy object
const taskCopy = { ...task };

// Update property
const updatedTask = {
  ...task,
  completed: true,
};

// Add property
const taskWithPriority = {
  ...task,
  priority: "high",
};
```

---

# Template Literals

```javascript {all|1-2|5|8|11-16}
const name = "React";
const version = 18;

// Instead of:
const message = "Welcome to " + name + " version " + version;

// Use template literals:
const message = `Welcome to ${name} version ${version}`;

// Multi-line strings
const html = `
  <div>
    <h1>${name}</h1>
    <p>Version: ${version}</p>
  </div>
`;
```

---

# JSX Syntax and Transpilation

## What is JSX?

**JSX** = JavaScript XML

A syntax extension that lets you write HTML-like code in JavaScript

```jsx
// This is JSX
const element = <h1>Hello, React!</h1>;

// It looks like HTML, but it's JavaScript
```

---

# JSX is Transpiled to JavaScript

```jsx {all|2|5-9}
// You write:
const element = <h1 className="title">Hello, React!</h1>;

// Babel converts it to:
const element = React.createElement(
  "h1",
  { className: "title" },
  "Hello, React!",
);
```

---

# JSX Rules

## 1. Must return a single parent element

```jsx {all|2-5|8-12|15-19}
// Wrong
return (
  <h1>Title</h1>
  <p>Paragraph</p>
);

// Correct
return (
  <div>
    <h1>Title</h1>
    <p>Paragraph</p>
  </div>
);

// Or use Fragment
return (
  <>
    <h1>Title</h1>
    <p>Paragraph</p>
  </>
);
```

---

# 2. Use camelCase for attributes

```jsx {all|2|5}
// Wrong (HTML attribute)
<div class="container" onclick="handleClick()">

// Correct (JSX)
<div className="container" onClick={handleClick}>
```

---

# 3. JavaScript expressions in curly braces

```jsx {all|1-2|4-9}
const name = "React";
const count = 5;

<div>
  <h1>{name}</h1>
  <p>You have {count} tasks</p>
  <p>Double: {count * 2}</p>
  <p>{count > 0 ? "Tasks remaining" : "All done!"}</p>
</div>;
```

---

# 4. All tags must be closed

```jsx {all|2-4|7-9}
// Wrong
<img src="image.jpg">
<br>
<input type="text">

// Correct
<img src="image.jpg" />
<br />
<input type="text" />
```

---
layout: center
class: text-center
---

# Break (15 minutes)

☕️

---
layout: center
class: text-center
---

# Part 2: React Core Concepts

---

# Components: Functional vs Class

```javascript {all|2-6|9-11|14}
// Class Components (Legacy)
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

// Functional Components (Modern)
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Or even shorter
const Welcome = (props) => <h1>Hello, {props.name}</h1>;
```

**We focus on functional components because:**
- Simpler syntax
- Easier to understand
- Better performance
- Modern React standard (with Hooks)

---

# Unidirectional Data Flow

## What is Unidirectional Data Flow?

**Data flows in ONE direction: from parent to child**

```text
Parent Component (has state)
    ↓ (props)
Child Component
    ↓ (props)
Grandchild Component

Changes flow UP via callbacks:
    ↑ (event handlers)
Parent updates state
    ↓ (new props)
Children re-render with new data
```

---

# Why Unidirectional Flow?

1. **Predictability**: Always know where data comes from
2. **Debugging**: Easy to trace data changes
3. **Performance**: React knows exactly what to update
4. **Maintainability**: Clear data dependencies

---

# Unidirectional Flow Example

```jsx {all|2|5-9|12-15|18-21}
function App() {
  const [count, setCount] = useState(0);

  // Data flows DOWN via props
  return (
    <div>
      <Display count={count} />
      <Controls onIncrement={() => setCount(count + 1)} />
    </div>
  );
}

function Display({ count }) {
  // Receives data from parent
  return <h1>Count: {count}</h1>;
}

function Controls({ onIncrement }) {
  // Sends events UP to parent
  return <button onClick={onIncrement}>+1</button>;
}
```

---

# Data Flow Rules

## Rule 1: Props flow down

```jsx {all|2-3|6-8}
function Parent() {
  const data = "Hello";
  return <Child message={data} />; // Data flows to child
}

function Child({ message }) {
  return <div>{message}</div>; // Child receives data
}
```

---

# Rule 2: Events flow up

```jsx {all|2|4|8-11}
function Parent() {
  const [data, setData] = useState("Hello");

  return <Child onUpdate={setData} />; // Function flows down
}

function Child({ onUpdate }) {
  return (
    <button onClick={() => onUpdate("Goodbye")}>Change</button>
    // Child calls parent's function (event flows up)
  );
}
```

---

# Props: Passing Data Down

## What are Props?

**Props** (properties) are how you pass data from parent to child components

```jsx {all|3-6|10-16}
// Parent component
function App() {
  return (
    <div>
      <Welcome name="Alice" age={25} />
      <Welcome name="Bob" age={30} />
    </div>
  );
}

// Child component
function Welcome(props) {
  return (
    <div>
      <h1>Hello, {props.name}</h1>
      <p>Age: {props.age}</p>
    </div>
  );
}
```

---

# Destructuring Props

```jsx {all|2-8|11-17}
// Instead of using props.name, props.age
function Welcome(props) {
  return (
    <div>
      <h1>Hello, {props.name}</h1>
      <p>Age: {props.age}</p>
    </div>
  );
}

// Destructure in parameter
function Welcome({ name, age }) {
  return (
    <div>
      <h1>Hello, {name}</h1>
      <p>Age: {age}</p>
    </div>
  );
}
```

---

# Props Can Be Anything

```jsx {all|2|5|8|11|14|17}
// Strings
<Button text="Click me" />

// Numbers
<Counter initial={0} max={10} />

// Booleans
<Task completed={true} />

// Arrays
<List items={['Item 1', 'Item 2']} />

// Objects
<User data={{ name: 'Alice', email: 'alice@example.com' }} />

// Functions
<Button onClick={() => alert('Clicked!')} />
```

---

# Props are Read-Only

```jsx {all|3|6}
function Welcome(props) {
  // Wrong - Never modify props
  props.name = "Changed"; // ❌

  // Correct - Props are read-only
  return <h1>Hello, {props.name}</h1>; // ✅
}
```

**Rule:** All React components must act like pure functions with respect to their props.

---

# The Children Prop

```jsx {all|2-4|7-10}
// Basic children
function Card({ children }) {
  return <div className="card">{children}</div>;
}

// Usage
<Card>
  <h2>Title</h2>
  <p>Content goes here</p>
</Card>;

// The JSX between <Card> tags becomes the children prop
```

---

# State: useState Hook

## What is State?

**State** is data that changes over time in your component

The difference:

- **Props**: Passed from parent (like function arguments)
- **State**: Managed within component (like variables)

---

# Using useState

````md magic-move
```jsx
import { useState } from "react";

function Counter() {
  // Declare state variable
  const [count, setCount] = useState(0);
  //     ↑        ↑           ↑
  //   current  function   initial
  //   value    to update   value
}
```

```jsx
import { useState } from "react";

function Counter() {
  // Declare state variable
  const [count, setCount] = useState(0);
  //     ↑        ↑           ↑
  //   current  function   initial
  //   value    to update   value

  return (
    <div>
      <p>Count: {count}</p>
    </div>
  );
}
```

```jsx
import { useState } from "react";

function Counter() {
  // Declare state variable
  const [count, setCount] = useState(0);
  //     ↑        ↑           ↑
  //   current  function   initial
  //   value    to update   value

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```
````

---

# Multiple State Variables

```jsx {all|2-5|8-16}
function TaskForm() {
  const [title, setTitle] = useState("");
  const [description, setDescription] = useState("");
  const [priority, setPriority] = useState("medium");
  const [completed, setCompleted] = useState(false);

  return (
    <form>
      <input value={title} onChange={(e) => setTitle(e.target.value)} />
      <textarea value={description}
        onChange={(e) => setDescription(e.target.value)} />
      <select value={priority} onChange={(e) => setPriority(e.target.value)}>
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>
    </form>
  );
}
```

---

# State Updates are Asynchronous

```jsx {all|2|4-7|10}
function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    console.log(count); // Still shows old value!
    // State updates are async
  };

  return <button onClick={handleClick}>Count: {count}</button>;
}
```

---

# Updating State Based on Previous State

```jsx {all|2-3|6-7}
// Wrong - Can miss updates
setCount(count + 1);
setCount(count + 1); // Might not work as expected

// Correct - Use function form
setCount((prevCount) => prevCount + 1);
setCount((prevCount) => prevCount + 1); // Works correctly
```

---

# State with Objects

```jsx {all|2-6|9-11|14-18}
function UserProfile() {
  const [user, setUser] = useState({
    name: "",
    email: "",
    age: 0,
  });

  // Wrong - Loses other properties
  const updateName = (name) => {
    setUser({ name: name }); // ❌
  };

  // Correct - Spread existing properties
  const updateName = (name) => {
    setUser((prevUser) => ({
      ...prevUser,
      name: name,
    })); // ✅
  };

  return (
    <input value={user.name} onChange={(e) => updateName(e.target.value)} />
  );
}
```

---

# Event Handling in React

## Basic Event Handling

```jsx {all|2-4|6|10}
function Button() {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return <button onClick={handleClick}>Click me</button>;
}

// Or inline
function Button() {
  return <button onClick={() => alert("Button clicked!")}>Click me</button>;
}
```

---

# Common Events

```jsx {all|5|8-12|15-21}
function EventExamples() {
  return (
    <div>
      {/* Click events */}
      <button onClick={() => console.log("Clicked")}>Click</button>

      {/* Input events */}
      <input
        onChange={(e) => console.log(e.target.value)}
        onFocus={() => console.log("Focused")}
        onBlur={() => console.log("Blurred")}
      />

      {/* Form events */}
      <form onSubmit={(e) => {
          e.preventDefault();
          console.log("Submitted");
        }}
      >
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}
```

---

# Controlled Components

## What is a Controlled Component?

A form element whose value is controlled by React state

```jsx {all|2|5|8-12}
function ControlledInput() {
  const [value, setValue] = useState("");

  // Uncontrolled - React doesn't know the value
  return <input />; // ❌

  // Controlled - React controls the value
  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  ); // ✅
}
```

---

# Complete Form Example

## State & Handlers

```jsx {all|2-6|8-13}
function TaskForm() {
  const [formData, setFormData] = useState({
    title: "",
    description: "",
    priority: "medium",
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: value,
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Form data:", formData);
  };
  // ...
}
```

---

# Complete Form Example

## JSX Render

```jsx {all|2-5}
function TaskForm() {
  // ... state and handlers from previous slide

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="title"
        value={formData.title}
        onChange={handleChange}
      />
      <button type="submit">Add Task</button>
    </form>
  );
}
```

---

# useEffect Hook

## What is useEffect?

**useEffect** lets you perform side effects in function components:

- Fetching data
- Subscriptions
- Manually changing the DOM
- Timers
- localStorage operations

---

# Basic useEffect

```jsx {all|1|4-5|8-10|13-15}
import { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  // Runs after every render
  useEffect(() => {
    console.log("Component rendered or updated");
    document.title = `Count: ${count}`;
  });

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

---

# useEffect with Dependencies

```jsx {all|2-3|6-8|11-13|16-18}
function Example() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("");

  // Runs only when count changes
  useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  // Runs only on mount (once)
  useEffect(() => {
    console.log("Component mounted");
  }, []);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <input value={name} onChange={(e) => setName(e.target.value)} />
    </div>
  );
}
```

---

# useEffect with Cleanup

```jsx {all|2|4-12|14}
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Set up interval
    const interval = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    // Cleanup function
    return () => {
      clearInterval(interval);
      console.log("Timer cleaned up");
    };
  }, []); // Empty array = run once on mount

  return <div>Seconds: {seconds}</div>;
}
```

---

# localStorage with useEffect

```jsx {all|2|5-9|12-14|17-21}
function TaskList() {
  const [tasks, setTasks] = useState([]);

  // Load from localStorage on mount
  useEffect(() => {
    const savedTasks = localStorage.getItem("tasks");
    if (savedTasks) {
      setTasks(JSON.parse(savedTasks));
    }
  }, []);

  // Save to localStorage when tasks change
  useEffect(() => {
    localStorage.setItem("tasks", JSON.stringify(tasks));
  }, [tasks]);

  return (
    <div>
      {tasks.map((task) => (
        <div key={task.id}>{task.title}</div>
      ))}
    </div>
  );
}
```

---

# Rendering Lists with .map()

## Basic List Rendering

```jsx {all|2-6|9-13}
function TaskList() {
  const tasks = [
    { id: 1, title: "Learn React" },
    { id: 2, title: "Build App" },
    { id: 3, title: "Deploy" },
  ];

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>{task.title}</li>
      ))}
    </ul>
  );
}
```

---

# Why Keys are Important

```jsx {all|2-4|7-9|12-14}
// Wrong - No key
{tasks.map((task) => (
  <li>{task.title}</li>
))} // ❌

// Wrong - Index as key (avoid if list changes)
{tasks.map((task, index) => (
  <li key={index}>{task.title}</li>
))} // ❌

// Correct - Unique ID as key
{tasks.map((task) => (
  <li key={task.id}>{task.title}</li>
))} // ✅
```

**Why?** Keys help React identify which items have changed, been added, or removed.

---

# Complete List Example

## State & Handlers

```jsx {all|2-5|7-14|16-18}
function TaskList() {
  const [tasks, setTasks] = useState([
    { id: 1, title: "Learn React", completed: false },
    { id: 2, title: "Build App", completed: false },
  ]);

  const toggleTask = (id) => {
    setTasks(
      tasks.map((task) =>
        task.id === id
          ? { ...task, completed: !task.completed }
          : task,
      ),
    );
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter((task) => task.id !== id));
  };
  // ...
}
```

---

# Complete List Example

## Rendering with .map()

```jsx {all|2-4|5-11}
function TaskList() {
  // ... state and handlers from previous slide

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>
          <input type="checkbox" checked={task.completed}
            onChange={() => toggleTask(task.id)} />
          <span>{task.title}</span>
          <button onClick={() => deleteTask(task.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

---

# Conditional Rendering Patterns

## Ternary Operator

```jsx {all|2-7}
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? (
        <h1>Welcome back!</h1>
      ) : (
        <h1>Please sign in</h1>
      )}
    </div>
  );
}
```

---

# Logical && Operator

```jsx {all|5|6-12}
function TaskList({ tasks }) {
  return (
    <div>
      <h2>Tasks</h2>
      {tasks.length === 0 && <p>No tasks yet. Add one!</p>}
      {tasks.length > 0 && (
        <ul>
          {tasks.map((task) => (
            <li key={task.id}>{task.title}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

---

# Early Return

```jsx {all|2-4|6-8|10-14}
function TaskItem({ task }) {
  if (!task) {
    return <div>No task found</div>;
  }

  if (task.deleted) {
    return null; // Render nothing
  }

  return (
    <div>
      <h3>{task.title}</h3>
      <p>{task.description}</p>
    </div>
  );
}
```

---

# Live Demo: Building a Simple Counter

```jsx {all|1|4-5|7-9|12-28|13|15-22|24-27}
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  const [step, setStep] = useState(1);

  const increment = () => setCount(count + step);
  const decrement = () => setCount(count - step);
  const reset = () => setCount(0);

  return (
    <div>
      <h1>Counter: {count}</h1>
      <div>
        <label>
          Step:
          <input
            type="number"
            value={step}
            onChange={(e) => setStep(Number(e.target.value))}
          />
        </label>
      </div>
      <div>
        <button onClick={decrement}>-{step}</button>
        <button onClick={reset}>Reset</button>
        <button onClick={increment}>+{step}</button>
      </div>
    </div>
  );
}
```

---
layout: center
class: text-center
---

# Lunch Break

🍕

---
layout: center
class: text-center
---

# Practice Session: Building TaskMaster v1

---

# TaskMaster v1 Requirements

**Core Features:**

- Display list of tasks
- Add new task with title and description
- Mark task as complete/incomplete
- Delete tasks
- Filter tasks (All, Active, Completed)
- Edit existing tasks
- Persist to localStorage
- Basic styling

---

# Technical Requirements

- **Minimum 3 components**
- Use **useState** for state management
- Use **useEffect** for localStorage
- Proper event handling
- **Key prop** for list items

---

# Suggested Component Structure

```text
App
├── TaskForm (add new task)
├── TaskFilter (filter buttons)
└── TaskList
    └── TaskItem (individual task)
```

---

# Data Structure

```javascript
const task = {
  id: 1, // unique identifier
  title: "Learn React", // task title
  description: "Study...", // task description
  completed: false, // completion status
  createdAt: Date.now(), // timestamp
};

const tasks = [task1, task2, task3];
```

---

# Step 1: Project Setup

```bash
# Create React app
npm create vite@latest taskmaster -- --template react
cd taskmaster
npm install
npm run dev
```

---

# Step 2: Create App Component

```jsx
// App.jsx
import { useState, useEffect } from "react";
import "./App.css";

function App() {
  const [tasks, setTasks] = useState([]);
  const [filter, setFilter] = useState("all");

  return (
    <div className="app">
      <h1>TaskMaster</h1>
      {/* Components will go here */}
    </div>
  );
}

export default App;
```

---

# Common Issues & Solutions

## Issue 1: State not updating

```jsx {all|2|5-9}
// Wrong
tasks[0].completed = true; // ❌

// Correct
setTasks(
  tasks.map((task) =>
    task.id === targetId ? { ...task, completed: true } : task,
  ),
); // ✅
```

---

# Issue 2: Missing keys in lists

```jsx {all|2-4|7-9}
// Wrong
{tasks.map((task) => (
  <li>{task.title}</li>
))} // ❌

// Correct
{tasks.map((task) => (
  <li key={task.id}>{task.title}</li>
))} // ✅
```

---

# Issue 3: Form not preventing default

```jsx {all|2-4|7-10}
// Wrong
const handleSubmit = () => {
  addTask(newTask);
}; // ❌

// Correct
const handleSubmit = (e) => {
  e.preventDefault();
  addTask(newTask);
}; // ✅
```

---

# Issue 4: Not validating input

```jsx {all|2|5-9}
// Wrong
onAddTask({ title, description }); // ❌

// Correct
if (!title.trim()) return;
onAddTask({
  title: title.trim(),
  description: description.trim(),
}); // ✅
```

---

# Code Quality Checklist

- ✅ Components are properly organized
- ✅ Functions have descriptive names
- ✅ State is updated immutably
- ✅ Event handlers prevent default when needed
- ✅ Forms validate input
- ✅ Lists have proper keys
- ✅ Code is readable and well-formatted
- ✅ localStorage works correctly
- ✅ No console errors

---

# Homework

## Required:

1. Complete all basic features if not finished
2. Add one bonus feature:
   - Search functionality
   - Task categories
   - Due dates
   - Priority levels
3. Read Next.js documentation (intro pages)
4. Write a brief reflection

---

# Bonus Challenges

- Add task sorting (by date, priority, alphabetical)
- Implement undo/redo functionality
- Add keyboard shortcuts (Ctrl+N for new task)
- Create dark mode toggle
- Add animations for task operations

---

# Key Concepts Covered Today

- React components and composition
- JSX syntax
- Props and prop drilling
- useState hook
- useEffect hook
- Event handling
- Form handling (controlled components)
- Array methods (.map, .filter)
- Conditional rendering
- localStorage API

---

# Preview: Next Week

**Session 2: Next.js + Routing + Detail Pages**

We'll learn:
- Client-side routing
- Next.js file-based routing
- Dynamic routes with parameters
- Multi-page navigation

We'll build:
- Migrate TaskMaster to Next.js
- Add task detail pages
- Implement categories

---
layout: center
class: text-center
---

# Questions?

🤔

---

# Resources

## Documentation

- [React Official Docs](https://react.dev/)
- [React Hooks Reference](https://react.dev/reference/react)
- [JavaScript Array Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

## Tools

- [React DevTools Chrome Extension](https://chrome.google.com/webstore/detail/react-developer-tools/)
- [Vite Documentation](https://vitejs.dev/)

---
layout: end
---

# Thank You!

**See you next week!**

Remember:
- Complete your homework
- Review concepts that were unclear
- Experiment with the code
- Ask questions in our discussion forum

**Happy coding!** 🚀
