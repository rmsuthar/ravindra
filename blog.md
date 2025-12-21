<style>
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Fira+Code:wght@400;500&display=swap');

  :root {
    --primary: #0066cc;
    --text-main: #1e293b;
    --text-muted: #64748b;
    --border-color: #e2e8f0;
  }

  body {
    background-color: #ffffff;
    color: var(--text-main);
    font-family: 'Outfit', sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
  }

  .markdown-body {
    max-width: 800px !important;
    margin: 0 auto !important;
    padding: 60px 20px !important;
    background: transparent !important;
  }

  h1 {
    font-size: 2.5rem;
    font-weight: 800;
    margin-bottom: 3rem !important;
    color: var(--text-main);
    border-bottom: 2px solid var(--primary);
    display: inline-block;
    padding-bottom: 8px;
  }

  /* Plain Accordion Styling */
  details {
    margin-bottom: 0;
    border-bottom: 1px solid var(--border-color);
  }

  summary {
    padding: 1.5rem 0;
    font-weight: 600;
    font-size: 1.25rem;
    cursor: pointer;
    list-style: none;
    display: flex;
    align-items: center;
    justify-content: space-between;
    transition: color 0.2s;
    outline: none;
  }

  summary:hover {
    color: var(--primary);
  }

  summary::-webkit-details-marker {
    display: none;
  }

  summary::after {
    content: '↓';
    font-size: 1rem;
    color: var(--text-muted);
    transition: transform 0.3s ease;
  }

  details[open] summary {
    color: var(--primary);
  }

  details[open] summary::after {
    transform: rotate(180deg);
    color: var(--primary);
  }

  /* Content Styling */
  details > *:not(summary) {
    padding: 0.5rem 0 2rem 0;
  }

  details h2 { 
    font-size: 1.5rem;
    margin-top: 1rem;
    border-bottom: none;
    color: var(--text-main);
  }

  .topic-tag {
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    color: var(--primary);
    font-weight: 700;
    margin-right: 12px;
  }

  .topic-title {
    color: inherit;
  }

  pre {
    background: #f8fafc !important;
    border: 1px solid var(--border-color) !important;
    border-radius: 8px !important;
    padding: 1.5rem !important;
    margin: 1.5rem 0 !important;
  }

  code {
    font-family: 'Fira Code', monospace !important;
    font-size: 0.9rem !important;
    color: #ef4444; /* Standard red for inline code */
    background: #f1f5f9;
    padding: 2px 4px;
    border-radius: 4px;
  }

  pre code {
    color: #1e293b;
    background: transparent;
    padding: 0;
  }

  .back-home {
    display: inline-flex;
    align-items: center;
    color: var(--text-muted);
    text-decoration: none;
    font-size: 0.9rem;
    margin-bottom: 2rem;
    transition: color 0.2s;
  }

  .back-home:hover {
    color: var(--primary);
  }
</style>

[← Back to Profile](index.md){: .back-home}

# Engineering Insights

<details>
<summary>
  <div>
    <span class="topic-tag">Architecture</span>
    <span class="topic-title">Execute Events & Functions from External JSON</span>
  </div>
</summary>

## Remote Scripting without Re-deployments
Executing logic from a remote JSON file allows for dynamic application updates without the need for constant JavaScript code modifications or full releases.

### Example Configuration (JSON)
```json
{
 "events": [
   { "name": "onClick", "handler": "console.log('onClick event triggered');" }
 ],
 "functions": [
   { "name": "sayHello", "handler": "console.log('Hello!');" }
 ]
}
```

### Implementation Strategy
Use the `Function` constructor to hydrate these strings into executable code:
```javascript
fetch('config.json')
 .then(res => res.json())
 .then(data => {
   data.functions.forEach(func => {
     window[func.name] = new Function(func.handler);
     window[func.name]();
   });
 });
```
</details>

<details>
<summary>
  <div>
    <span class="topic-tag">DOM Utility</span>
    <span class="topic-title">Detecting Element Visibility in Depth</span>
  </div>
</summary>

## Visibility Beyond 'display: none'
Identifying if a selector is truly visible to the user requires checking parents, opacity, and viewport constraints.

```javascript
function isElementVisible(selector) {
  const el = document.querySelector(selector);
  if (!el) return false;

  const rect = el.getBoundingClientRect();
  const style = window.getComputedStyle(el);

  return (
    rect.width > 0 && 
    rect.height > 0 &&
    style.display !== 'none' &&
    style.visibility !== 'hidden' &&
    parseFloat(style.opacity) > 0 &&
    rect.top < window.innerHeight &&
    rect.bottom > 0
  );
}
```
</details>

<details>
<summary>
  <div>
    <span class="topic-tag">Algorithms</span>
    <span class="topic-title">Wildcard Search in Array Lists</span>
  </div>
</summary>

## Supporting '*' Pattern Matching
A custom dictionary class can help identify if a string matching a wildcard pattern exists within a dataset.

```javascript
class Dict {
 constructor(list) { this.dict = list; }
 
 inRegEx(word) {
   const pattern = word.replaceAll("*", "(.*)");
   return new RegExp(`^${pattern}$`, "g");
 }
 
 isInDict(word) {
   return this.dict.some(exp => this.inRegEx(word).test(exp));
 }
}

const ls = new Dict(["mon", "tue", "wed"]);
console.log(ls.isInDict("m*n")); // true
```
</details>

<details>
<summary>
  <div>
    <span class="topic-tag">I18n</span>
    <span class="topic-title">Localizing Weekdays & Months via JS</span>
  </div>
</summary>

## Native Localization using `toLocaleDateString`
Instead of large translation libraries, leverage the browser's built-in internationalization API to get localized names.

```javascript
function getWeekdays(lang) {
  const weekdays = [];
  const date = new Date(2024, 0, 1); // Start at a Monday
  for (let i = 0; i < 7; i++) {
    weekdays.push(date.toLocaleDateString(lang, { weekday: 'long' }));
    date.setDate(date.getDate() + 1);
  }
  return weekdays;
}
```
</details>

<details>
<summary>
  <div>
    <span class="topic-tag">Security</span>
    <span class="topic-title">Security-First Frontend Development</span>
  </div>
</summary>

## Defense in Depth
Security in the frontend isn't just about sanitizing inputs; it's about a holistic approach to protecting user data.

1. **Content Security Policy**: Mitigate XSS by strictly defining trusted sources.
2. **Secure Cookies**: Use `HttpOnly` and `SameSite` flags.
3. **DOM Shielding**: Prevent unauthorized tampering via browser extensions.
</details>

<details>
<summary>
  <div>
    <span class="topic-tag">Leadership</span>
    <span class="topic-title">Engineering Excellence & Mentorship</span>
  </div>
</summary>

## Scaling Excellence
Technical leadership is about building systems that empower teams to produce high-quality code consistently.

*   **DORA Metrics**: Track deployment frequency and lead time.
*   **Architecture Peer Reviews**: Decentralize decision-making.
*   **Knowledge Sharing**: Foster "Communities of Practice."
</details>
