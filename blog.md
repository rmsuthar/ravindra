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
