### **1. Provide examples to explain SOLID principles.**

1. **S (Single Responsibility Principle)**: Each module or class should have one responsibility.
    - **Example**: A `UserService` class handles user operations (e.g., CRUD), while `EmailService` sends emails, avoiding overlap.
2. **O (Open/Closed Principle)**: Classes should be open for extension but closed for modification.
    - **Example**: Using polymorphism in a `PaymentProcessor` to add new payment methods without altering the existing logic.
3. **L (Liskov Substitution Principle)**: Derived classes should be substitutable for their base class.
    - **Example**: A `Rectangle` class shouldn't break behavior when substituted with a `Square` subclass.
4. **I (Interface Segregation Principle)**: Clients shouldn't be forced to implement unused methods.
    - **Example**: Split a large interface like `DocumentProcessor` into smaller ones like `Printable`, `Scannable`.
5. **D (Dependency Inversion Principle)**: High-level modules shouldn't depend on low-level modules; both should depend on abstractions.
    - **Example**: Use dependency injection to inject an `ILogger` into a `UserService`.

---

### **2. SPA / MPA? How would you choose?**

- **SPA (Single Page Application)**: A single HTML page dynamically updates based on user interaction.
    - **When to choose**: High interactivity, seamless user experience, minimal page reloads, e.g., Gmail, Trello.
- **MPA (Multi-Page Application)**: Each page is a separate HTML file.
    - **When to choose**: SEO-heavy, large-scale applications requiring static pages, e.g., e-commerce sites.

---

### **3. CSR / SSR? How would you choose?**

- **CSR (Client-Side Rendering)**: Rendering happens in the browser.
    - **When to choose**: SPA with minimal SEO requirements, rich interactivity, e.g., dashboards.
- **SSR (Server-Side Rendering)**: Rendering happens on the server, delivering pre-rendered HTML.
    - **When to choose**: SEO-focused apps, fast initial load times, e.g., blogs, e-commerce.

---

### **4. gzip, Brotli?**

- Both are compression algorithms to reduce file sizes for faster web delivery.
- **Brotli**: Generally provides better compression ratios than gzip but may take slightly longer to compress.
- **Use Case**: Use Brotli if the server/browser supports it; fallback to gzip otherwise.

---

### **5. What is CORS? How to fix CORS problems?**

- **CORS (Cross-Origin Resource Sharing)**: A mechanism allowing restricted resources on a web page to be requested from a different domain.
- **Fixing CORS**:
    - Set correct HTTP headers (`Access-Control-Allow-Origin`, `Methods`, `Headers`).
    - Proxy requests via the same-origin server.

---

### **6. What is an options preflight request?**

- **Preflight request**: A `OPTIONS` request sent by browsers before the actual request to check server CORS permissions.
- **Purpose**: Ensure the server accepts the actual request with the specified headers/methods.

---

### **7. What is XSS? How to deal with it?**

- **XSS (Cross-Site Scripting)**: Injecting malicious scripts into web pages viewed by others.
- **Prevention**:
    - Sanitize inputs and outputs.
    - Use CSP (Content Security Policy).
    - Employ Trusted Types API.
    - Avoid `innerHTML` and dynamic script injections.

---

### **8. Front-end optimization methods:**

1. Algorithmic: Optimize loops, reduce complexity.
2. Caching: Cache static assets, API results.
3. Pagination: Reduce data on a single view.
4. Infinite Scrolling: Efficient content rendering.
5. Virtualized Lists: Render only visible items.
6. Code Splitting: Load JavaScript in chunks.

---

### **9. SEO optimization (JS perspective):**

- Server-side rendering (SSR) or static generation for better crawlability.
- Web metrics:
    - **TTFP**: Time to First Paint.
    - **TTFI**: Time to First Interactive.
    - **LCP**: Largest Contentful Paint.
    - **CLS**: Cumulative Layout Shift.
- Use Lighthouse for performance audits.

---

### **10. What is O(n) notation?**

- Describes the time/space complexity of algorithms.
- **Example**: A for loop over an array has `O(n)` complexity.

---

### **11. What do you understand by architecture?**

- A system’s structure: organizing components, ensuring scalability, and maintainability.

---

### **12. Basic concepts of OOP:**

1. **Encapsulation**: Wrapping data and behavior in a single unit.
2. **Inheritance**: Deriving new classes from existing ones.
3. **Polymorphism**: Methods behave differently based on the object.
4. **Abstraction**: Hiding implementation details.

---

### **13-15. Class, Object, Method, Property, Static:**

- **Class**: Blueprint for objects.
- **Object**: Instance of a class.
- **Method**: Function in a class.
- **Property**: Variable in a class.
- **Static Methods/Properties**: Belong to the class, not instances.

---

### **16. DRY, KISS, YAGNI**

- **DRY**: Don’t repeat yourself.
- **KISS**: Keep it simple, stupid.
- **YAGNI**: You aren’t gonna need it.

---

### **17. npm & pnpm**

- **pnpm**: Efficient package manager using symlinks.
- **devDeps**: For development tools.
- **deps**: For runtime dependencies.

---

### **18. Cookie / Local Storage / Session Storage**

