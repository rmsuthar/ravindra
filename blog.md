<style>
    /* Styles for h1 and h2 */
h1 {
  font-size: 2.5rem;
  margin: 2rem 0;
  color: #333; /* dark grey */
}

.markdown-body h1:first-child{
   text-transform: capitalize;
}

/* Styles for code blocks */
h2 {
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 4px;
  line-height: 1.4;
  overflow-x: auto;
  padding: 0.5rem;
  white-space: pre-wrap;
  font-size: 2rem;
  margin: 1.5rem 0;
  color: #555; /* grey */
}

a:hover, a:focus {
    text-decoration: none;
    background-color: blue;
    color: white;
    border-radius: 3px;
    padding: 2px 3px;
}

</style>
# React Custom Hook

### Custom Hook Example: `useFetch`

A custom hook allows you to extract reusable logic from your components, promoting better code organization and reuse. Here, we’ll create a `useFetch` hook for fetching data from an API.

#### Purpose:
`useFetch` is a custom hook designed to handle the logic for making HTTP requests and managing states like `loading`, `error`, and `data`. This hook can be reused across multiple components for making API calls.

### Code for `useFetch` Custom Hook

```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Failed to fetch data');
        }
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}
```

### How to Use the `useFetch` Hook

```jsx
import React from 'react';
import useFetch from './useFetch';

function UserList() {
  const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {data.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export default UserList;
```

### Explanation:

1. **State Management**:
   - `data`: Stores the fetched data.
   - `loading`: Boolean indicating the loading state.
   - `error`: Stores any error message from the fetch request.

2. **Effect Hook (`useEffect`)**:
   - Handles the fetching logic when the component mounts or when the `url` changes.
   - It uses `async/await` for making the request and handles possible errors.

3. **Return Object**:
   - Returns an object containing `data`, `loading`, and `error`, which can be destructured and used in any component.

### Benefits of Custom Hook:

- **Reusability**: The logic for fetching data is centralized, so you can use `useFetch` across multiple components.
- **Separation of Concerns**: Your components focus on rendering logic, while the hook takes care of the data fetching logic.

# React hooks cheat sheet

### React Hooks Cheat Sheet

React hooks allow you to use state, side effects, refs, and more in functional components. Here’s a cheat sheet for all the essential React hooks:

---

### 1. **useState**
- **Purpose**: Manage local state in a functional component.
- **Syntax**:
  ```jsx
  const [state, setState] = useState(initialValue);
  ```
- **Example**:
  ```jsx
  const [count, setCount] = useState(0);
  ```
  - `count`: current state
  - `setCount`: function to update state

---

### 2. **useEffect**
- **Purpose**: Perform side effects in functional components (e.g., data fetching, timers).
- **Syntax**:
  ```jsx
  useEffect(() => {
    // effect logic
    return () => {
      // cleanup logic
    };
  }, [dependencies]);
  ```
- **Example**:
  ```jsx
  useEffect(() => {
    document.title = `Clicked ${count} times`;
  }, [count]); // Only run when `count` changes
  ```

---

### 3. **useContext**
- **Purpose**: Access data from a context without passing props.
- **Syntax**:
  ```jsx
  const value = useContext(Context);
  ```
- **Example**:
  ```jsx
  const theme = useContext(ThemeContext);
  ```
  - `theme` is the value provided by `ThemeContext`.

---

### 4. **useReducer**
- **Purpose**: Alternative to `useState` for complex state logic. Works similar to Redux reducers.
- **Syntax**:
  ```jsx
  const [state, dispatch] = useReducer(reducer, initialState);
  ```
- **Example**:
  ```jsx
  const [state, dispatch] = useReducer((state, action) => {
    switch (action.type) {
      case 'increment':
        return { count: state.count + 1 };
      case 'decrement':
        return { count: state.count - 1 };
      default:
        return state;
    }
  }, { count: 0 });
  ```

---

### 5. **useRef**
- **Purpose**: Create mutable object that persists across renders. Can also reference DOM elements.
- **Syntax**:
  ```jsx
  const ref = useRef(initialValue);
  ```
- **Example**:
  ```jsx
  const inputRef = useRef(null);
  inputRef.current.focus();
  ```

---

### 6. **useMemo**
- **Purpose**: Memoize expensive calculations, recomputes only when dependencies change.
- **Syntax**:
  ```jsx
  const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
  ```
