**Strict mode** is a way to opt into a restricted variant of JavaScript. It makes the code more secure and prevents certain actions that are prone to bugs or unsafe behavior. It was introduced in ECMAScript 5.

---

### **Why Use Strict Mode?**
1. **Avoids Silent Errors**: Converts some JavaScript mistakes into explicit errors, making bugs easier to catch.
2. **Improves Performance**: Enables JavaScript engines to optimize code execution.
3. **Future-proof Code**: Disallows syntax likely to be problematic in future ECMAScript versions.

---

### **How to Enable Strict Mode?**

#### 1. **In Pure JavaScript (Browser or Node.js)**

- **Global Scope**:
  Place `"use strict";` at the top of your script file. This enables strict mode for the entire script.

```javascript
"use strict";
let x = 10;
// x = 20; // Error in strict mode: Assignment to undeclared variable
```

- **Function Scope**:
  Place `"use strict";` inside a function to limit it to that function.

```javascript
function myFunction() {
  "use strict";
  // code here runs in strict mode
}
```

---

#### 2. **Node.js Environment**

- Strict mode is **enabled by default** in ECMAScript modules (.[[mjs]] or files with `type: module` in `package.json`).
- For CommonJS (`.js`) files, you need to explicitly declare `"use strict";`.

---

#### 3. **In Frameworks**

- **React**: React uses strict mode in its development environment via the `<React.StrictMode>` wrapper. This does not enforce strict mode in JavaScript but helps identify unsafe lifecycle methods or deprecated code.

```javascript
import React from "react";
import ReactDOM from "react-dom";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

- **Vue, Angular**: These frameworks enforce strict practices through their own conventions but don't automatically enable JavaScript's strict mode.

---

### **Where is Strict Mode Used by Default?**

- **Modules (`import/export`)**: Strict mode is always enabled in ECMAScript modules.
- **Class Constructors**: All code within JavaScript class constructors runs in strict mode.

---

### **Cases that Break Code in Strict Mode**

1. **Assigning to Undeclared Variables**:
   ```javascript
   "use strict";
   x = 10; // Error: x is not defined
   ```

2. **Assigning to Read-only Properties**:
   ```javascript
   "use strict";
   const obj = Object.freeze({ prop: 42 });
   obj.prop = 77; // Error
   ```

3. **Using `delete` on Non-deletable Properties**:
   ```javascript
   "use strict";
   delete Object.prototype; // Error
   ```

4. **Duplicate Parameter Names**:
   ```javascript
   "use strict";
   function sum(a, a) { // Error
     return a + a;
   }
   ```

5. **Octal Syntax**:
   ```javascript
   "use strict";
   const num = 010; // Error: Octal literals are not allowed
   ```

6. **Restricted Keywords for Variable Names**:
   ```javascript
   "use strict";
   const public = 123; // Error: Reserved keyword
   ```

7. **`with` Statement**:
   ```javascript
   "use strict";
   with (Math) { // Error: with is not allowed
     const x = PI;
   }
   ```

---

### **How to Use Strict Mode in Different Scenarios**

#### Browser
1. Place `"use strict";` at the top of your `<script>` file.
2. When using a `<script>` tag with `type="module"`, strict mode is applied automatically.

#### Node.js
1. Use `"use strict";` in CommonJS files.
2. Use `.mjs` files or set `type: module` in `package.json`.

#### Frameworks
Strict mode often depends on the environment configuration or project setup. For custom behavior, explicitly add `"use strict";` in your files.

---

### **Advantages and Common Use Cases**
1. **Prevent Global Leaks**: Helps avoid accidental global variable declarations.
2. **Secure JavaScript**: Avoids potential vulnerabilities like `this` being bound to the global object.
3. **Catch Errors Early**: Highlights bugs that might otherwise go unnoticed.

Strict mode encourages writing clean, secure, and maintainable code.