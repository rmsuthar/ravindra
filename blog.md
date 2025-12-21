<style>
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Fira+Code:wght@400;500&display=swap');

  :root {
    --bg-main: #fcfcfd;
    --text-main: #1a1a1b;
    --primary: #0066cc;
    --border: #e2e8f0;
  }

  body {
    background-color: var(--bg-main);
    color: var(--text-main);
    font-family: 'Outfit', sans-serif;
    line-height: 1.7;
  }

  .markdown-body {
    max-width: 800px !important;
    margin: 0 auto !important;
    padding: 60px 20px !important;
  }
  .markdown-body h1:first-child:not(.show-title) {
    display: none;
  }

  h1 {
    font-size: 3.5rem;
    font-weight: 800;
    text-align: left;
    margin-bottom: 3rem !important;
    letter-spacing: -0.04em;
    color: #111;
  }

  h2 {
    font-size: 2.25rem;
    font-weight: 700;
    margin-top: 4rem !important;
    margin-bottom: 1.5rem !important;
    border-bottom: 4px solid #f1f5f9;
    padding-bottom: 10px;
    color: #000;
  }

  h3 {
    font-size: 1.5rem;
    font-weight: 600;
    margin-top: 2rem !important;
    color: var(--primary);
  }

  p {
    font-size: 1.15rem;
    color: #374151;
    margin-bottom: 1.5rem;
  }

  hr {
    border: 0;
    height: 1px;
    background: linear-gradient(to right, transparent, var(--border), transparent);
    margin: 6rem 0;
    position: relative;
    overflow: visible;
  }

  hr::after {
    content: "✦";
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: var(--bg-main);
    padding: 0 1.5rem;
    color: var(--primary);
    font-size: 1.2rem;
    opacity: 0.5;
  }

  pre {
    background: #0f172a !important; /* Deep Navy - High Contrast Base */
    border-radius: 12px !important;
    padding: 1.5rem !important;
    margin: 2rem 0 !important;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
    overflow-x: auto;
    border: 1px solid #334155;
  }

  code {
    font-family: 'Fira Code', monospace !important;
    background: #f1f5f9;
    padding: 2px 6px;
    border-radius: 4px;
    color: #b91c1c; /* High contrast red for inline code */
    font-size: 0.9em;
  }

  /* ADA Compliant Code Text: Ensuring white/light-gray on dark background */
  pre code {
    background: transparent !important;
    color: #f8fafc !important; /* Off-white for maximum readability */
    padding: 0 !important;
    font-size: 0.95rem !important;
    line-height: 1.6;
    display: block;
    text-shadow: none;
  }

  /* Enhancing visibility for syntax if handled by various themes */
  pre span {
    color: inherit !important;
  }

  ul {
    padding-left: 1.5rem;
    margin-bottom: 2rem;
  }

  li {
    margin-bottom: 0.75rem;
    font-size: 1.1rem;
    color: #4b5563;
  }

  /* Back button styling */
  .back-link {
    display: inline-block;
    color: var(--primary);
    text-decoration: none;
    font-weight: 600;
    margin-bottom: 2rem;
    font-size: 1rem;
    transition: transform 0.2s;
  }

  .back-link:hover {
    transform: translateX(-5px);
  }
</style>

[← Back to Profile](index.md){: .back-link}

# Engineering Journal

A collection of technical insights, architectural patterns, and engineering excellence discovered across 17+ years of building enterprise scale UI.

---

## 01. Execute Events from External JSON
Modern enterprise applications require high agility. By externalizing logic into JSON, teams can update event handlers and business rules without triggering a full CI/CD pipeline deployment.

### Implementation Strategy
The core concept involves fetching a configuration object and using the JavaScript `Function` constructor to instantiate executable code.

```javascript
// Fetch and hydrate remote logic
const hydrateActions = async () => {
  const config = await fetch('/api/ui-config').then(r => r.json());
  
  config.events.forEach(event => {
    // Dynamically create the function from the JSON string
    window[event.name] = new Function(event.handler);
  });
};
```