- **Example**:
  ```jsx
  const memoizedValue = useMemo(() => expensiveCalculation(count), [count]);
  ```

---

### 7. **useCallback**
- **Purpose**: Memoize functions, prevents recreation of functions unless dependencies change.
- **Syntax**:
  ```jsx
  const memoizedCallback = useCallback(() => {
    // function logic
  }, [dependencies]);
  ```
- **Example**:
  ```jsx
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);
  ```

---

### 8. **useLayoutEffect**
- **Purpose**: Similar to `useEffect`, but fires synchronously after all DOM mutations (useful for layout measurements).
- **Syntax**:
  ```jsx
  useLayoutEffect(() => {
    // effect logic
  }, [dependencies]);
  ```
- **Example**:
  ```jsx
  useLayoutEffect(() => {
    console.log('DOM has been updated');
  });
  ```

---

### 9. **useImperativeHandle**
- **Purpose**: Customize instance value exposed to parent components using `ref`.
- **Syntax**:
  ```jsx
  useImperativeHandle(ref, () => ({
    customMethod() {
      // logic
    }
  }));
  ```
- **Example**:
  ```jsx
  useImperativeHandle(ref, () => ({
    focus() {
      inputRef.current.focus();
    }
  }));
  ```

---

### 10. **useDebugValue**
- **Purpose**: Display custom label in React DevTools for custom hooks.
- **Syntax**:
  ```jsx
  useDebugValue(value, formatter);
  ```
- **Example**:
  ```jsx
  useDebugValue(isOnline ? 'Online' : 'Offline');
  ```

---

### 11. **useTransition** (React 18+)
- **Purpose**: Defer updates to allow for a smooth UI experience.
- **Syntax**:
  ```jsx
  const [isPending, startTransition] = useTransition();
  ```
- **Example**:
  ```jsx
  startTransition(() => {
    setState(newState);
  });
  ```

---

### 12. **useDeferredValue** (React 18+)
- **Purpose**: Defers a value to prevent UI lag for lower-priority updates.
- **Syntax**:
  ```jsx
  const deferredValue = useDeferredValue(value);
  ```
- **Example**:
  ```jsx
  const deferredValue = useDeferredValue(inputValue);
  ```

---

### 13. **useId** (React 18+)
- **Purpose**: Generate stable IDs for components, useful for accessibility and forms.
- **Syntax**:
  ```jsx
  const id = useId();
  ```
- **Example**:
  ```jsx
  <label htmlFor={id}>Name</label>
  <input id={id} />
  ```

---

### 14. **useSyncExternalStore** (React 18+)
- **Purpose**: Subscribe to external stores (Redux, etc.) to ensure state is synchronized.
- **Syntax**:
  ```jsx
  const state = useSyncExternalStore(subscribe, getSnapshot);
  ```
- **Example**:
  ```jsx
  const state = useSyncExternalStore(store.subscribe, store.getState);
  ```

---

### 15. **useInsertionEffect** (React 18+)
- **Purpose**: Useful for injecting styles before the DOM updates.
- **Syntax**:
  ```jsx
  useInsertionEffect(() => {
    // injection logic
  }, [dependencies]);
  ```

---

### Summary of React Hooks:
- **State Management**: `useState`, `useReducer`
- **Side Effects**: `useEffect`, `useLayoutEffect`
- **Context**: `useContext`
- **Refs**: `useRef`, `useImperativeHandle`
- **Memoization**: `useMemo`, `useCallback`
- **Advanced Features**: `useTransition`, `useDeferredValue`, `useSyncExternalStore`, `useId`, `useInsertionEffect`, `useDebugValue`

---

This cheat sheet covers all the hooks you need for effective React development. Whether managing state, side effects, or optimizing performance, React hooks provide a powerful way to enhance functional components.

# In Adobe Experience Cloud suits works.

Here's an overview of how each Adobe Experience Cloud application works, their connections, and a brief description of each. This can be formatted as a Word or PDF document as needed. Here's the content structure:

---

# Adobe Experience Cloud Applications Overview

