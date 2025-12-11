### 1. **What is JS?**

JavaScript (JS) is a high-level, dynamic, prototype-based, and interpreted (line by line, at runtime) programming language primarily used for web development. It enables interactive web pages by running scripts in the browser. Beyond the browser, JavaScript powers back-end development (via Node.js), mobile apps, and more. It is single-threaded and event-driven, supporting both functional and object-oriented paradigms.

---

### 2. **Difference Between JS and TS?**

- **TypeScript (TS)** is a superset of JavaScript with optional static typing, developed by Microsoft. It compiles to plain JavaScript.
- **Key differences:**
    - **Static Typing:** TS allows type annotations, helping catch errors at compile time.
    - **Enhanced Features:** TS adds interfaces, enums, generics, and more.
    - **Tooling:** TS integrates with IDEs for better autocomplete and error detection.
    - **Compatibility:** TS code compiles to JS, making it compatible with existing JS libraries.

---

### 3. **What is ECMAScript?**

ECMAScript (ES) is the standardized specification that defines the behavior of JavaScript. Managed by ECMA International, it governs syntax, semantics, and features. Each version (ES5, ES6/ES2015, etc.) introduces new features like `let`, `const`, classes, async/await, and modules.

---

### 4. **What is Polyfills?**

A polyfill is a piece of code that implements modern features in older browsers or environments that do not natively support them. For example, `Promise` or `fetch` APIs might be polyfilled using libraries to ensure compatibility with older browsers.

---

### 5. **How Does Garbage Collection Work in JS?**

The JavaScript Garbage Collector (GC) automatically frees up memory by removing objects that are no longer referenced.

- **Mark-and-Sweep Algorithm:** The most common method:
    1. GC marks reachable objects starting from the root (e.g., global objects).
    2. Unmarked objects are considered unreachable and are cleaned up.
- Memory leaks can occur if references are stored unintentionally (e.g., circular references, global variables, DOM elements that are removed from the visible document but still referenced in JavaScript, Event Listeners Not Removed, Closures Holding References, Timers and Intervals Not Cleared, ).

---

### 6. **Event Loop, Microtasks, and Macrotasks?**

- **Event Loop:** The mechanism that handles asynchronous operations by queuing and executing tasks.
- **Microtasks:** High-priority tasks executed immediately after the current execution context (e.g., `Promise.then`, `queueMicrotask`).
- **Macrotasks:** Lower-priority tasks (e.g., `setTimeout`, `setInterval`, I/O operations) queued after microtasks are cleared.

---

### 7. **Tell Me About HEAP and STACK Memory**

- **Heap:** Used for dynamic memory allocation, where objects and closures are stored.
- **Stack:** Stores function calls, local variables, and execution contexts. It operates in a Last In, First Out (LIFO) manner.

---

### 8. **What is a Garbage Collector? Is There One in JS?**

Yes, JavaScript has a garbage collector to manage memory automatically. It identifies objects no longer in use and deallocates their memory, primarily using the mark-and-sweep algorithm.

---

### 9. **What TypeScript Features Do You Use?**

- **Interfaces and Types:** Define structured contracts.
- **Generics:** Create reusable and type-safe components.
- **Enums:** Represent constant sets of values.
- **Type Guards:** Narrow down types at runtime.
- **Decorators:** Add metadata or modify behavior (especially in frameworks like Angular).

---

### 10. **Async/Await?**

`async/await` simplifies asynchronous code, making it look synchronous.

- **Async:** Declares a function that always returns a `Promise`.
- **Await:** Pauses execution until the promise resolves.
- **Difference from Promises:** Improved readability and error handling using `try-catch`.
- **API Support:** Many modern JS APIs, like `fetch`, support promises and can be used with `async/await`.

---

### 11. **Promise?**

A `Promise` is an object representing the eventual result of an asynchronous operation. It can be in one of three states:

- **Pending:** Initial state.
- **Fulfilled:** Operation completed successfully.
- **Rejected:** Operation failed.

---

### 12. **DOM?**

The Document Object Model (DOM) is a representation of an HTML document as a tree structure, where each node corresponds to an element or text. JavaScript manipulates the DOM to create dynamic and interactive web content.

---

### 13. **How Does Inheritance Work in JS (Prototypes)?**

JavaScript uses prototype-based inheritance.

- Each object has a hidden `[[Prototype]]` linking it to another object.
- Prototypes allow objects to inherit properties and methods.  
    For example:

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function () {
  console.log(`${this.name} makes a sound.`);
};