---

## 02. Advanced Element Visibility Detection
Simple `display: none` checks are insufficient for modern interactive UIs. Elements might be hidden through opacity, parent clip-paths, or simply being outside the current viewport.

### The Problem
Traditional checks often fail because they don't account for:
*   `opacity: 0`
*   `visibility: hidden`
*   Viewport position (above or below scroll)

### The Deterministic Solution
Using `getBoundingClientRect()` provides absolute coordinates relative to the viewport.

```javascript
function isVisible(selector) {
  const el = document.querySelector(selector);
  if (!el) return false;

  const rect = el.getBoundingClientRect();
  const style = window.getComputedStyle(el);

  return (
    rect.width > 0 && 
    rect.height > 0 &&
    style.visibility !== 'hidden' &&
    parseFloat(style.opacity) > 0 &&
    rect.top < window.innerHeight &&
    rect.bottom > 0
  );
}
```

---

## 03. Wildcard Pattern Matching in Arrays
Implementing a pattern matching system allows for flexible data filtering (like `m*n`) without the overhead of manually managing complex RegEx objects.

### Usage
*   **Fast**: Uses pre-cached regex templates.
*   **Flexible**: Supports multi-symbol wildcarding.

```javascript
class PatternSearch {
  constructor(list) {
    this.list = list;
  }

  find(pattern) {
    const regex = new RegExp(`^${pattern.replace(/\*/g, '.*')}$`, 'i');
    return this.list.filter(item => regex.test(item));
  }
}

const db = new PatternSearch(["monday", "tuesday", "wednesday"]);
console.log(db.find("m*n*y")); // ["monday"]
```

---

## 04. Native Browser Localization (Zero-Dependency)
Modern browsers have built-in support for locale-aware strings through `Intl` and `toLocaleDateString`. Avoid shipping massive libraries for basic localization tasks.

### Localized Weekdays
```javascript
const getLocalizedDays = (locale = 'en-US') => {
  return Array.from({ length: 7 }, (_, i) => {
    return new Date(2024, 0, 1 + i).toLocaleDateString(locale, { 
      weekday: 'long' 
    });
  });
};

// Example: getLocalizedDays('fr-FR') 
// -> ["lundi", "mardi", "mercredi", ...]
```

---

## 05. Security & Engineering Governance
Engineering excellence is more than just code; it's about the security and resilience of the system as a whole.

### Core Focus Areas:
*   **Content Security Policy (CSP)**: Strictly defining trusted origins.
*   **Subresource Integrity (SRI)**: Verifying remote scripts.
*   **Reference Architecture**: Implementing "Poison Pill" and "Self-Healing" behaviors.

---

## 06. Architecture Guide for UI Engineers
A beginner-friendly breakdown of core architectural concepts, explained specifically for the modern frontend landscape with practical code examples.

### 1. Micro-frontends (MFE)
**Concept:** Splitting a giant monolith into independent "Lego" pieces.

*   **Example (Conceptual Module Federation):**

```javascript
// A host app "stitching" together two remote MFEs
import React, { Suspense } from 'react';

const PaymentsMFE = React.lazy(() => import('PaymentsApp/Widget'));
const SettingsMFE = React.lazy(() => import('SettingsApp/Page'));

const Dashboard = () => (
  <div>
    <Suspense fallback="Loading Payments...">
      <PaymentsMFE />
    </Suspense>
    <Suspense fallback="Loading Settings...">
      <SettingsMFE />
    </Suspense>
  </div>
);
```

### 2. SDK Development (npm / Yarn Workspaces)
**Concept:** Building a shared toolbox (Design System/API Library) in a single repository.

*   **Example (Yarn Workspaces Structure):**

```bash
/my-project
  /packages
    /ui-kit        # Shared Buttons, Inputs (SDK)
    /data-service  # Shared API logic (SDK)
    /partner-app   # Uses ui-kit & data-service
  package.json     # "workspaces": ["packages/*"]
```

