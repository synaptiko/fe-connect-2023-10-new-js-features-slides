---
theme: ./theme
layout: cover
class: text-center
highlighter: monaco
lineNumbers: false
transition: slide-left
title: New JS features
mdc: true
---

# New JS features

by Ozgur & Jiri
<br>
<small>(& ChatGPT with DALL-E 3)</small>


---
layout: cover
background: ./images/intro0.png
dim: false
---

---
layout: cover
background: ./images/intro1.png
dim: false
---

---
layout: cover
background: ./images/intro2.png
dim: false
---

---
layout: image-right
image: ./images/overview.png
---

# Overview

- WeakMap and Symbols
- Change Array by Copy
- Top-level await
- Usage of class & its recent improvements
- String.prototype.replaceAll

---
layout: image-right
image: ./images/weak-map.png
---

# What is WeakMap?

- Special type of Map allowing keys & values to be garbage-collected if not referenced elsewhere.

<br><br>

# Usages of WeakMap

- Managing memory efficiently, particularly when using objects as keys.
- Storing private data for objects.
- Associating metadata with DOM nodes.
- Caching computed results tied to objects.

---

# Syntax of WeakMap

```javascript
const weakMap = new WeakMap();
const key = { any: 'object' };

weakMap.set(key, value);
weakMap.get(key);
weakMap.has(key);
```

<br><br>

# Example 1 of WeakMap

```javascript
const domMetadata = new WeakMap();
const div = document.createElement('div');

domMetadata.set(div, { clicks: 0 });

div.addEventListener('click', () => {
  const { clicks } = domMetadata.get(div);
  domMetadata.set(div, { clicks: clicks + 1 });
});
```

---

# Example 2 of WeakMap

```javascript
const computedCache = new WeakMap();

function compute(obj) {
  if (computedCache.has(obj)) {
    return computedCache.get(obj);
  }
  
  const result = /* ...expensive computation... */;

  computedCache.set(obj, result);

  return result;
}
```

---

# Benefits of WeakMap

- Helps prevent memory leaks in applications.
- Enables better memory management practices.

---
layout: image-right
image: ./images/symbol.png
---

# What is Symbol?

- Creation of unique identifiers for object keys to prevent name clashes.

```javascript
const uniqueKey = Symbol('my unique key');
const obj = { [uniqueKey]: 'value' };
```

<br>

- Define custom iteration behaviors or other meta-programming tasks.
  - **Example:** Defining custom iteration with `[Symbol.iterator]`.

---
layout: image-right
image: ./images/symbols-in-weakmap.png
---

# Usage of Symbols as keys in WeakMap

- Easier creation and sharing of keys.
- Improved ergonomics of `WeakMap``, clarifying roles of keys and mapped items.
- Symbols, as unique primitive values, ensure distinctive key-value pairs in WeakMap.

```javascript
const weakMap = new WeakMap();
const symbol = Symbol('my symbol');

weakMap.set(symbol, 'my value');
console.log(weakMap.get(symbol));  // Outputs: 'my value'
```

---
layout: image-right
image: ./images/array-copy.png
---

# Change Array by Copy

**TODO**

---
layout: image-right
image: ./images/top-level-await.png
---

# Top-level await

- Allowing modules to behave like async functions.
- Modules importing a top-level await module will wait for it to load before evaluating.
- Streamlines promise handling, avoiding promise chaining.
- Useful for scripting in Node.js, Deno and Bun
- Available in Node.js 14+

---

# Example 1 of Top-level await

```javascript
import { fetchData } from 'data-module';

console.log(await fetchData());
```

# Example 2 of Top-level await

```javascript
const dbConnection = await connectToDatabase();
const server = await startServer();
```

---

# Benefits of Top-level await

- Better management of asynchronous operations in module initialization.
- Cleaner, more readable code with easier async handling.

---
layout: image-right
image: ./images/classes.png
---

# Usage of class & its recent improvements

**TODO**

---
layout: image-right
image: ./images/replace-all.png
---

# String.prototype
# &nbsp;&nbsp;&nbsp;.replaceAll

- Replaces all occurrences of a substring or pattern with another substring.
- Simplifies global replacement operations.
- More readable code compared to using `String.prototype.replace` with a global regex.

```javascript
const phone = '+420 744 40 31 40';
const noSpaces = phone.replaceAll(' ', '');
// => +420744403140
// no need to convert ' ' to / /g
```

---
layout: cover
background: ./images/thank-you.png
dim: false
---