const dog = new Animal('Dog');
dog.speak(); // Inherits from Animal.prototype
```

---

### 14. **What is 'this'?**

`this` refers to the context in which a function is executed.

- **Global scope:** Refers to `window` or `global` object.
- **Object method:** Refers to the object calling the method.
- **Arrow functions:** Lexically bind `this` from their enclosing context.

---

### 15. **Strict Mode**

`"use strict"` enables stricter parsing and error handling for JavaScript.

- Prevents usage of undeclared variables.
- Eliminates `this` binding to `global` in functions.
- Throws errors for certain silent failures.

---

### 16. **Closures**

A closure is a function that remembers its lexical scope even when executed outside that scope.  
Example:

```javascript
function outer() {
  let counter = 0;
  return function inner() {
    counter++;
    return counter;
  };
}
const increment = outer();
console.log(increment()); // 1
console.log(increment()); // 2
```

Closures are powerful for data encapsulation and managing state.

---

### 17. **What is the difference between `var`, `let`, and `const`?**

- **`var`:**
    - Function-scoped.
    - Hoisted but initialized to `undefined`.
    - Can be re-declared and updated.
- **`let`:**
    - Block-scoped.
    - Hoisted but not initialized (temporal dead zone).
    - Can be updated but not re-declared in the same scope.
- **`const`:**
    - Block-scoped.
    - Must be initialized during declaration.
    - Immutable binding (value can change if it's an object, but the reference cannot).

---

### 18. **Explain the difference between synchronous and asynchronous programming.**

- **Synchronous:** Tasks are executed sequentially, blocking further execution until the current task completes.
- **Asynchronous:** Tasks are non-blocking and may be executed in parallel, enabling other operations to continue while waiting for a result (e.g., promises, async/await, callbacks).

---

### 19. **What are higher-order functions?**

A higher-order function is a function that:

- Takes other functions as arguments, or
- Returns a function as its result.

Example:

```javascript
function map(array, callback) {
  return array.map(callback);
}
map([1, 2, 3], (x) => x * 2); // [2, 4, 6]
```

---

### 20. **What are JavaScript modules?**

Modules allow code to be organized into reusable, isolated blocks.

- **ES6 Modules:** Use `export` and `import`.
- **CommonJS Modules:** Use `module.exports` and `require` (Node.js).

---

### 21. **What is the difference between `null` and `undefined`?**

- **`null`:** Explicitly set to indicate the absence of a value.
- **`undefined`:** Default value for variables that are declared but not initialized or missing function arguments.

---

### 22. **What is the difference between shallow and deep copy?**

- **Shallow Copy:** Copies only the first level of properties. Nested objects retain references to the original.
- **Deep Copy:** Creates a new instance of all nested objects.  
    Example for deep copy:

```javascript
const deepClone = JSON.parse(JSON.stringify(obj));
```

---

### 23. **What are arrow functions, and how are they different from regular functions?**

- Arrow functions are a shorthand for defining functions, introduced in ES6.
- Differences:
    - Do not have their own `this` binding (inherits from enclosing scope).
    - Cannot be used as constructors (`new`).
    - No `arguments` object.

---

### 24. **What are the different types of errors in JavaScript?**

1. **Syntax Errors:** Mistakes in code structure.
2. **Runtime Errors:** Errors during execution (e.g., `TypeError`, `ReferenceError`).
3. **Logical Errors:** Code runs without crashing but produces incorrect results.

---

### 25. **What is the difference between == and === ?**

- **== (Abstract Equality):** Performs type conversion before comparison.
- **=== (Strict Equality):** Compares without type conversion.

Example:

```javascript
'5' == 5; // true
'5' === 5; // false
```

---

### 26. **What is the difference between call, apply, and bind?**

- **`call`:** Invokes a function with a specific `this` value and arguments passed individually.
- **`apply`:** Similar to `call` but arguments are passed as an array.
- **`bind`:** Returns a new function with `this` and arguments pre-bound.

---

### 27. **What is currying?**

Currying transforms a function with multiple arguments into a sequence of functions each taking a single argument.  
Example:

```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    } else {
      return (...nextArgs) => curried(...args, ...nextArgs);
    }
  };
}
```

---

### 28. **What are Web APIs?**

Web APIs are browser-provided functionalities that allow JavaScript to interact with the environment, such as:

- **DOM API:** Manipulating the document.
- **Fetch API:** Making HTTP requests.
- **Geolocation API:** Accessing location data.
- **WebSockets API:** Enabling real-time communication.

---

### 29. **What is the purpose of the `async` attribute in `<script>` tags?**

- Scripts with `async` load in parallel to the HTML parsing and execute as soon as they're ready, without waiting for other scripts.

---

### 30. **What are Web Workers?**

Web Workers enable multithreading in JavaScript by running scripts in the background. This prevents blocking the main thread for intensive tasks.

---

### 31. **Explain the debounce and throttle techniques.**

- **Debounce:** Ensures a function is executed only after a specified delay since the last call.
- **Throttle:** Ensures a function is executed at most once within a specified time interval.

---

### 32. **What are Singleton Patterns in JavaScript?**

Singletons restrict a class to a single instance, providing a global point of access.  
Example:

```javascript
const Singleton = (function () {
  let instance;
  return {
    getInstance() {
      if (!instance) {
        instance = { name: 'Singleton Instance' };
      }
      return instance;
    },
  };
})();
```

---

### 33. **What is the difference between deep and shallow equality?**

- **Shallow Equality:** Compares only the first level of properties.
- **Deep Equality:** Recursively checks all properties, including nested objects.

---

### 34. **What are Tagged Templates?**

Tagged templates allow you to parse template literals with a custom function.  
Example:

```javascript
function tag(strings, ...values) {
  return strings.reduce((result, str, i) => result + str + (values[i] || ''), '');
}
tag`Hello, ${name}!`; // Custom parsing
```

---

### 35. **What are JavaScript design patterns you frequently use?**

- Singleton
- Module
- Factory
- Observer
- Strategy
- Prototype

---

### 36. **Explain memory leaks in JavaScript and how to avoid them.**

Memory leaks occur when objects are no longer needed but not garbage collected.

- Common causes:
    - Unused global variables.
    - Circular references.
    - Detached DOM elements.
- Solutions:
    - Use `WeakMap`/`WeakSet` for weak references.
    - Remove event listeners when not needed.
    - Minimize global variables.

---


# **Testing**
### **1. Which types of testing do you know, and which tools have you used?**

#### **Types of Testing:**

1. **Unit Testing:**
    
    - Tests individual functions or modules in isolation.
    - Ensures the smallest units of code work as expected.
    - Tools: **Jest**, **Vitest**, **Mocha**, **QUnit**.
2. **Integration Testing:**
    
    - Tests how different modules or components interact with each other.
    - Ensures the integration points work as expected.
    - Tools: **Jest**, **Vitest**, **Cypress**, **Playwright**.
3. **End-to-End (E2E) Testing:**
    
    - Tests the complete flow of an application from start to finish.
    - Simulates user interactions in a real or headless browser.
    - Tools: **Cypress**, **Playwright**, **Selenium**, **Puppeteer**.
4. **Component Testing:**
    
    - Tests individual UI components in isolation.
    - Verifies the rendering and behavior of the component.
    - Tools: **React Testing Library**, **Enzyme**, **Vue Test Utils**, **Jest**.
5. **Performance Testing:**
    
    - Evaluates how well an application performs under certain conditions (load, stress).
    - Tools: **Lighthouse**, **WebPageTest**, **k6**, **JMeter**.
6. **Regression Testing:**
    
    - Ensures new code changes don’t break existing functionality.
    - Tools: **Jest**, **Snapshot Testing**.
7. **Accessibility Testing:**
    
    - Ensures the application is usable by people with disabilities.
    - Tools: **axe-core**, **Lighthouse**, **Wave**.
8. **Visual Regression Testing:**
    
    - Compares screenshots of UI components or pages to detect visual changes.
    - Tools: **Percy**, **Applitools**, **Chromatic**.

---

### **2. Tests: e2e, unit, component, and tools**

#### **Unit Testing:**

- Focus: Tests isolated units of logic, such as functions or methods.
- Tools:
    - **Jest**: Popular for unit tests with a simple setup and powerful mocking capabilities.
    - **Vitest**: A faster alternative to Jest, optimized for Vite-based projects.
- Example:
    
    ```javascript
    test("adds two numbers", () => {
      expect(add(2, 3)).toBe(5);
    });
    ```
    

#### **Component Testing:**

- Focus: Tests individual UI components in isolation, including their rendering, behavior, and interactions.
- Tools:
    - **React Testing Library**: Focuses on testing user interactions and DOM updates.
    - **Enzyme**: Provides options for shallow and deep rendering of React components.
    - **Vue Test Utils**: For Vue components.
- Example:
    
    ```javascript
    render(<Button label="Click Me" />);
    expect(screen.getByText("Click Me")).toBeInTheDocument();
    ```
    

#### **End-to-End (E2E) Testing:**

- Focus: Simulates real user scenarios to test the entire application flow.
- Tools:
    - **Cypress**: Easy-to-use for E2E tests with a rich GUI for debugging.
    - **Playwright**: Robust cross-browser testing with support for multiple browser contexts.
    - **Selenium**: A long-standing tool for browser automation.
- Example (Cypress):
    
    ```javascript
    cy.visit("/login");
    cy.get("input[name='username']").type("user123");
    cy.get("input[name='password']").type("password123");
    cy.get("button[type='submit']").click();
    cy.contains("Welcome, user123");
    ```
    

#### **Supporting Tools:**

1. **Happy-dom/JSdom:**
    
    - **Happy-dom:** A fast, lightweight DOM implementation for Node.js.
    - **JSdom:** A complete DOM and HTML environment emulating a browser for testing in Node.js.
2. **Shallow Rendering:**
    
    - Renders a component without rendering its children. Useful for unit testing components in isolation.
    - Tools: **Enzyme**, **React Testing Library** (limited support).

---

### **Comparison of Tools:**

|**Aspect**|**Jest**|**Vitest**|**Cypress**|**Playwright**|
|---|---|---|---|---|
|**Purpose**|Unit, Integration|Unit, Integration|E2E Testing|E2E Testing|
|**Speed**|Moderate|Very Fast|Moderate|Moderate|
|**Ease of Setup**|Easy|Easy|Easy|Moderate|
|**Browser Testing**|No|No|Yes|Yes|

This breakdown provides a robust foundation for answering testing-related questions in interviews!