<style>
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Fira+Code:wght@400;500&display=swap');

  :root {
    --bg-main: #f0f4f8;
    --text-main: #1a202c;
    --card-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
  }

  body {
    background-color: var(--bg-main);
    color: var(--text-main);
    font-family: 'Outfit', sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
  }

  .markdown-body {
    max-width: 900px !important;
    margin: 0 auto !important;
    padding: 60px 20px !important;
    background: transparent !important;
  }

  h1 {
    font-size: 3rem;
    font-weight: 800;
    text-align: center;
    margin-bottom: 4rem !important;
    background: linear-gradient(135deg, #2b6cb0 0%, #4299e1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: -0.02em;
  }

  /* Universal Card Styling */
  details {
    background: #ffffff;
    border-radius: 20px;
    margin-bottom: 2rem;
    overflow: hidden;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: var(--card-shadow);
    border: 1px solid rgba(255,255,255,0.3);
  }

  details[open] {
    transform: scale(1.02);
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2);
  }

  summary {
    padding: 1.75rem 2.25rem;
    font-weight: 700;
    font-size: 1.4rem;
    cursor: pointer;
    list-style: none;
    display: flex;
    align-items: center;
    justify-content: space-between;
    outline: none;
    user-select: none;
  }

  summary::-webkit-details-marker { display: none; }

  /* Color Variations */
  .card-blue { border-left: 8px solid #3182ce; }
  .card-purple { border-left: 8px solid #805ad5; }
  .card-green { border-left: 8px solid #38a169; }
  .card-orange { border-left: 8px solid #dd6b20; }
  .card-red { border-left: 8px solid #e53e3e; }
  .card-indigo { border-left: 8px solid #5a67d8; }

  /* Indicator */
  .indicator {
    width: 32px;
    height: 32px;
    background: #edf2f7;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s;
    font-size: 0.9rem;
    color: #4a5568;
  }

  details[open] .indicator {
    background: #2d3748;
    color: #fff;
    transform: rotate(180deg);
  }

  /* HTML Content Styling (Fixes Markdown Issue) */
  .content-wrapper {
    padding: 0 2.25rem 2.25rem 2.25rem;
    color: #4a5568;
    animation: slideDown 0.4s ease-out;
  }

  .content-wrapper h2 {
    color: var(--text-main);
    font-size: 1.8rem;
    margin-bottom: 1rem;
    margin-top: 0;
  }

  .content-wrapper h3 {
    color: #2d3748;
    font-size: 1.25rem;
    margin-bottom: 0.75rem;
  }

  .content-wrapper p { margin-bottom: 1.25rem; font-size: 1.1rem; }

  .content-wrapper ul {
    margin-bottom: 1.5rem;
    padding-left: 1.5rem;
  }

  .content-wrapper li { margin-bottom: 0.5rem; }

  pre {
    background: #1a202c !important;
    color: #e2e8f0;
    padding: 1.5rem;
    border-radius: 12px;
    overflow-x: auto;
    font-family: 'Fira Code', monospace;
    font-size: 0.95rem;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
    margin: 1.5rem 0;
  }

  code {
    background: #f1f5f9;
    color: #e53e3e;
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Fira Code', monospace;
    font-size: 0.9em;
  }

  pre code {
    background: transparent;
    color: inherit;
    padding: 0;
  }

  .tag {
    display: inline-block;
    padding: 4px 12px;
    border-radius: 99px;
    font-size: 0.75rem;
    font-weight: 800;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 0.5rem;
  }

  .tag-blue { background: #ebf8ff; color: #2b6cb0; }
  .tag-purple { background: #faf5ff; color: #6b46c1; }
  .tag-green { background: #f0fff4; color: #2f855a; }
  .tag-orange { background: #fffaf0; color: #c05621; }
  .tag-red { background: #fff5f5; color: #c53030; }
  .tag-indigo { background: #ebf4ff; color: #4c51bf; }

  @keyframes slideDown {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .back-link {
    display: inline-flex;
    align-items: center;
    padding: 10px 20px;
    background: #fff;
    border-radius: 12px;
    text-decoration: none;
    color: #2d3748;
    font-weight: 600;
    box-shadow: 0 4px 6px rgba(0,0,0,0.05);
    margin-bottom: 2rem;
    transition: all 0.2s;
  }

  .back-link:hover {
    transform: translateX(-5px);
    background: #2d3748;
    color: #fff;
  }

  @media (max-width: 640px) {
    summary { padding: 1.25rem 1.5rem; font-size: 1.1rem; }
    .content-wrapper { padding: 0 1.5rem 1.5rem 1.5rem; }
  }
</style>

<a href="index.md" class="back-link">← Return to Profile</a>

<h1>Engineering Masterclass</h1>

<details class="card-blue">
  <summary>
    <div>
      <span class="tag tag-blue">Architecture</span>
      <div class="title">Dynamic Script Loading via Remote JSON</div>
    </div>
    <div class="indicator">▼</div>
  </summary>
  <div class="content-wrapper">
    <h2>Externalizing Application Logic</h2>
    <p>Modern enterprise applications require high agility. By externalizing logic into JSON, teams can update event handlers and business rules without triggering a full CI/CD pipeline deployment.</p>
    
    <h3>Implementation Strategy</h3>
    <p>The core concept involves fetching a configuration object and using the JavaScript <code>Function</code> constructor to instantiate executable code securely.</p>
    
    <pre><code>// Example Remote Configuration
{
  "events": [
    { "name": "onSubmit", "code": "alert('Form Submitted!');" }
  ]
}

// Global Hydration Logic
async function hydrateActions() {
  const config = await fetch('/api/ui-config').then(r => r.json());
  config.events.forEach(e => {
    window[e.name] = new Function(e.code);
  });
}</code></pre>

    <ul>
      <li><strong>Benefit:</strong> Instant updates for feature flags and simple UI logic.</li>
      <li><strong>Caution:</strong> Ensure strict sanitization and use Content Security Policy (CSP) to mitigate XSS risks.</li>
    </ul>
  </div>
</details>

<details class="card-purple">
  <summary>
    <div>
      <span class="tag tag-purple">DOM Utility</span>
      <div class="title">Fail-Safe Visibility Detection</div>
    </div>
    <div class="indicator">▼</div>
  </summary>
  <div class="content-wrapper">
    <h2>Detecting User-Facing Elements</h2>
    <p>Simple <code>display: none</code> checks are insufficient for modern interactive UIs. Elements might be hidden through opacity, parent clip-paths, or simply being outside the current viewport.</p>

    <h3>The Deterministic Checker</h3>
    <p>Using <code>getBoundingClientRect()</code> provides precise coordinates relative to the viewport, while <code>getComputedStyle()</code> reveals opacity and visibility states.</p>

    <pre><code>function isElementVisible(selector) {
  const el = document.querySelector(selector);
  if (!el) return false;

  const rect = el.getBoundingClientRect();
  const style = window.getComputedStyle(el);

  return (
    rect.width > 0 && 
    rect.height > 0 &&
    style.visibility !== 'hidden' &&
    parseFloat(style.opacity) > 0 &&
    rect.top <= window.innerHeight &&
    rect.bottom >= 0
  );
}</code></pre>
  </div>
</details>

<details class="card-green">
  <summary>
    <div>
      <span class="tag tag-green">Algorithms</span>
      <div class="title">Wildcard Pattern Recognition</div>
    </div>
    <div class="indicator">▼</div>
  </summary>
  <div class="content-wrapper">
    <h2>Flexible Data Searching</h2>
    <p>Implementing a wildcard search mechanism allows users to query complex datasets using the familiar <code>*</code> operator, commonly used in Unix shells and version control systems.</p>

    <h3>Optimized Regex Translator</h3>
    <p>Converting a wildcard string into a Regular Expression ensures native-level performance across large arrays.</p>

    <pre><code>class WildcardSearch {
  constructor(list) { this.list = list; }

  search(pattern) {
    const escapedPattern = pattern.replace(/[.+^${}()|[\]\\]/g, '\\$&');
    const regex = new RegExp(`^${escapedPattern.replace(/\*/g, '.*')}$`, 'i');
    return this.list.filter(item => regex.test(item));
  }
}

// Usage
const dict = new WildcardSearch(["monday", "tuesday", "friday"]);
dict.search("*day"); // Returns all matches</code></pre>
  </div>
</details>

<details class="card-orange">
  <summary>
    <div>
      <span class="tag tag-orange">I18n</span>
      <div class="title">Locale-Aware Date Utilities</div>
    </div>
    <div class="indicator">▼</div>
  </summary>
  <div class="content-wrapper">
    <h2>Native Browser Internationalization</h2>
    <p>Stop shipping large date libraries for simple localized strings. Modern browsers provide the <code>Intl</code> object and <code>toLocaleDateString</code> which are extremely efficient.</p>

    <pre><code>function getLocalizedDays(locale = 'en-US') {
  const formatter = new Intl.DateTimeFormat(locale, { weekday: 'long' });
  return Array.from({ length: 7 }, (_, i) => {
    const date = new Date(2021, 0, 4 + i); // 2021-01-04 was a Monday
    return formatter.format(date);
  });
}</code></pre>
    <p>This approach reduces bundle size by avoiding dependencies like Moment.js or date-fns for basic localization tasks.</p>
  </div>
</details>

<details class="card-red">
  <summary>
    <div>
      <span class="tag tag-red">Security</span>
      <div class="title">Advanced Frontend Guarding</div>
    </div>
    <div class="indicator">▼</div>
  </summary>
  <div class="content-wrapper">
    <h2>Hardening Client-Side Integrity</h2>
    <p>Protecting the UI from tampering (via Malicious Extensions or DevTools) is critical for enterprise security and state consistency.</p>
    
    <h3>Key Strategies</h3>
    <ul>
      <li><strong>CSP Headers:</strong> Strictly control script origins.</li>
      <li><strong>Subresource Integrity (SRI):</strong> Verify that fetched assets haven't been modified.</li>
      <li><strong>DOM Attribute Guarding:</strong> Prevent unauthorized modifications to sensitive ID and data-attributes.</li>
    </ul>
  </div>
</details>

<details class="card-indigo">
  <summary>
    <div>
      <span class="tag tag-indigo">Leadership</span>
      <div class="title">Engineering Management Framework</div>
    </div>
  </summary>
  <div class="content-wrapper">
    <h2>Scaling Quality through Automation</h2>
    <p>Leadership in engineering is about creating high-trust environments where quality is built-in, not inspected at the end.</p>
    
    <h3>DORA Metrics Focus</h3>
    <ul>
      <li><strong>Deployment Frequency:</strong> Enabling multiple releases per day.</li>
      <li><strong>Lead Time for Changes:</strong> Reducing time from commit to production.</li>
      <li><strong>Change Failure Rate:</strong> Ensuring stability via automated testing.</li>
    </ul>
  </div>
</details>