## 1. **Adobe Analytics**
   **Brief:**  
   Adobe Analytics is a data collection and analysis platform that tracks user behavior across digital platforms (websites, apps, etc.). It provides businesses with insights into customer journeys, conversions, and audience interactions, helping optimize digital marketing efforts.  

   **How It Works:**  
   Adobe Analytics collects and analyzes large sets of data in real time, using tracking codes placed on digital properties. It allows marketers to segment audiences, create detailed reports, and gain actionable insights.

   **Connection with Other Applications:**  
   - **Adobe Audience Manager:** Shares audience segments for targeted analysis.
   - **Adobe Target:** Uses analytics data to personalize user experiences and measure campaign success.
   - **Adobe Launch:** Collects data through tags and integrates directly with Adobe Analytics.

---

## 2. **Adobe Target**
   **Brief:**  
   Adobe Target is a personalization and A/B testing tool that allows businesses to deliver tailored experiences to users. It helps test different variations of content, images, or layouts to optimize user engagement.

   **How It Works:**  
   Adobe Target enables marketers to set up A/B, multivariate, and automated personalization tests. It analyzes user behavior and preferences in real-time, helping businesses personalize user journeys.

   **Connection with Other Applications:**  
   - **Adobe Analytics:** Pulls data from Adobe Analytics to measure the performance of personalized experiences and make data-driven decisions.
   - **Adobe Audience Manager:** Uses audience segments to tailor personalized content.
   - **Adobe Experience Cloud ID Service:** Tracks and identifies users across platforms to ensure the continuity of personalized experiences.

---

## 3. **Adobe Audience Manager**
   **Brief:**  
   Adobe Audience Manager is a data management platform (DMP) that allows businesses to collect, segment, and activate audience data from multiple sources. It helps build high-value audience profiles for targeted marketing.

   **How It Works:**  
   Audience Manager aggregates data from multiple channels, including online, offline, and third-party data. It enables businesses to create dynamic audience segments for personalized marketing campaigns.

   **Connection with Other Applications:**  
   - **Adobe Analytics:** Provides detailed insights on audience segments.
   - **Adobe Target:** Sends audience segments for personalized content delivery.
   - **Adobe Experience Cloud ID Service:** Unifies customer identities across devices and platforms for consistent audience targeting.

---

## 4. **Adobe Experience Cloud ID Service (ECID)**
   **Brief:**  
   The Experience Cloud ID Service (ECID) is a core service that provides a unified ID for identifying and tracking users across various Adobe Experience Cloud applications. It ensures that user data is synchronized across platforms.

   **How It Works:**  
   ECID assigns a unique identifier to each user that interacts with your digital properties. This identifier is used across Adobe Experience Cloud products, ensuring consistency and accuracy in user tracking and personalization.

   **Connection with Other Applications:**  
   - **Adobe Analytics, Target, Audience Manager, and Launch:** All leverage ECID to track users across platforms and maintain consistency in data collection and personalization.
   - **Adobe Experience Platform:** Provides a seamless flow of user data between platforms for deeper analysis and targeting.

---

## 5. **Adobe Experience Platform (AEP)**
   **Brief:**  
   Adobe Experience Platform is an open and extensible platform that centralizes and activates customer data in real time. It enables businesses to deliver personalized customer experiences by collecting, organizing, and synthesizing customer data across channels.

   **How It Works:**  
   AEP unifies all customer data into a single profile, enabling businesses to make data-driven decisions. It integrates with machine learning and AI tools to predict customer behavior and personalize experiences at scale.

   **Connection with Other Applications:**  
   - **Adobe Analytics, Audience Manager, and Target:** AEP consolidates data from these applications to create unified profiles and power personalized experiences.
   - **Adobe Launch:** Uses AEP’s data to trigger actions and deploy personalized content.

---

## 6. **Adobe Launch (Adobe Experience Platform Launch)**
   **Brief:**  
   Adobe Launch is a tag management system that allows you to manage and deploy marketing and analytics tags on your websites and apps. It helps ensure efficient and error-free data collection across platforms.

   **How It Works:**  
   Launch acts as a control center for managing various third-party tags and Adobe Experience Cloud products. It allows marketers to deploy tags without code changes and helps manage integrations across platforms.

   **Connection with Other Applications:**  
   - **Adobe Analytics and Target:** Tags deployed using Launch collect data for analysis and personalization.
   - **Adobe Experience Cloud ID Service:** Leverages ECID to ensure that data collected by tags is linked to a single user profile.
   - **Adobe Experience Platform:** Launch sends data to AEP, enabling businesses to use real-time data in personalizing customer journeys.

---

# How These Applications are Internally Connected

