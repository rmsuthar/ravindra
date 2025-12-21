<style>
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Fira+Code:wght@400;500&display=swap');

  :root {
    --primary: #0066cc;
    --primary-light: #e6f0ff;
    --text-main: #1e293b;
    --card-bg: #ffffff;
    --border-color: #cbd5e1;
    --shadow-sharp: 4px 4px 0px rgba(0, 0, 0, 0.1);
  }

  body {
    background-color: #f1f5f9;
    color: var(--text-main);
    font-family: 'Outfit', sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
  }

  .markdown-body {
    max-width: 850px !important;
    margin: 0 auto !important;
    padding: 60px 20px !important;
    background: transparent !important;
  }

  h1 {
    font-size: 2.8rem;
    font-weight: 800;
    text-align: center;
    margin-bottom: 3.5rem !important;
    color: #0f172a;
    text-transform: uppercase;
    letter-spacing: -0.01em;
  }

  /* Sharp Box Design */
  details {
    background: var(--card-bg);
    border: 2px solid #000; /* Sharp black border for a bold look */
    border-radius: 0 !important; /* Forces sharp corners */
    margin-bottom: 2rem;
    transition: all 0.2s ease;
    box-shadow: var(--shadow-sharp);
  }

  details[open] {
    box-shadow: 8px 8px 0px rgba(0, 0, 0, 0.15);
    transform: translate(-2px, -2px);
  }

  summary {
    padding: 1.5rem 2rem;
    font-weight: 700;
    font-size: 1.3rem;
    cursor: pointer;
    list-style: none;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #fff;
    outline: none;
    border-bottom: 2px solid transparent;
  }

  details[open] summary {
    border-bottom: 2px solid #000;
    background: var(--primary-light);
  }

  summary::-webkit-details-marker {
    display: none;
  }

  summary::after {
    content: 'expand';
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    padding: 4px 8px;
    border: 1px solid #000;
    font-weight: 800;
  }

  details[open] summary::after {
    content: 'close';
    background: #000;
    color: #fff;
  }

  /* Ensure Markdown renders correctly */
  .inner-content {
    display: block;
    padding: 2rem;
    background: #fff;
  }

  /* Styling Markdown elements inside the box */
  .inner-content h2 { 
    margin-top: 0; 
    border-bottom: 2px solid var(--primary-light);
    color: var(--primary);
    padding-bottom: 0.5rem;
  }
  
  .inner-content h3 { color: #0f172a; margin-top: 1.5rem; }

  .topic-tag {
    font-size: 0.65rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    background: #000;
    color: #fff;
    padding: 2px 8px;
    font-weight: 700;
    margin-bottom: 0.5rem;
    display: inline-block;
  }

  .topic-title {
    display: block;
    margin-top: 4px;
  }

  pre {
    background: #1a1a1a !important;
    border-radius: 0 !important;
    padding: 1.5rem !important;
    border-left: 4px solid var(--primary) !important;
  }

  code {
    font-family: 'Fira Code', monospace !important;
    font-size: 0.9rem !important;
  }

  .back-home {
    display: inline-flex;
    align-items: center;
    color: #000;
    text-decoration: none;
    font-weight: 800;
    text-transform: uppercase;
    font-size: 0.8rem;
    border: 2px solid #000;
    padding: 6px 12px;
    margin-bottom: 2rem;
    background: #fff;
    box-shadow: 3px 3px 0px #000;
  }

  .back-home:hover {
    transform: translate(-1px, -1px);
    box-shadow: 4px 4px 0px #000;
  }
</style>

[Home](index.md){: .back-home}

# Technical Logs

<details>
<summary>
<div>
<span class="topic-tag">Architecture</span>
<span class="topic-title">Execute Events from External JSON</span>
</div>
</summary>
<div class="inner-content">

## Dynamic Script Execution
By using a combination of `fetch` and the `Function` constructor, we can externalize application logic into JSON files. This is particularly useful for feature flags or dynamic layout updates.

### JSON Structure
```json
{
 "functions": [
   { "name": "trackUser", "handler": "console.log('User tracked');" }
 ]
}
```

### JS Implementation
```javascript
const hydrate = async () => {
  const data = await fetch('/config.json').then(r => r.json());
  data.functions.forEach(f => {
    window[f.name] = new Function(f.handler);
  });
};
```
</div>
</details>

<details>
<summary>
<div>
<span class="topic-tag">DOM Utility</span>
<span class="topic-title">Advanced Visibility Detection</span>
</div>
</summary>
<div class="inner-content">

## Real Visibility Check
Standard `isVisible` checks often fail to account for opacity or viewport positioning. 

### The Solution:
The `getBoundingClientRect` API, combined with `getComputedStyle`, provides a deterministic way to verify if an element is truly reachable by the user.

```javascript
function isVisible(selector) {
  const el = document.querySelector(selector);
  const rect = el.getBoundingClientRect();
  const style = window.getComputedStyle(el);
  
  return rect.width > 0 && style.opacity > 0;
}
```
</div>
</details>

<details>
<summary>
<div>
<span class="topic-tag">Algorithms</span>
<span class="topic-title">Array Pattern Matching</span>
</div>
</summary>
<div class="inner-content">

## Wildcard Logic
Implementing a pattern matching system allows for flexible data filtering without complex regex management.

*   **Fast**: Uses pre-cached regex templates.
*   **Flexible**: Supports multi-symbol wildcarding.

```javascript
const matches = (pattern, str) => {
  const regex = new RegExp(`^${pattern.replace('*', '.*')}$`);
  return regex.test(str);
};
```
</div>
</details>

<details>
<summary>
<div>
<span class="topic-tag">Internationalization</span>
<span class="topic-title">Localizing Dates with Vanilla JS</span>
</div>
</summary>
<div class="inner-content">

## Zero-Dependency I18n
Modern browsers have built-in support for locale-aware strings. There's almost never a need to ship a 50kb library just for month names.

```javascript
const getFrenchMonths = () => {
  const date = new Date(2024, 0, 1);
  return Array.from({length: 12}, (_, i) => {
    date.setMonth(i);
    return date.toLocaleString('fr-FR', { month: 'long' });
  });
};
```
</div>
</details>

<details>
<summary>
<div>
<span class="topic-tag">Security</span>
<span class="topic-title">Security & Governance</span>
</div>
</summary>
<div class="inner-content">

## Hardening the Frontend
Security is a continuous cycle of assessment and implementation.

1.  **CSP**: Define strict content origins.
2.  **Integrity**: Use Subresource Integrity (SRI) for external scripts.
3.  **Hiding State**: Protected DOM attributes to prevent unauthorized tampering.

</div>
</details>