| **Feature**   | **Cookies**                              | **Local Storage**                         | **Session Storage**                       |
| ------------- | ---------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| **Lifetime**  | Can persist until a set expiration date. | Persists until manually cleared.          | Persists until the browser tab is closed. |
| **Capacity**  | ~4 KB per cookie.                        | ~5–10 MB per domain.                      | ~5–10 MB per domain.                      |
| **Scope**     | Sent with HTTP requests automatically.   | Accessible only via JavaScript.           | Accessible only via JavaScript.           |
| **Use Cases** | Authentication, user preferences.        | Persistent client-side data.              | Temporary client-side data.               |
| **Security**  | Vulnerable to XSS/CSRF if not secured.   | Less vulnerable (not sent to the server). | Less vulnerable (not sent to the server). |

---

### **19. Git: Stash, Cherry-Pick, Merge, Rebase**

- **Stash**: Temporarily save changes without committing.
    - **Use Case**: When switching branches and you have uncommitted work.
    - Command: `git stash` / `git stash apply`.
- **Cherry-Pick**: Apply a specific commit from one branch to another.
    - **Use Case**: Selectively integrate features or fixes.
    - Command: `git cherry-pick <commit-hash>`.
- **Merge**: Combine changes from one branch into another.
    - **Use Case**: Combine feature branches into `main`.
    - Command: `git merge <branch>`.
- **Rebase**: Reapply commits from one branch on top of another.
    - **Use Case**: Clean up commit history or synchronize with `main`.
    - Command: `git rebase <branch>`.
- **Naming Conventions**: Use clear branch names, e.g., `feature/add-auth`, `bugfix/login-error`.

---

### **20. Do You Understand Semver Libraries?**

- **Semantic Versioning (SemVer)**: Format: `MAJOR.MINOR.PATCH`.
    - **MAJOR**: Breaking changes.
    - **MINOR**: Backward-compatible features.
    - **PATCH**: Bug fixes.
- **Example**: `1.4.2` → `2.0.0` (breaking changes).

---

### **21. devDeps vs. deps**

- **devDependencies**: Tools for development (`@types/`, linters, bundlers).
    - Example: `webpack`, `eslint`.
- **dependencies**: Required in production.
    - Example: `express`, `react`.
- **Build Stage**: Place tools like `babel`, `webpack` in `devDeps`.

---

### **22. Choosing NPM Libraries**

- **Factors to Consider**:
    - Popularity: Downloads, stars.
    - Maintenance: Last update, issue resolution.
    - Size: Smaller bundles reduce app load times.
    - Security: Check for vulnerabilities.
- **Why Many Dependencies are Bad**:
    - Increases bundle size.
    - Dependency chain risks (e.g., broken transitive dependencies).

---

### **23. What Bundlers Do You Have Experience With? Vite?**

- **Experience**: Webpack, Rollup, Parcel, Vite.
- **Vite**:
    - Faster builds using native ES modules.
    - Optimized for modern frameworks like Vue/React.

---

### **24. i18n**

- Internationalization libraries (e.g., `i18next`, `react-intl`).
- Handle translation files (`.json`), date formats, currencies.
- **Example**: Show localized content based on browser locale.

---

### **25. Routing**

- Framework-based routing (e.g., React Router, Vue Router).
- **Client-Side Routing**: SPA-like navigation.
- **Server-Side Routing**: MPA-like navigation.

---

### **26. Frameworks**

- **React**: Component-based, fast updates.
- **Vue**: Reactive system, simple syntax.
- **Angular**: Enterprise-ready, opinionated.

---

### **27. SPA/MPA SSR/CSR/Hybrids**

- **SPA**: Seamless UX, CSR or SSR.
- **MPA**: SEO-focused, static pages.
- **Hybrid**: Mix SPA/MPA using frameworks like Next.js.

---

### **28. State Management**

- Libraries: Redux, MobX, Zustand, Recoil.
- Use `useState` or `useReducer` for small apps.

---

### **29. Caching Libraries**

- **Client-Side**: SWR, React Query.
- **Server-Side**: Redis, Memcached.

---

### **30. UI Libraries**

- **Component Frameworks**: Material-UI, Ant Design, Tailwind CSS.
- **Custom Libraries**: Storybook for isolated development.

---

### **31. Native?**

- **React Native**: Build cross-platform mobile apps using JavaScript.
- **Expo**: Simplifies React Native setups.

---

### **32. Assembly/Bundling**

- Tools: Webpack, Vite.
- Optimizations: Code splitting, tree shaking, minification.

---

### **33. Styling**

- **CSS-in-JS**: Styled-components, Emotion.
- **Preprocessors**: SASS, LESS.
- **Frameworks**: Tailwind, Bootstrap.

---

### **34. Web Vitals**

- **Key Metrics**:
    - **LCP**: Largest Contentful Paint (loading performance).
    - **CLS**: Cumulative Layout Shift (visual stability).

---

### **35. Lighthouse**

- Chrome tool to audit performance, SEO, PWA, accessibility.

---

### **36. SEO**

- Optimize SSR for crawlability.
- Use structured data (`JSON-LD`).

---

### **37. SemVer**

- Respect MAJOR.MINOR.PATCH.
- Avoid breaking changes in patches or minors.

---
