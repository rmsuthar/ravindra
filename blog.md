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