### 3. System Design
**Concept:** The blueprint of how data moves through your app.

*   **Example (UI Search Flow):**

```javascript
// System Design: Handling frequent API calls (Debouncing)
const SearchBar = () => {
  const [query, setQuery] = useState('');
  
  useEffect(() => {
    // 1. Wait for user to stop typing (300ms) - "System Governance"
    const timer = setTimeout(() => {
      if (query) fetchResults(query);
    }, 3000);
    
    // 2. Cleanup: Cancel previous call if user types again
    return () => clearTimeout(timer);
  }, [query]);

  return <input onChange={(e) => setQuery(e.target.value)} />;
};
```

### 4. Reference Architecture (Resiliency)
**Concept:** Ensuring the app doesn't stay broken when something fails.

*   **Example (Self-Healing / Fallback):**

```javascript
// Error Boundary: "Self-healing" mechanism to keep UI alive
class ResilientComponent extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true }; // "Self-healing" state update
  }

  render() {
    if (this.state.hasError) {
      return <div>Oops! This section crashed, but the rest of the site is OK.</div>;
    }
    return this.props.children;
  }
}
```

### 5. Design Patterns (MVC & DI)
**Concept:** Proven templates to keep code clean and testable.

*   **Example (Dependency Injection):**

```javascript
// Dependency Injection: Passing the "tool" into the function
// Instead of hardcoding 'apiService', we inject it
const useUser = (apiService) => { 
  const [user, setUser] = useState();
  
  useEffect(() => {
    apiService.getUser().then(setUser);
  }, [apiService]);

  return user;
};

// During testing, we inject a "MockService" instead of a "RealService"
const testUser = useUser(MockService); 
```

---

## 07. Deep Dive: Architectural Excellence (Intermediate)
Moving beyond basic concepts into implementation strategies, trade-offs, and professional-grade patterns for senior-level engineering.

### 1. Micro-frontends in the Real World
While the "Lego" analogy works for beginners, intermediate engineers must solve the challenges of **Shared State** and **Dependency Contention**.

*   **Orchestration vs. Integration:** Decide between a "Shell" (Container) that dynamically loads bundles via Module Federation vs. "Build-time integration" where sub-apps are npm packages.

*   **Communication Patterns:** Avoid direct coupling. Use a **Pub-Sub (Publish-Subscribe)** pattern or `CustomEvents` through the `window` object to allow MFEs to talk without knowing each other exists.

*   **Scoped Styling:** Implement CSS-in-JS or CSS Modules with unique hash prefixes to ensure Team A's `.button` doesn't overwrite Team B's `.button`.

### 2. Scalable SDKs & Monorepo Governance
Building a library (SDK) that 50+ developers use requires strict governance.

*   **Tree-shaking & Bundle Hygiene:** Ensure your SDK is side-effect free. Use `ESM` (ES Modules) to allow bundlers like Webpack/Vite to remove unused code, keeping the consumer's app lean.

*   **Semantic Versioning (SemVer):** Master the **Major.Minor.Patch** flow. 
    *   *Major:* Breaking changes (removing a prop).
    *   *Minor:* New features (adding a style).
    *   *Patch:* Bug fixes.

*   **The "Core" Pattern:** Keep your SDK thin. The core logic should be vanilla JS/TS, with separate "adapters" for React, Angular, or Vue.

### 3. Frontend System Design: The Layered Approach
Senior architects design UIs in "Layers" to ensure the logic stays separate from the pixels.

*   **Domain Layer:** Where the pure business logic and math live. (Zero dependencies on React).

*   **Infrastructure/Data Layer:** Handles API calls, local caching (SWR/React Query), and data normalization.

*   **Presentation Layer:** The "Dumb" components that only care about padding, colors, and layout.

*   **Trade-off (SLA):** Designing for "99.9% Uptime" isn't just a backend metric. In the UI, this means handling "Partial Failure"—where the header loads even if the sidebar fails.

### 4. Advanced Resiliency: The Circuit Breaker
If an external API is slow or failing, your UI should stop trying to call it temporarily to prevent "Cascading Failures."

