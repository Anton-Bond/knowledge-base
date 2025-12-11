### 1. **What is React and why is it needed? What are the pros and cons?**

React is a JavaScript library developed by Facebook for building user interfaces, primarily for single-page applications. It allows developers to build reusable UI components and efficiently update and render components based on data changes.

**Pros:**

- **Component-based architecture:** Encourages code reuse and modularity.
- **Virtual DOM:** Enhances performance by minimizing direct DOM manipulation.
- **Unidirectional data flow:** Makes it easier to debug and reason about the application state.
- **Rich ecosystem:** Tools like React Router, Redux, and a strong community support.

**Cons:**

- **Steep learning curve:** Requires understanding JSX, hooks, state management, etc.
- **Frequent updates:** Rapid evolution can sometimes make older codebases harder to maintain.
- **Overhead for simple projects:** React might be overkill for small applications.

---

### 2. **What is virtual DOM and why is it needed?**

The Virtual DOM is an in-memory representation of the actual DOM. React uses it to determine the most efficient way to update the user interface.

**Why needed:**

- Direct DOM manipulation is costly in terms of performance.
- The Virtual DOM optimizes updates by batching and calculating the minimal set of changes required before updating the actual DOM.

---

### 3. **What is JSX and why is it needed? How is JSX code read by the browser?**

JSX (JavaScript XML) is a syntax extension for JavaScript that resembles HTML. It allows developers to write UI components declaratively.

**Why needed:**

- Improves readability and maintainability.
- Allows combining UI layout and logic.

**How JSX works:** Browsers don’t understand JSX. It must be transpiled into plain JavaScript using tools like Babel. For example, `<h1>Hello</h1>` is transpiled to `React.createElement('h1', null, 'Hello')`.

---

### 4. **What is reconciliation?**

Reconciliation is the process by which React updates the DOM efficiently. When state or props change, React:

1. Generates a new Virtual DOM tree.
2. Diffs it with the previous tree.
3. Applies the minimal set of changes to the actual DOM.

---

### 5. **What is batching in React?**

Batching refers to React’s mechanism of grouping multiple state updates into a single render to optimize performance. Starting with React 18, batching occurs across asynchronous boundaries like `setTimeout`.

---

### 6. **Component lifecycle? How to interact with the lifecycle using hooks?**

React components have distinct phases: **mounting**, **updating**, and **unmounting**.

**Class Components Lifecycle Methods:**

- Mounting: `constructor`, `componentDidMount`
- Updating: `componentDidUpdate`
- Unmounting: `componentWillUnmount`

**Hooks for Functional Components:**

- `useEffect`: For side effects. Replace `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` with clean-up logic in the return function.

---

### 7. **Portals?**

Portals allow rendering children into a DOM node outside the parent component’s DOM hierarchy. Useful for modals, tooltips, or overlays. Use `ReactDOM.createPortal`.

---

### 8. **What is JSX/TSX?**

- **JSX**: Syntax extension for JavaScript.
- **TSX**: TypeScript’s version of JSX, allowing static type checking.

---

### 9. **Virtual DOM & Reconciliation process?**

See answers 2 and 4.

---

### 10. **Common pitfalls of using useEffect?**

- **Infinite loops:** Caused by missing dependencies or incorrect dependency arrays.
- **Unnecessary executions:** Effects running on every render due to missing dependency optimizations.
- **Memory leaks:** Forgetting clean-up logic for subscriptions or timers.

---

### 11. **How to optimize performance?**

- **React.memo:** Prevents unnecessary renders for functional components.
- **useCallback:** Memoizes callback functions.
- **useMemo:** Memoizes expensive computations.
- **Key prop in lists:** Helps React identify items uniquely for efficient diffing.
- **Lazy loading:** Reduces initial load time.
- **State management:** Use tools like Redux or Zustand to minimize prop drilling.

---

### 12. **SWR and React Query?**

- **SWR (Stale-While-Revalidate):** Lightweight library for data fetching and caching.
- **React Query:** More feature-rich, providing caching, query invalidation, and optimistic updates.

---

### 13. **Zustand vs. Redux?**

- **Zustand:** Minimalistic state management, ideal for simpler applications.
- **Redux:** More powerful, with strict patterns for managing state.

---

### 14. **React and React-DOM: Why two packages?**

- **React:** Core library for building components.
- **React-DOM:** Handles rendering to the browser’s DOM. React Native uses a different renderer for mobile apps.

---

### 15. **What is reconciliation?**

See answer 4.

---

### 16. **Key in .map()?**

Keys identify elements uniquely. They improve reconciliation by letting React track changes efficiently. Keys should be unique and stable.

---

### 17. **SyntheticEvent?**

React’s cross-browser wrapper for browser events, providing consistent behavior across different browsers.

---

### 18. **What is a component in general?**

A reusable, independent piece of UI that can accept inputs (props) and manage its own state.

---

### 19. **Memoization?**

Caching expensive computations or renders. Achieved in React using `React.memo`, `useMemo`, or `useCallback`.

---

### 20. **Class components and lifecycle methods?**

See answer 6.

---

### 21. **Hooks and function components as a concept?**

Hooks like `useState` and `useEffect` enable state and lifecycle functionality in functional components.

---

### 22. **Unidirectional data flow, flux?**

React data flows in one direction: from parent to child. Flux patterns reinforce this with centralized state management.

---

