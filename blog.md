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