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

- Copying an array and modifying the copy without affecting the original.
- Avoids mutating the original array.
- Useful for state management in React.

<br>

> Achieved via adding new functions to `Array.prototype.`

---

## New functions in `Array.prototype`

- toReversed() -> Array
- toSorted(compareFn) -> Array
- toSpliced(start, deleteCount, ...items) -> Array
- with(index, value) -> Array

## Difference in usage

```javascript
const arr = [1, 2, 3, 4, 5];
arr.sort();  // Mutates the original array

const newArr = arr.toSorted(); // Returns a sorted copy
```

---

# Example for `Array.with`

```javascript
const arr = [1, 2, 3, 4, 5];
const newArr = arr.with(2, 10);

console.log(arr);  // Outputs: [1, 2, 3, 4, 5]

console.log(newArr);  // Outputs: [1, 2, 10, 4, 5]
```

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

- Classes are syntactic sugar over JavaScript's prototype-based inheritance.
- Classes are special functions.
- Class declarations are not hoisted.

---

# Class declarations

```javascript
class Rectangle extends Shape {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  calcArea() {
    return this.height * this.width;
  }

  get area() {
    return this.calcArea();
  }
}

const square = new Rectangle(10, 10);

console.log(square.area); // Outputs: 100
```

---

# Class use-cases

#### Classes are good for:

- Reusable components
- Encapsulation of related variables and functions
- Inheritance (especially for built-in objects)
- Method overriding
- Static methods

<br>

#### Additionally for react:

- State management
- Lifecycle methods
- Event handling

---

# Decorators

- Functions that can be used to modify class declarations and methods.

```javascript
function logged(value, { kind, name }) {
  if (kind === "method" || kind === "getter" || kind === "setter") {
    return function (...args) {
      console.log(`starting ${name} with arguments ${args.join(", ")}`);
      const ret = value.call(this, ...args);
      console.log(`ending ${name}`);
      return ret;
    };
  }
}

class C {
  @logged
  set x(arg) {}
}

new C().x = 1
// starting x with arguments 1
// ending x
```

---

# Private fields and methods

- Fields and methods that are only accessible within the class.

```javascript
class Rectangle {
  #height;
  #width;

  constructor(height, width) {
    this.#height = height;
    this.#width = width;
  }

  #calcArea() {
    return this.#height * this.#width;
  }

  get area() {
    return this.#calcArea();
  }
}
```

---

# Static methods

- Methods that are called directly on the class, not on an instance of the class.

```javascript
class Rectangle {
  static fromSquare(square) {
    return new Rectangle(square, square);
  }
}

const square = new Rectangle(10, 10);
```

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