### 23. **What is Suspense?**

Suspense allows React to wait for asynchronous operations like lazy-loaded components or data fetching before rendering.

---

### 24. **What is RSC (SSR/CSR + SPA/MPA)?**

- **RSC:** React Server Components.
- **SSR:** Server-Side Rendering.
- **CSR:** Client-Side Rendering.
- **SPA/MPA:** Single/Multi-Page Applications.

---

### 25. **What is Fragment?**

A lightweight wrapper (`<React.Fragment>`) that doesn’t render additional nodes to the DOM.

---

### 26. **What is forwardRef?**

A function that passes refs through components to DOM elements or other components.

---

### 27. **What is lazy?**

`React.lazy` enables dynamic imports for code-splitting and lazy loading.

---

### 28. **What is hydration?**

Hydration reuses static HTML rendered by the server and attaches React event listeners to make it interactive on the client side.

---

### **Hooks**

#### 1. **Main hooks: useState, useEffect (what are the problems?), useRef, useContext, useLayoutEffect, useReducer**

- **useState**: Manages local state in functional components.
    
    - **Problem:** Triggers re-renders every time the state changes, potentially leading to performance issues in larger applications.
- **useEffect**: Performs side effects like fetching data or subscribing to events.
    
    - **Problem:** It can run too frequently if not carefully controlled with dependency arrays, leading to unnecessary re-renders and performance bottlenecks.
- **useRef**: Holds mutable references to DOM elements or values that persist across renders without causing re-renders.
    
    - **Problem:** Cannot trigger re-renders when the `current` value changes.
- **useContext**: Provides a way to share values (like state or themes) across the component tree without prop drilling.
    
    - **Problem:** Can cause unnecessary re-renders of consuming components when context value changes.
- **useLayoutEffect**: Similar to `useEffect`, but runs synchronously after DOM mutations, preventing any flickering.
    
    - **Problem:** Can cause performance issues due to its synchronous nature if overused or if it causes layout thrashing.
- **useReducer**: Alternative to `useState`, especially when managing more complex state logic (like multiple values or nested state).
    
    - **Problem:** More boilerplate compared to `useState` and requires careful management of actions and state transitions.

---

#### 2. **useId**

`useId` generates a stable unique ID on the client side, useful for accessibility or any component requiring a unique identifier. It’s particularly helpful in scenarios like form inputs with labels.

---

#### 3. **useDeferredValue (defer rendering, + as debounce)**

`useDeferredValue` helps to defer the rendering of non-urgent updates to avoid blocking the main UI thread. It can be considered a form of implicit debounce, delaying rendering of less critical state updates.

- **Use case:** It’s ideal when there are expensive operations, such as typing in a search bar, where immediate rendering isn’t necessary and can be delayed.

---

#### 4. **useTransition (mark rendering as non-blocking)**

`useTransition` allows developers to mark certain updates as non-blocking, enabling React to prioritize more urgent updates, such as user input, over less critical updates, like displaying a list of results.

- **Use case:** Useful in scenarios where some UI updates can be delayed to ensure smoother user interactions, such as loading lists or transitions.

---

#### 5. **useImperativeHandle**

`useImperativeHandle` is used with `forwardRef` to control the instance value exposed to the parent when using refs. It allows you to customize what is exposed when the parent accesses the child’s ref.

- **Use case:** If you need to expose a custom API from the child component (e.g., a method that manipulates the DOM directly).

---

### **Performance**

#### 1. **lazy**

`React.lazy` is used to lazily load a component, splitting the code and only loading the component when it is needed. This reduces the initial loading time of your app.

- **Use case:** When you have large components or routes that are not immediately needed, lazy loading can improve the app’s initial load time.

---

#### 2. **Code splitting, chunks**

Code splitting involves breaking the application code into smaller chunks that are loaded as needed rather than all at once.

- **Use case:** For large applications, splitting code by route or by component allows users to load only the necessary code for the initial view, improving performance.

---

#### 3. **useMemo/useCallback/memo, composition**

- **useMemo**: Memoizes a value or result of a computation to prevent recalculation on every render.
- **useCallback**: Memoizes a function, ensuring that it does not get re-created unless its dependencies change.
- **React.memo**: A higher-order component that prevents unnecessary re-renders of functional components.

**Composition:** Using these hooks in combination can help optimize performance by preventing unnecessary re-renders and recomputations, especially for large components or complex UIs.

---

#### 4. **Caching**

Caching is used to store results of expensive computations or data fetches and reuse them to avoid recomputing or refetching data.

- **Use case:** Libraries like **React Query** or **SWR** implement caching strategies that automatically cache API results and serve them from memory until the data becomes stale.

---

#### 5. **Virtualization**

Virtualization involves rendering only the items visible within the viewport, rather than all items in a large list or grid. This significantly reduces the number of DOM nodes and improves performance, especially with long lists.

- **Libraries:** `react-window` and `react-virtualized` are popular libraries that implement list virtualization.

---

#### 6. **Algorithms**

Optimizing performance often involves using more efficient algorithms. For example:

- **Debouncing and throttling:** For handling frequent events (like typing or scrolling), reducing the frequency of updates.
- **Efficient data structures:** Use of algorithms with time complexity improvements (e.g., O(n) instead of O(n^2) for sorting or searching).
- **Memoization of expensive computations**: Helps avoid recalculating results unnecessarily.