The applications within Adobe Experience Cloud work together to create a cohesive ecosystem for digital marketing and customer experience management. Here's how:

1. **Data Flow and User Tracking:**
   - **Adobe Launch** collects data via tags and sends it to **Adobe Analytics** for reporting.
   - **Adobe Experience Cloud ID Service** ensures all user data collected is consistent and unified across platforms.
   
2. **Audience Segmentation and Personalization:**
   - **Adobe Audience Manager** segments audiences and shares these segments with **Adobe Target** for personalized experiences.
   - **Adobe Analytics** data is used by **Adobe Target** to optimize A/B tests and personalized experiences.

3. **Unified Customer Profiles:**
   - **Adobe Experience Platform** serves as the central hub for combining data from **Adobe Analytics**, **Audience Manager**, and **Target** to create a unified customer profile.
   
4. **Personalized Experience Delivery:**
   - With data collected from all platforms, **Adobe Target** can deliver highly personalized content to users based on real-time insights from **Adobe Analytics** and **Adobe Audience Manager**.

5. **Cross-Device Tracking:**
   - **Adobe Experience Cloud ID Service** ensures user tracking and data consistency across devices, which is essential for delivering seamless experiences and accurate reporting.

---

This interconnected ecosystem allows marketers to manage the full customer journey, from data collection and analysis to audience segmentation and personalized marketing, all while ensuring data consistency across platforms.

---


# AEM dispatcher cache for specific URL params
## Problem: AEM dispatcher, if a specific URL contains utm parameter then it should load from the dispatcher cache instead of the publisher instance.

To configure Adobe Experience Manager (AEM) Dispatcher to serve cached content when a URL contains specific UTM parameters, you'll need to modify the Dispatcher configuration files. The goal is to ensure that URLs with UTM parameters still utilize the cache effectively, rather than bypassing it and hitting the AEM Publisher instance.

Here's how you can achieve this:
### Step-by-Step Guide

- Modify the Dispatcher Configuration:

  - The Dispatcher configuration is usually found in the dispatcher.any or dispatcher_vhost.conf file, depending on your setup.
- Ignore Query Parameters for Caching:

  - You need to instruct the Dispatcher to ignore the UTM query parameters when caching. This ensures that URLs with different UTM parameters are treated as the same URL for caching purposes.

- Set up Dispatcher Filters:

  - Configure the Dispatcher to pass certain requests and cache others based on the presence of UTM parameters.

Here's an example configuration snippet for dispatcher.any:


```console
# dispatcher.any configuration

/cache
{
    /rules
    {
        /0000
        {
            # Cache everything except URLs with specific UTM parameters
            /glob "*"
            /type "allow"
        }
    }

    /ignoreUrlParams
    {
        # Add UTM parameters here to be ignored for caching purposes
        /0001 { /glob "utm_source*" }
        /0002 { /glob "utm_medium*" }
        /0003 { /glob "utm_campaign*" }
        /0004 { /glob "utm_term*" }
        /0005 { /glob "utm_content*" }
    }
}


```


### Additional Configuration:

Ensure your web server (e.g., Apache, Nginx) is configured correctly to support the Dispatcher settings. For Apache, you might have configurations similar to this:

```console
# dispatcher_vhost.conf

<IfModule disp_apache2.c>
    # Dispatcher configuration
    DispatcherConfig conf/dispatcher.any
    DispatcherLog /var/log/dispatcher.log
    DispatcherLogLevel 3
    DispatcherUseProcessedURL 0
    DispatcherPassError 0
    DispatcherNoServerHeader 0
    DispatcherDeclineRoot 0
    DispatcherKeepAliveTimeout 60

    # Cache directory
    DispatcherCacheRoot /path/to/dispatcher/cache
</IfModule>

<Directory "/path/to/dispatcher/cache">
    Require all granted
</Directory>


```

---

# Find duplicate [id] selectors on page
## Problem: For ADA perspectives and standrad, selector id attribute should not duplicates on page.

To find duplicate IDs on a web page and associate them with their corresponding selectors, you can create an object that stores the duplicate IDs as keys and an array of their associated selectors as values. Here's an example code snippet:

```javascript
function findDuplicateIdsWithSelectors() {
  const elements = document.querySelectorAll('[id]');
  const idMap = new Map();
  const duplicateIds = {};

  elements.forEach((element) => {
    const id = element.id;
    const selector = getSelector(element);

    if (idMap.has(id)) {
      if (!duplicateIds[id]) {
        duplicateIds[id] = [idMap.get(id)];
      }
      duplicateIds[id].push(selector);
    } else {
      idMap.set(id, selector);
    }
  });

  return duplicateIds;
}

function getSelector(element) {
  if (!element) return '';

  let selector = element.tagName.toLowerCase();
  if (element.id) {
    selector += `#${element.id}`;
  } else {
    selector += Array.from(element.classList).map((className) => `.${className}`).join('');
  }

  return selector;
}

// Usage
const duplicateIdsWithSelectors = findDuplicateIdsWithSelectors();
console.log(duplicateIdsWithSelectors);

```

---


# Locale weekdays and months accordingly to language code
## Problem: weekdays and months in the user's language using JavaScript


I have defined a getWeekdays function that takes a language code as a parameter and returns an array of weekday names in the specified language. The function creates an empty array weekdays, then initialises a Date object to the current date. We then use a for loop to iterate over the next 7 days and add the localised weekday name to the weekdays array using the toLocaleDateString method. This method formats the date according to the user's locale and options specified in the options object.

```javascript
function getWeekdays(lang) {
 const weekdays = [];
 const date = new Date();
 const options = { weekday: 'long' };
 for (let i = 0; i < 7; i++) {
   date.setDate(date.getDate() + 1);
   weekdays.push(date.toLocaleDateString(lang, options));
 }
 return weekdays;
}


// Example usage
const lang = 'fr-FR'; // Pass the user's language code here
const weekdays = getWeekdays(lang);
console.log(weekdays); // Output: ["lundi", "mardi", "mercredi", "jeudi", "vendredi", "samedi", "dimanche"]
```


Also I have defined a getMonths function that takes a language code as a parameter and returns an array of month names in the specified language. The function creates an empty array of months, then initialises a Date object to the current date. We then use a for loop to iterate over the 12 months of the year and add the localised month name to the months array using the toLocaleDateString method. This method formats the date according to the user's locale and options specified in the options object.


```javascript
function getMonths(lang) {
 const months = [];
 const date = new Date();
 const options = { month: 'long' };
 for (let i = 0; i < 12; i++) {
   date.setMonth(i);
   months.push(date.toLocaleDateString(lang, options));
 }
 return months;
}


// Example usage
const lang = 'fr-FR'; // Pass the user's language code here
const months = getMonths(lang);
console.log(months); // Output: ["janvier", "février", "mars", "avril", "mai", "juin", "juillet", "août", "septembre", "octobre", "novembre", "décembre"]
```


---




# Wildcard search in array list and check exist in list
## Problem: Search in Array list using wildcard (*) and also check if such text exist in it.


```javascript
const list = ["mon", "tue", "wed", "thu", "fri", "sat", "sun"];


class Dict {
 constructor(list) {
   this.dict = list;
 }
 inRegEx(word) {
   const regTemplate = word.replaceAll("*", "(.*)");
   return new RegExp(`^${regTemplate}$`, "g");
 }
 isInDict(word) {
   return this.dict.some((exp) => {
     return this.inRegEx(word).test(exp);
   });
 }
 inAll(word) {
   return this.dict.filter((el) => {
     return el.match(this.inRegEx(word));
   });
 }
}


const ls = new Dict(list);


console.log(ls.isInDict("m*n")); // true
console.log(ls.inAll("t*")); // ["tue","thu"]


```
---


# Add two strings with numbers


## Problem: algorithm which contains a function that takes in any two numbers but in the form of strings, and returns the sum as a string.
```javascript
var str1 = "94";
var str2 = "09";


// Pad the shorter string with leading zeroes
while (str1.length < str2.length) {
 str1 = "0" + str1;
}
while (str2.length < str1.length) {
 str2 = "0" + str2;
}


var result = "";
var carry = 0;


// Iterate over the strings from right to left
for (var i = str1.length - 1; i >= 0; i--) {
 var digit1 = parseInt(str1.charAt(i));
 var digit2 = parseInt(str2.charAt(i));
 var sum = digit1 + digit2 + carry;
 carry = Math.floor(sum / 10);
 var digitSum = sum % 10;
 result = digitSum.toString() + result;
}


// If there's still a carry after iterating over all digits, add it to the result
if (carry > 0) {
 result = carry.toString() + result;
}