*   **Circuit Breaker Pattern:**

```javascript
// A simple state machine for API calls
const circuitBreaker = {
  state: 'CLOSED', // Standard operation
  failureCount: 0,
  
  async callAPI(apiFn) {
    if (this.state === 'OPEN') throw new Error('Circuit is open (Safe Mode)');
    
    try {
      return await apiFn();
    } catch (e) {
      if (++this.failureCount > 5) this.state = 'OPEN';
      throw e;
    }
  }
};
```

### 5. Clean Architecture in Frontend (DI & Hooks)
Intermediate engineering is about **Inversion of Control**.

*   **Provider Pattern:** Leverage React Context not just for "Theme", but for injecting **Adapters**.

*   **Example (Mocking for Speed):**

```javascript
// Injecting an API adapter through Context
const ApiProvider = ({ adapter, children }) => (
  <ApiContext.Provider value={adapter}>{children}</ApiContext.Provider>
);

// This allows you to test the entire app with a 
// "FakeMockAdapter" without changing a single line of component code.
```

---

## 08. Master Level: Strategic Architecture (Advanced)
Strategic patterns for creating future-proof, high-performance, and ultra-secure global enterprise systems.

### 1. The BFF Pattern (Backend for Frontend)
In large systems, the UI shouldn't talk to 20 different microservices. We use a **BFF** to orchestrate and aggregate data specifically for the UI's needs.

*   **GraphQL as an Orchestrator:** Instead of multiple REST calls, the UI requests exactly what it needs in one query.

*   **Performance Engineering:** The BFF handles complex data joining and filtering, reducing the computational load (and battery drain) on the user's device.

### 2. Deterministic UI with State Machines
As applications grow, "Boolean Hell" (e.g., `isLoading`, `isError`, `isSuccess`) leads to buggy, unpredictable UIs.

*   **Finite State Machines (FSM):** Use libraries like **XState** to define formal states (e.g., `Idle`, `Loading`, `Retrying`, `Success`, `Failure`) and the transitions between them.

*   **Impact:** This makes the UI mathematically predictable and eliminates "impossible states" (like showing a success message and a loading spinner at the same time).

### 3. Performance Engineering: Off-Main-Thread & WASM
For heavy computations (data visualization, encryption, or image processing), we move work off the main thread to keep the UI smooth (60 FPS).

*   **Web Workers:** Running heavy JS logic in a background thread so the main thread stays free to handle user clicks and animations.

*   **WebAssembly (WASM):** For ultra-high performance, we compile C++ or Rust code into a binary that runs in the browser at near-native speeds.

### 4. Agentic AI & Generative UI
The next frontier is interfaces that aren't static but are generated dynamically by AI based on user intent.

*   **RAG-driven UI:** Integrating Retrieval-Augmented Generation to surface context-aware widgets and data.

*   **Prompt Engineering as a Service:** Architects must design how LLM prompts are securely injected and managed to drive specific UI behaviors.

---

## 09. Learning Resources & References
To master these concepts, I highly recommend reviewing the following industry-leading resources:

*   **[Microfrontends.org](https://micro-frontends.org/)**: The definitive guide to building MFE architectures.

*   **[Patterns.dev](https://www.patterns.dev/)**: A comprehensive interactive resource for modern web design patterns and performance optimizations.

*   **[Martin Fowler: Micro-frontends](https://martinfowler.com/articles/micro-frontends.html)**: A deep architectural dive by the person who literally wrote the book on enterprise architecture.

*   **[Clean Architecture on Frontend](https://bespoyasov.me/blog/clean-architecture-on-frontend/)**: A practical guide on applying Uncle Bob's Clean Architecture principles to React and TypeScript.

*   **[MDN: Web Workers API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API)**: The foundation for high-performance, off-main-thread JavaScript.

*   **[OWASP Top 10 for Frontend](https://owasp.org/www-project-top-ten/)**: The absolute baseline for any architect building secure enterprise-grade UIs.

---
