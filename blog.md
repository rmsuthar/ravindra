<style>
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap');

  :root {
    --primary: #0066cc;
    --primary-light: #00d4ff;
    --bg-main: #f8fafc;
    --text-main: #1e293b;
    --card-bg: #ffffff;
    --border-color: #e2e8f0;
    --shadow-sm: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
    --shadow-md: 0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -1px rgba(0,0,0,0.06);
    --shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05);
  }

  body {
    background-color: var(--bg-main);
    color: var(--text-main);
    font-family: 'Outfit', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
  }

  /* Container styling for the blog body */
  .markdown-body {
    max-width: 900px !important;
    margin: 0 auto !important;
    padding: 40px 20px !important;
    background: transparent !important;
  }

  h1 {
    font-size: 3rem;
    font-weight: 800;
    text-align: center;
    background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 3rem !important;
    letter-spacing: -0.02em;
  }

  /* Accordion styling */
  details {
    background: var(--card-bg);
    border: 1px solid var(--border-color);
    border-radius: 16px;
    margin-bottom: 1.5rem;
    overflow: hidden;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: var(--shadow-md);
  }

  details[open] {
    box-shadow: var(--shadow-lg);
    transform: translateY(-4px);
    border-color: var(--primary);
  }

  summary {
    padding: 1.5rem 2rem;
    font-weight: 600;
    font-size: 1.25rem;
    cursor: pointer;
    list-style: none;
    display: flex;
    align-items: center;
    justify-content: space-between;
    transition: background 0.3s;
    user-select: none;
  }

  summary:hover {
    background: #f1f5f9;
  }

  summary::-webkit-details-marker {
    display: none;
  }

  /* Custom indicator */
  summary::after {
    content: '';
    width: 24px;
    height: 24px;
    background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 24 24' stroke='%230066cc' stroke-width='2.5'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' d='dp12 4.5v15m7.5-7.5h-15' /%3E%3C/svg%3E") no-repeat center;
    transition: transform 0.4s ease;
    filter: drop-shadow(0 1px 2px rgba(0,102,204,0.2));
  }

  details[open] summary::after {
    transform: rotate(45deg);
    background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 24 24' stroke='%230066cc' stroke-width='2.5'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' d='M6 18L18 6M6 6l12 12' /%3E%3C/svg%3E") no-repeat center;
  }

  .blog-content {
    padding: 2rem;
    border-top: 1px solid var(--border-color);
    background: #fafcfe;
    animation: slideDown 0.4s ease-out;
  }

  .topic-tag {
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--primary);
    background: #e6f0ff;
    padding: 4px 10px;
    border-radius: 20px;
    font-weight: 700;
    margin-bottom: 0.75rem;
    display: inline-block;
  }

  .topic-title {
    display: block;
    color: var(--text-main);
  }

  /* Content typography */
  .blog-content h2, .blog-content h3 {
    margin-top: 0;
    color: var(--primary);
  }

  pre {
    background: #1e293b !important;
    border-radius: 12px !important;
    padding: 1.5rem !important;
    overflow-x: auto;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.3);
  }

  code {
    font-family: 'Fira Code', 'Cascadia Code', monospace;
    font-size: 0.9rem;
  }

  @keyframes slideDown {
    from { opacity: 0; transform: translateY(-20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* Back to home link */
  .back-home {
    display: inline-flex;
    align-items: center;
    color: var(--primary);
    text-decoration: none;
    font-weight: 600;
    margin-bottom: 2rem;
    transition: transform 0.2s;
  }

  .back-home:hover {
    transform: translateX(-5px);
  }

  .back-home::before {
    content: '←';
    margin-right: 8px;
  }

  @media (max-width: 640px) {
    h1 { font-size: 2.25rem; }
    summary { padding: 1.25rem; font-size: 1.1rem; }
  }
</style>

[Back to Profile](index.md){: .back-home}

# Engineering Blogs & Insights

<details>
  <summary>
    <div>
      <span class="topic-tag">Architecture</span>
      <span class="topic-title">Modern React: Beyond the Basics</span>
    </div>
  </summary>
  <div class="blog-content">

  ## Mastering Composition over Inheritance
  In modern React development, the key to scalable UI is composition. Instead of complex prop-drilling or rigid component hierarchies, leverage children and render props to create truly flexible systems.

  ### Key React Hooks Cheat Sheet
  
  *   **`useState`**: Manage local state.
  *   **`useEffect`**: Handle side effects (API calls, subscriptions).
  *   **`useContext`**: Global state without prop drilling.
  *   **`useMemo`**: Cache expensive calculations.
  *   **`useCallback`**: Memoize functions to prevent unnecessary re-renders.

  ```jsx
  const OptimizedComponent = memo(({ data, onLoad }) => {
    const processed = useMemo(() => expensiveLogic(data), [data]);
    const handleClick = useCallback(() => onLoad(processed), [processed, onLoad]);

    return <button onClick={handleClick}>Process Data</button>;
  });
  ```
  </div>
</details>

<details>
  <summary>
    <div>
      <span class="topic-tag">Security</span>
      <span class="topic-title">Security-First Frontend Development</span>
    </div>
  </summary>
  <div class="blog-content">

  ## Beyond OWASP Top 10
  Security in the frontend isn't just about sanitizing inputs. It's about defense in depth.

  ### 1. Content Security Policy (CSP)
  Implement a robust CSP to prevent XSS and data injection. 
  ```http
  Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com;
  ```

  ### 2. Secure Storage
  Avoid storing sensitive tokens in `localStorage`. Use **HttpOnly Cookies** to mitigate token theft via XSS.

  ### 3. DOM Integrity
  Protect your application from malicious browser extensions and DevTools tampering.
  </div>
</details>

<details>
  <summary>
    <div>
      <span class="topic-tag">Leadership</span>
      <span class="topic-title">Engineering Excellence & Mentorship</span>
    </div>
  </summary>
  <div class="blog-content">

  ## Cultivating a High-Performance Culture
  Technical leadership is less about writing code and more about enabling others to write *better* code.

  ### Core Principles:
  *   **Empathy-Driven Code Reviews**: Focus on the code, not the person. Ask questions rather than making demands.
  *   **Architecture Communities of Practice**: Create forums where developers can debate technical decisions and share innovations.
  *   **SDLC Optimization**: Measure what matters—Lead Time, Deployment Frequency, and Change Failure Rate (DORA metrics).

  </div>
</details>

<details>
  <summary>
    <div>
      <span class="topic-tag">Case Study</span>
      <span class="topic-title">Optimizing Core Web Vitals at Scale</span>
    </div>
  </summary>
  <div class="blog-content">

  ## Achieving a 50% Performance Boost
  Performance isn't a one-time task; it's a continuous engineering discipline.

  ### Our Approach:
  1.  **Image Optimization**: Moving to Next-Gen formats (WebP/AVIF) and dynamic resizing.
  2.  **Code Splitting**: Implementing aggressive route-based and component-based lazy loading.
  3.  **Critical CSS**: Extracting and inlining the styles needed for the first paint.
  4.  **Resource Prioritization**: Using `preconnect`, `preload`, and `fetchpriority` effectively.

  </div>
</details>