console.log(result); // Output: "103"
```


---
# ResourceLoader component that includes cache busting and async parameters:


## Problem: create custom resource loaders for scripts and stylesheets. with feature of cache busting.




```htm
<!DOCTYPE html>
<html>
 <head>
   <script>
     class ResourceLoader extends HTMLElement {
       constructor() {
         super();
         this.attachShadow({ mode: "open" });


         const type = this.getAttribute("type");
         const src = this.getAttribute("src");


         if (type === "script") {
           const script = document.createElement("script");
           script.src = this._addCacheBuster(src);
           script.async = this.hasAttribute("async");
           this.shadowRoot.appendChild(script);
         } else if (type === "stylesheet") {
           const link = document.createElement("link");
           link.rel = "stylesheet";
           link.href = this._addCacheBuster(src);
           this.shadowRoot.appendChild(link);
         }
       }


       _addCacheBuster(src) {
         const cacheBuster = new Date().getTime();
         return `${src}?cache=${cacheBuster}`;
       }
     }


     customElements.define("resource-loader", ResourceLoader);
   </script>


   <!-- Use the custom element as a tag to load a script and a stylesheet with cache busting and async parameters -->
   <resource-loader type="script" src="https://code.jquery.com/jquery-3.6.0.min.js" async></resource-loader>
   <resource-loader type="stylesheet" src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css"></resource-loader>
 </head>
 <body>
   <!-- Your HTML content here -->
 </body>
</html>


```
In this version of the component, we've added a private _addCacheBuster() method that appends a cache-busting query parameter to the end of the URL to force the browser to fetch a fresh version of the resource each time. We've also added an async attribute to the script tag if the async attribute is present on the custom element.


---


# Execute all events and functions from external JSON without modify javascript code frequently, all instructions should be part of json file


## Problem: remote JSON to execute script without releasing entire script.


To execute all events and functions from an external JSON file without modifying the JavaScript code frequently, you can include all the necessary instructions in the JSON file itself.


Example JSON file that includes instructions for executing events and functions:


```json
{
 "events": [
   {
     "name": "onClick",
     "handler": "console.log('onClick event triggered');"
   },
   {
     "name": "onMouseOver",
     "handler": "console.log('onMouseOver event triggered');"
   }
 ],
 "functions": [
   {
     "name": "sayHello",
     "handler": "console.log('Hello!');"
   },
   {
     "name": "sayGoodbye",
     "handler": "console.log('Goodbye!');"
   }
 ]
}


```


In this JSON file, the events and functions properties are arrays of objects. Each object represents an event or function and includes a name property and a handler property. The name property specifies the name of the event or function, and the handler property contains the JavaScript code to execute when the event or function is triggered.


To execute the events and functions defined in the JSON file, you can use the following code:


```javascript
// load the JSON file
fetch('events.json')
 .then(response => response.json())
 .then(data => {
   // execute all events
   data.events.forEach(event => {
     window[event.name] = new Function(event.handler);
     window[event.name]();
   });


   // execute all functions
   data.functions.forEach(func => {
     window[func.name] = new Function(func.handler);
     window[func.name]();
   });
 })
 .catch(error => console.error(error));


```


In this code, we use the forEach() method to iterate over the events and functions arrays in the JSON data. For each event or function, we create a new function using the Function constructor and assign it to a property on the window object with the same name as the event or function. We can then call the event or function using the **window\[event.name\]\(\)** syntax.


---


# Identify selector is visible or not


## Problem: selector is used to identify whether an HTML element is currently visible on the webpage or not.


Check HTML element's visibility including parent visibility.


```javascript
function isElementVisible(selector) {
 const element = document.querySelector(selector);

  if (!element) {
    // Element not found in the document
    return false;
  }

  const rect = element.getBoundingClientRect();

  if (rect.width === 0 && rect.height === 0) {
    // Element has no width or height (hidden)
    return false;
  }

  const style = window.getComputedStyle(element);
  if (style.display === 'none' || style.visibility === 'hidden' || style.opacity === '0') {
    // Element is hidden by CSS properties
    return false;
  }

  if (rect.top >= window.innerHeight || rect.bottom <= 0) {
    // Element is outside the viewport vertically
    return false;
  }

  if (rect.left >= window.innerWidth || rect.right <= 0) {
    // Element is outside the viewport horizontally
    return false;
  }

  // Element is visible on the screen
  return true;
}
```


---



