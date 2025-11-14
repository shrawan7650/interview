# JavaScript Interview Preparation Guide
## Complete Roadmap: Beginner → Intermediate → Advanced → Coding → Tricky

---

## 📋 Table of Contents
1. [Basics](#basics)
2. [Intermediate](#intermediate)
3. [Advanced](#advanced)
4. [Coding Problems](#coding-problems)
5. [Tricky Questions & Gotchas](#tricky-questions--gotchas)
6. [Mock Interview Scripts](#mock-interview-scripts)

---

## BASICS

### 1. Data Types (Primitives)

**Question:** What are the primitive data types in JavaScript?

**Short Answer:** JavaScript has 7 primitive types: `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, and `null`.

**Deep Explanation:**
Primitives are immutable values stored directly in memory. Unlike objects, they don't have methods (though JS temporarily wraps them in objects when you access properties). Understanding primitives is crucial because they behave differently than reference types in assignments and comparisons.

**Example:**
```javascript
// Primitives
const str = "hello";           // string
const num = 42;                // number
const big = 9007199254740991n; // bigint
const bool = true;             // boolean
const undef = undefined;       // undefined
const sym = Symbol('id');      // symbol
const nil = null;              // null (typeof shows 'object' - historic bug)

// Primitives are immutable
let x = "test";
x.toUpperCase(); // Returns "TEST" but doesn't change x
console.log(x);  // Still "test"

// Primitives are compared by value
console.log(5 === 5);          // true
console.log("a" === "a");      // true
```

**Complexity/Impact:** O(1) for primitive operations and comparisons.

**Edge Cases & Tests:**
```javascript
// Edge cases
console.log(typeof null);           // "object" (historical bug)
console.log(typeof undefined);      // "undefined"
console.log(NaN === NaN);          // false (use Number.isNaN())
console.log(0.1 + 0.2 === 0.3);    // false (floating point precision)
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
```

**How to explain in interview:**
"JavaScript has 7 primitive types that are immutable and stored by value. The key distinction from objects is that primitives are compared by value, while objects are compared by reference."

**Common Mistakes:**
- Confusing `null` and `undefined`
- Using `typeof null` expecting "null"
- Not understanding `NaN` is not equal to itself

**Quick Practice:**
1. Write a function that checks if a value is a primitive
2. Explain why `typeof null === "object"`

---

### 2. Variables (var, let, const)

**Question:** What's the difference between var, let, and const?

**Short Answer:** `var` is function-scoped and hoisted, `let` and `const` are block-scoped; `const` cannot be reassigned.

**Deep Explanation:**
- `var`: Function-scoped, hoisted (initialized as `undefined`), can be redeclared
- `let`: Block-scoped, hoisted but not initialized (Temporal Dead Zone), cannot be redeclared
- `const`: Block-scoped, hoisted but not initialized, cannot be reassigned (but objects/arrays are mutable)

**Example:**
```javascript
// var - function scoped
function testVar() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10 (accessible outside block)
}

// let - block scoped
function testLet() {
  if (true) {
    let y = 20;
  }
  // console.log(y); // ReferenceError
}

// const - cannot reassign
const z = 30;
// z = 40; // TypeError

// But objects are mutable
const obj = { a: 1 };
obj.a = 2; // OK
obj.b = 3; // OK
// obj = {}; // TypeError

// Hoisting difference
console.log(a); // undefined (var hoisted)
var a = 5;

// console.log(b); // ReferenceError (TDZ)
let b = 10;
```

**Complexity/Impact:** Understanding scope prevents bugs and memory leaks.

**Edge Cases & Tests:**
```javascript
// var in loops
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // 3, 3, 3
}

// let in loops
for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 100); // 0, 1, 2
}

// const with objects
const arr = [1, 2, 3];
arr.push(4); // OK
// arr = []; // TypeError
```

**How to explain in interview:**
"Use `const` by default for immutability intent, `let` when you need reassignment, and avoid `var` due to function-scope and hoisting confusion."

**Common Mistakes:**
- Using `var` in loops with async callbacks
- Thinking `const` makes objects immutable
- Not understanding Temporal Dead Zone


//Notes/n

JavaScript code jab run hota hai, to pehle memory allocate hoti hai (creation phase).

let x ka naam memory me aa jaata hai (hoisting hoti hai, but without initialization).

Lekin jab tak line let x = 10; execute nahi hoti, tab tak x TDZ me rehta hai.
**Quick Practice:**
1. Fix the `var` loop issue using `let`
2. Demonstrate TDZ with an example :-
TDZ (Temporal Dead Zone) wo time period hota hai jab ek variable declare to ho chuka hota hai,
lekin access nahi kiya ja sakta — kyunki abhi initialize nahi hua hota.

---

### 3. Scope & Hoisting

**Question:** Explain JavaScript scope and hoisting.

**Short Answer:** Scope determines variable accessibility; hoisting moves declarations to the top of their scope before execution.

**Deep Explanation:**
- **Scope types:** Global, function, block (ES6+)
- **Hoisting:** `var` declarations and function declarations are hoisted; `let`/`const` are hoisted but not initialized (TDZ)
- **Lexical scope:** Inner functions access outer scope variables

**Example:**
```javascript
// Function scope
function outer() {
  const x = 10;

  function inner() {
    console.log(x); // 10 (lexical scope)
  }

  inner();
}

// Hoisting - var
console.log(a); // undefined (hoisted, not initialized)
var a = 5;

// Equivalent to:
// var a;
// console.log(a);
// a = 5;

// Hoisting - function
greet(); // "Hello" (function hoisted)
function greet() {
  console.log("Hello");
}

// Function expressions NOT hoisted
// sayHi(); // TypeError
const sayHi = function() {
  console.log("Hi");
};

// Block scope
{
  let blockScoped = 100;
  var functionScoped = 200;
}
// console.log(blockScoped); // ReferenceError
console.log(functionScoped); // 200
```

**Complexity/Impact:** Scope chain lookup is O(n) where n is depth, but usually optimized.

**Edge Cases & Tests:**
```javascript
// Shadowing
let x = 1;
{
  let x = 2;
  console.log(x); // 2
}
console.log(x); // 1

// Closure capturing
function makeCounter() {
  let count = 0;
  return function() {
    return ++count;
  };
}
const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

**How to explain in interview:**
"Hoisting moves declarations to the top of their scope. Function declarations are fully hoisted, while `var` is hoisted but initialized as `undefined`. `let` and `const` are hoisted but remain in the Temporal Dead Zone until declaration."

**Common Mistakes:**
- Expecting `let`/`const` to behave like `var` with hoisting
- Not understanding closure scope retention
- Confusing block scope with function scope

**Quick Practice:**
1. Predict the output of hoisting examples
2. Create a closure that captures a variable /n

Note/n

Lexical Scope ka matlab hota hai —
ek variable kis jagah (block/function) likha gaya hai, usi ke base pe decide hota hai ki wo kahaan tak accessible hoga.

---

### 4. Equality (== vs ===)

**Question:** What's the difference between == and ===?

**Short Answer:** `==` performs type coercion before comparison; `===` checks both value and type without coercion.

**Deep Explanation:**
`==` (loose equality) converts operands to the same type before comparison, following complex coercion rules. `===` (strict equality) returns false if types differ. Always prefer `===` to avoid unexpected behavior.

**Example:**
```javascript
// Strict equality (===)
console.log(5 === 5);       // true
console.log(5 === "5");     // false (different types)
console.log(true === 1);    // false

// Loose equality (==)
console.log(5 == "5");      // true (string coerced to number)
console.log(true == 1);     // true (boolean to number)
console.log(false == 0);    // true
console.log(null == undefined); // true (special case)

// Weird coercions
console.log([] == false);   // true
console.log([] == ![]);     // true (tricky!)
console.log("" == 0);       // true
console.log(" " == 0);      // true
```

**Complexity/Impact:** O(1) comparison, but coercion adds cognitive overhead.

**Edge Cases & Tests:**
```javascript
// NaN
console.log(NaN === NaN);   // false
console.log(NaN == NaN);    // false
console.log(Number.isNaN(NaN)); // true (correct way)

// Object comparison (both == and === compare references)
console.log({} == {});      // false
console.log({} === {});     // false

// null and undefined
console.log(null === undefined); // false
console.log(null == undefined);  // true
```

**How to explain in interview:**
"Always use `===` for predictable comparisons. `==` performs type coercion which can lead to unexpected results like `[] == ![]` being true."

**Common Mistakes:**
- Using `==` and encountering weird coercions
- Comparing objects with `==` or `===` expecting value comparison
- Not knowing `NaN !== NaN`


[1] == 1        // true  → [1].toString() → '1' → number 1 /n

[1,2] == '1,2'  // true  → both strings /n

[] == ''        // true  → [].toString() → ''  /n

[] == 0         // true  → '' → 0 /n

[null] == 0     // true  → '0' → 0 /n

[undefined] == 0 // true → '' → 0 /n



**Quick Practice:**
1. Explain why `[] == ![]` is true
2. Write a function that safely compares two values

/n
| Type Pair          | Conversion Rule                           | Example             | Result |
| ------------------ | ----------------------------------------- | ------------------- | ------ |
| String & Number    | String → Number                           | `'5' == 5`          | true   |
| Boolean & Any      | Boolean → Number                          | `true == 1`         | true   |
| null & undefined   | Equal only to each other                  | `null == undefined` | true   |
| Object & Primitive | Object → primitive (via toString/valueOf) | `[1] == 1`          | true   |



---

### 5. Functions (Declaration, Expression, Arrow)

**Question:** What are the different ways to define functions in JavaScript?

**Short Answer:** Function declarations, function expressions, arrow functions, and methods. Each has different hoisting and `this` binding behavior.

**Deep Explanation:**
- **Function Declaration:** Hoisted, has its own `this`, can be called before definition
- **Function Expression:** Not hoisted, assigned to variable
- **Arrow Function:** Lexical `this`, no `arguments` object, cannot be constructor
- **Method:** Function as object property

**Example:**
```javascript
// Function Declaration (hoisted)
console.log(add(2, 3)); // 5
function add(a, b) {
  return a + b;
}

// Function Expression (not hoisted)
// console.log(subtract(5, 2)); // TypeError
const subtract = function(a, b) {
  return a - b;
};

// Arrow Function (concise, lexical this)
const multiply = (a, b) => a * b;
const square = x => x * x; // Single param, no parens needed
const greet = () => "Hello"; // No params

// Multi-line arrow function
const divide = (a, b) => {
  if (b === 0) throw new Error("Division by zero");
  return a / b;
};

// Method
const calculator = {
  value: 0,
  add(n) {
    this.value += n;
    return this;
  },
  getValue() {
    return this.value;
  }
};

// this binding difference
const obj = {
  name: "Object",
  regularFunc: function() {
    console.log(this.name); // "Object"
  },
  arrowFunc: () => {
    console.log(this.name); // undefined (lexical this from outer scope)
  }
};
```

**Complexity/Impact:** Function calls are O(1) overhead, but recursion depth matters.

**Edge Cases & Tests:**
```javascript
// Arrow functions don't have arguments
const regular = function() {
  console.log(arguments); // [1, 2, 3]
};
regular(1, 2, 3);

const arrow = (...args) => {
  console.log(args); // Use rest parameters instead
};
arrow(1, 2, 3);

// Arrow functions can't be constructors
const Person = (name) => {
  this.name = name;
};
// new Person("John"); // TypeError

// Default parameters
function greet(name = "Guest") {
  return `Hello, ${name}`;
}
console.log(greet());      // "Hello, Guest"
console.log(greet("John")); // "Hello, John"
```

**How to explain in interview:**
"Function declarations are hoisted and have dynamic `this`. Arrow functions have lexical `this` and are ideal for callbacks. Use arrow functions for cleaner syntax, but avoid them as methods when you need `this` binding."

**Common Mistakes:**
- Using arrow functions as methods expecting `this` binding
- Not understanding hoisting differences
- Forgetting parentheses in arrow functions with object returns

**Quick Practice:**
1. Convert a regular function to arrow function
2. Demonstrate `this` binding difference

---

### 6. Arrays

**Question:** What are common array methods and their use cases?

**Short Answer:** Arrays have methods for iteration (forEach, map, filter, reduce), search (find, indexOf), mutation (push, pop, splice), and more.

**Deep Explanation:**
Arrays are ordered collections with zero-based indexing. Modern JS provides functional methods that don't mutate (map, filter, reduce) and imperative methods that do (push, splice). Understanding mutability is crucial.

**Example:**
```javascript
const arr = [1, 2, 3, 4, 5];

// Iteration
arr.forEach(x => console.log(x)); // Side effects

// Transformation (immutable)
const doubled = arr.map(x => x * 2); // [2, 4, 6, 8, 10]
const evens = arr.filter(x => x % 2 === 0); // [2, 4]
const sum = arr.reduce((acc, x) => acc + x, 0); // 15

// Search
const found = arr.find(x => x > 3); // 4
const index = arr.findIndex(x => x > 3); // 3
const includes = arr.includes(3); // true

// Mutation
arr.push(6);        // Add to end
arr.pop();          // Remove from end
arr.unshift(0);     // Add to start
arr.shift();        // Remove from start
arr.splice(1, 2, 'a', 'b'); // Remove 2 items at index 1, add 'a', 'b'

// Other useful methods
const sliced = arr.slice(1, 3); // Extract portion (immutable)
const joined = arr.join('-');   // Convert to string
const reversed = [...arr].reverse(); // Reverse (copy first to avoid mutation)

// ES6+ methods
const flat = [1, [2, [3, 4]]].flat(2); // [1, 2, 3, 4]
const mapped = [1, 2].flatMap(x => [x, x * 2]); // [1, 2, 2, 4]
```

**Complexity/Impact:**
- forEach, map, filter, reduce: O(n)
- find, findIndex: O(n) worst case
- push, pop: O(1)
- shift, unshift: O(n)
- splice: O(n)
- slice: O(k) where k is slice length

**Edge Cases & Tests:**
```javascript
// Empty array edge cases
[].reduce((a, b) => a + b); // TypeError (no initial value)
[].reduce((a, b) => a + b, 0); // 0 (with initial value)

// Sparse arrays
const sparse = [1, , 3]; // [1, empty, 3]
sparse.forEach(x => console.log(x)); // 1, 3 (skips empty)
sparse.map(x => x * 2); // [2, empty, 6]

// Array.from and spread
const str = "hello";
const chars = Array.from(str); // ['h', 'e', 'l', 'l', 'o']
const copy = [...arr]; // Shallow copy

// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first, second, rest); // 1, 2, [3, 4, 5]
```

**How to explain in interview:**
"Prefer immutable methods like map, filter, and reduce for functional programming. Be aware of time complexity—shift/unshift are O(n) because they reindex the entire array."

**Common Mistakes:**
- Mutating arrays when immutability is needed
- Using reduce without an initial value on empty arrays
- Confusing slice (immutable) with splice (mutable)

**Quick Practice:**
1. Implement custom map, filter, reduce
2. Remove duplicates from an array

/n
| Method      | Changes original array? | Returns new array? |
| ----------- | ----------------------- | ------------------ |
| `push()`    | ✅ Yes                   | ❌ No               |
| `pop()`     | ✅ Yes                   | ❌ No               |
| `shift()`   | ✅ Yes                   | ❌ No               |
| `unshift()` | ✅ Yes                   | ❌ No               |
| `splice()`  | ✅ Yes                   | ❌ No               |
| `sort()`    | ✅ Yes                   | ❌ No               |
| `reverse()` | ✅ Yes                   | ❌ No               |
| `map()`     | ❌ No                    | ✅ Yes              |
| `filter()`  | ❌ No                    | ✅ Yes              |
| `slice()`   | ❌ No                    | ✅ Yes              |
| `concat()`  | ❌ No                    | ✅ Yes              |


---

### 7. Objects

**Question:** How do you work with objects in JavaScript?

**Short Answer:** Objects are key-value pairs accessed via dot or bracket notation. They can be created with literals, constructors, or Object.create().

**Deep Explanation:**
Objects are reference types that store collections of properties. Keys are strings or symbols. Understanding prototype chain, property descriptors, and object methods (Object.keys, Object.assign, etc.) is essential.

**Example:**
```javascript
// Object literal
const person = {
  name: "John",
  age: 30,
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

// Access properties
console.log(person.name);      // Dot notation
console.log(person["age"]);    // Bracket notation (useful for dynamic keys)

// Add/modify properties
person.email = "john@example.com";
person.age = 31;

// Delete properties
delete person.email;

// Check property existence
console.log("name" in person);        // true
console.log(person.hasOwnProperty("name")); // true

// Object methods
const keys = Object.keys(person);     // ["name", "age", "greet"]
const values = Object.values(person); // ["John", 31, function]
const entries = Object.entries(person); // [["name", "John"], ...]

// Object.assign (shallow copy/merge)
const defaults = { theme: "dark", lang: "en" };
const userPrefs = { theme: "light" };
const config = Object.assign({}, defaults, userPrefs);
// or with spread: const config = { ...defaults, ...userPrefs };

// Object destructuring
const { name, age } = person;
console.log(name, age); // "John", 31

// Rest in destructuring
const { name: userName, ...rest } = person;
console.log(userName, rest); // "John", { age: 31, greet: [Function] }

// Computed property names
const key = "dynamic";
const obj = {
  [key]: "value",
  [`${key}Key`]: "another"
};

// Shorthand property names
const x = 10, y = 20;
const point = { x, y }; // { x: 10, y: 20 }
```

**Complexity/Impact:**
- Property access: O(1) average
- Object.keys/values/entries: O(n)
- Object.assign/spread: O(n)

**Edge Cases & Tests:**
```javascript
// Objects are passed by reference
const original = { a: 1 };
const reference = original;
reference.a = 2;
console.log(original.a); // 2

// Shallow vs deep copy
const nested = { a: { b: 1 } };
const shallowCopy = { ...nested };
shallowCopy.a.b = 2;
console.log(nested.a.b); // 2 (nested object still shared)

// Deep copy (JSON method - limitations)
const deepCopy = JSON.parse(JSON.stringify(nested));
// Doesn't copy functions, undefined, symbols, dates correctly
Method 2 — structuredClone() (modern JS)
const deepCopy = structuredClone(user);
deepCopy.address.city = "Chennai";

console.log(user.address.city); // ✅ Still 'Delhi'


Best modern approach — handles most data types properly (except functions).
// Property descriptors
Object.defineProperty(obj, 'readOnly', {
  value: 42,
  writable: false,
  enumerable: true,
  configurable: false
});

// Freeze, seal
const frozen = Object.freeze({ a: 1 });
// frozen.a = 2; // Fails in strict mode
```

**How to explain in interview:**
"Objects store key-value pairs and are passed by reference. Use Object.keys/values/entries to iterate, spread operator for shallow copies, and be mindful that nested objects are still references."

**Common Mistakes:**
- Thinking spread creates deep copies
- Using JSON.parse/stringify for cloning objects with functions
- Not checking property existence before access

**Quick Practice:**
1. Implement deep clone function
2. Merge two objects with nested properties

---

### 8. DOM Basics

**Question:** How do you manipulate the DOM in JavaScript?

**Short Answer:** Use methods like `getElementById`, `querySelector`, `createElement`, and properties like `innerHTML`, `textContent` to interact with HTML elements.

**Deep Explanation:**
The DOM (Document Object Model) is a tree structure representing HTML. JavaScript can select, create, modify, and remove elements. Modern approach uses `querySelector` for flexibility and avoids `innerHTML` for security when possible.

**Example:**
```javascript
// Selection
const byId = document.getElementById('myId');
const byClass = document.getElementsByClassName('myClass')[0];
const byTag = document.getElementsByTagName('div')[0];

// Modern selectors (prefer these)
const single = document.querySelector('.class'); // First match
const multiple = document.querySelectorAll('div.item'); // NodeList

// Creation
const div = document.createElement('div');
div.className = 'container';
div.textContent = 'Hello World';

// Modification
div.innerHTML = '<span>Updated</span>'; // Parses HTML (XSS risk)
div.textContent = 'Safe Text'; // Text only (safe)
div.style.color = 'blue';
div.setAttribute('data-id', '123');

// Adding to DOM
document.body.appendChild(div);
document.body.insertBefore(div, referenceElement);

// Removal
div.remove(); // Modern
// or parent.removeChild(div); // Old way

// Traversal
const parent = div.parentElement;
const children = div.children;
const firstChild = div.firstElementChild;
const nextSibling = div.nextElementSibling;

// Class manipulation
div.classList.add('active');
div.classList.remove('inactive');
div.classList.toggle('visible');
div.classList.contains('active'); // true

// Data attributes
div.dataset.userId = '456'; // <div data-user-id="456">
console.log(div.dataset.userId); // "456"
```

**Complexity/Impact:**
- getElementById: O(1) typically
- querySelector: O(n)
- DOM manipulation causes reflow/repaint (expensive)

**Edge Cases & Tests:**
```javascript
// querySelector returns null if not found
const notFound = document.querySelector('.nonexistent');
console.log(notFound); // null

// querySelectorAll returns NodeList (not live)
const items = document.querySelectorAll('.item');
// items is static, doesn't update if DOM changes

// innerHTML XSS risk
const userInput = '<img src=x onerror=alert(1)>';
div.innerHTML = userInput; // XSS!
// Use textContent or sanitize input

// Event delegation (efficient)
document.querySelector('ul').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log('Clicked:', e.target.textContent);
  }
});
```

**How to explain in interview:**
"Use `querySelector` for flexible selection, avoid `innerHTML` with user input to prevent XSS, and prefer `textContent` for text-only updates. Batch DOM changes to minimize reflows."

**Common Mistakes:**
- Using `innerHTML` with unsanitized user input
- Querying DOM repeatedly in loops (cache selections)
- Not checking if element exists before manipulation

**Quick Practice:**
1. Create a todo list with add/remove functionality
2. Implement a live search filter for a list

---

### 9. Events

**Question:** How does event handling work in JavaScript?

**Short Answer:** Events are actions (clicks, inputs) that trigger handlers. Use `addEventListener` to attach handlers and understand bubbling, capturing, and delegation.

**Deep Explanation:**
Events propagate in three phases: capturing (top-down), target, bubbling (bottom-up). Event delegation leverages bubbling to handle events on parent elements. `preventDefault()` stops default behavior, `stopPropagation()` stops bubbling.

**Example:**
```javascript
// Basic event listener
const button = document.querySelector('button');
button.addEventListener('click', function(event) {
  console.log('Button clicked!');
  console.log('Target:', event.target);
  console.log('Current target:', event.currentTarget);
});

// Remove listener (must use named function)
function handleClick(e) {
  console.log('Clicked');
}
button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick);

// Event object properties
button.addEventListener('click', (e) => {
  console.log(e.type);        // 'click'
  console.log(e.target);      // Element clicked
  console.log(e.currentTarget); // Element with listener
  console.log(e.clientX, e.clientY); // Mouse position
});

// Prevent default
const link = document.querySelector('a');
link.addEventListener('click', (e) => {
  e.preventDefault(); // Don't navigate
  console.log('Link clicked but navigation prevented');
});

// Stop propagation
const parent = document.querySelector('.parent');
const child = document.querySelector('.child');

parent.addEventListener('click', () => {
  console.log('Parent clicked');
});

child.addEventListener('click', (e) => {
  e.stopPropagation(); // Won't trigger parent handler
  console.log('Child clicked');
});

// Event delegation (efficient for many items)
document.querySelector('ul').addEventListener('click', (e) => {
  if (e.target.matches('li')) {
    console.log('List item clicked:', e.target.textContent);
  }
});

// Capturing phase (rare use case)
element.addEventListener('click', handler, true); // true = capture

// Common events
input.addEventListener('input', (e) => console.log(e.target.value));
form.addEventListener('submit', (e) => {
  e.preventDefault();
  // Handle form submission
});
window.addEventListener('load', () => console.log('Page loaded'));
document.addEventListener('DOMContentLoaded', () => console.log('DOM ready'));
```

**Complexity/Impact:**
- Event attachment: O(1)
- Event bubbling depth: O(d) where d is DOM depth
- Too many listeners cause memory leaks

**Edge Cases & Tests:**
```javascript
// Once option (listener fires once then removes itself)
button.addEventListener('click', handler, { once: true });

// Passive listeners (improve scroll performance)
element.addEventListener('touchstart', handler, { passive: true });

// Custom events
const customEvent = new CustomEvent('myEvent', {
  detail: { message: 'Hello' }
});
element.addEventListener('myEvent', (e) => {
  console.log(e.detail.message);
});
element.dispatchEvent(customEvent);

// Event delegation with dynamic content
// Works even for elements added later!
list.addEventListener('click', (e) => {
  if (e.target.classList.contains('delete-btn')) {
    e.target.closest('li').remove();
  }
});
```

**How to explain in interview:**
"Events bubble from target to root by default. Use event delegation to handle events on parent elements for efficiency, especially with dynamic content. Always call `preventDefault()` to stop default actions like form submission."

**Common Mistakes:**
- Not preventing default behavior when needed
- Attaching listeners inside loops instead of using delegation
- Memory leaks from not removing listeners

**Quick Practice:**
1. Implement event delegation for a dynamic list
2. Create a form with validation on submit

---

## Do you want to go deeper on Basics, practice coding now, or move to Intermediate level?

---

## INTERMEDIATE

### 10. Closures

**Question:** What is a closure and how does it work?

**Short Answer:** A closure is a function that remembers variables from its outer scope even after the outer function has returned.

**Deep Explanation:**
Closures occur when an inner function accesses variables from an outer function's scope. The inner function maintains a reference to the outer scope (lexical environment) even after the outer function executes. This enables data privacy, function factories, and partial application patterns.

**Example:**
```javascript
// Basic closure
function outer() {
  const message = "Hello";

  function inner() {
    console.log(message); // Accesses outer scope
  }

  return inner;
}

const greet = outer();
greet(); // "Hello" - closure retained message

// Practical use: Private variables
function createCounter() {
  let count = 0; // Private variable

  return {
    increment() {
      return ++count;
    },
    decrement() {
      return --count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// console.log(counter.count); // undefined (private)

// Function factory
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
const triple = multiplier(3);
console.log(double(5));  // 10
console.log(triple(5));  // 15

❌ var → function scope

var block ko ignore karta hai, sirf function ko scope maanta hai.

✔️ let → block scope

let har block { } ke andar apna separate variable banata hai.

Full Concept Summary (20 sec me bol sakte ho)
var (function scope)

Block { } ko nahi maanta

Loop me ek hi i hota hai

setTimeout closure me final i (3) capture hoti hai

let (block scope)

Har iteration ek naya block create karta hai

Har iteration me naya j

setTimeout closure correct j ko capture karta hai


// Closure in loops (classic problem)
// WRONG
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // 3, 3, 3
}

// CORRECT - using let
for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 100); // 0, 1, 2
}

Bonus: Agar var ko hi use karna ho?

Tab IIFE use karte hain:

for (var i = 0; i < 3; i++) {
  (function(x){
    setTimeout(() => console.log(x), 100);
  })(i);
}


✔ Output: 0 1 2

Because x ek copy ban gaya.

// Partial application and Currying Kya Hai?

Currying matlab:

Ek function jo multiple arguments leta tha, usko hum is tarah tod dete hain ki wo ek-ek argument lekar chain banata hai.
Currying ek technique hai jisme ek function multiple arguments ki jagah ek argument per function return karta hai.
Ye code ko reusable, modular, clean aur functional banata hai.

function add(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

console.log(add(1)(2)(3)); // 6
const add1 = add(1);
const add1And2 = add1(2);
console.log(add1And2(3)); // 6
```

**Complexity/Impact:**
- Closures have minimal overhead but retain memory
- Can cause memory leaks if not managed properly

**Edge Cases & Tests:**
```javascript
// Closure memory retention
function heavyObject() {
  const bigData = new Array(1000000).fill('data');

  return function() {
    // If bigData isn't used here, modern engines may optimize it away
    return bigData.length;
  };
}

// Module pattern
const module = (function() {
  let private = "secret";

  return {
    getPrivate() {
      return private;
    },
    setPrivate(value) {
      private = value;
    }
  };
})();

console.log(module.getPrivate()); // "secret"
// console.log(module.private); // undefined

// Closure with setTimeout
function delayLog(message, delay) {
  setTimeout(() => {
    console.log(message); // Closure over message
  }, delay);
}
delayLog("Hello", 1000);
```

**How to explain in interview:**
"Closures allow functions to access variables from their outer scope even after the outer function has returned. This enables data privacy and function factories. The key is understanding lexical scoping—where a function is defined, not where it's called."

**Common Mistakes:**
- Not understanding `var` in loops creates a single binding
- Creating memory leaks by retaining large objects unnecessarily
- Confusing closure scope with `this` binding

**Quick Practice:**
1. Implement a counter with private state using closure
2. Fix the classic loop closure bug
3. Create a function factory for mathematical operations

---

### 11. Prototypes & Inheritance

**Question:** Explain JavaScript's prototypal inheritance.

**Short Answer:** Every object has a prototype (internal `[[Prototype]]`). When accessing a property, JS looks up the prototype chain until found or reaching `null`.

**Deep Explanation:**
JavaScript uses prototypal inheritance instead of classical classes (pre-ES6). Objects inherit from other objects. When a property isn't found on an object, JavaScript looks at its prototype, then the prototype's prototype, and so on. ES6 classes are syntactic sugar over prototypes.

**Example:**
```javascript
// Constructor function (pre-ES6)
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const john = new Person("John", 30);
console.log(john.greet()); // "Hello, I'm John"

// Prototype chain
console.log(john.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null

// ES6 Class (syntactic sugar)
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }

  speak() {
    return `${this.name} barks`;
  }

  fetch() {
    return `${this.name} fetches the ball`;
  }
}

const dog = new Dog("Buddy", "Golden Retriever");
console.log(dog.speak());  // "Buddy barks"
console.log(dog.fetch());  // "Buddy fetches the ball"

// Check inheritance
console.log(dog instanceof Dog);    // true
console.log(dog instanceof Animal); // true
console.log(dog instanceof Object); // true

// Object.create (manual prototype setup)
const personProto = {
  greet() {
    return `Hi, I'm ${this.name}`;
  }
};

const jane = Object.create(personProto);
jane.name = "Jane";
console.log(jane.greet()); // "Hi, I'm Jane"

// Property lookup
const obj = { a: 1 };
console.log(obj.a);          // 1 (own property)
console.log(obj.toString()); // [object Object] (from Object.prototype)
console.log(obj.nonExistent); // undefined (not found in chain)

// Own property check
console.log(obj.hasOwnProperty('a'));         // true
console.log(obj.hasOwnProperty('toString'));  // false (inherited)
```

**Complexity/Impact:**
- Property lookup: O(d) where d is prototype chain depth (usually shallow)
- Method sharing via prototype saves memory

**Edge Cases & Tests:**
```javascript
// Shadowing
class Parent {
  constructor() {
    this.name = "Parent";
  }

  getName() {
    return this.name;
  }
}

class Child extends Parent {
  constructor() {
    super();
    this.name = "Child"; // Shadows parent property
  }
}

const child = new Child();
console.log(child.getName()); // "Child"

// Static methods
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtils.add(2, 3)); // 5
// const util = new MathUtils();
// util.add(2, 3); // TypeError (static methods not on instances)

// Private fields (ES2022)
class BankAccount {
  #balance = 0; // Private field

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
// console.log(account.#balance); // SyntaxError

// Getters and setters
class Temperature {
  constructor(celsius) {
    this._celsius = celsius;
  }

  get fahrenheit() {
    return this._celsius * 9/5 + 32;
  }

  set fahrenheit(f) {
    this._celsius = (f - 32) * 5/9;
  }
}

const temp = new Temperature(0);
console.log(temp.fahrenheit); // 32
temp.fahrenheit = 212;
console.log(temp._celsius);   // 100
```

**How to explain in interview:**
"JavaScript uses prototypal inheritance where objects inherit from other objects. ES6 classes are syntactic sugar over prototypes. When you access a property, JavaScript searches the prototype chain. Methods on the prototype are shared across instances for memory efficiency."

**Common Mistakes:**
- Modifying built-in prototypes (never do this)
- Not calling `super()` in derived class constructors
- Confusing static methods with instance methods

**Quick Practice:**
1. Implement inheritance with constructor functions
2. Create a class hierarchy with method overriding
3. Explain the prototype chain for a given object

---

### 12. `this` Keyword

**Question:** How does the `this` keyword work in JavaScript?

**Short Answer:** `this` refers to the execution context. It depends on how the function is called: method call, standalone, constructor, arrow function, or explicit binding.

**Deep Explanation:**
`this` is determined at runtime based on the call-site:
1. **Method call:** `obj.method()` → `this` is `obj`
2. **Standalone function:** `func()` → `this` is `undefined` (strict mode) or `window` (non-strict)
3. **Constructor:** `new Func()` → `this` is new object
4. **Arrow function:** Lexical `this` from enclosing scope
5. **Explicit binding:** `call`, `apply`, `bind`

**Example:**
```javascript
// Method call
const person = {
  name: "Alice",
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

person.greet(); // "Hello, I'm Alice" (this = person)

// Standalone function
function showThis() {
  "use strict";
  console.log(this); // undefined (strict mode)
}
showThis();

// Lost context
const greet = person.greet;
// greet(); // TypeError: Cannot read property 'name' of undefined

// Solution 1: Bind
const boundGreet = person.greet.bind(person);
boundGreet(); // "Hello, I'm Alice"

// Constructor function
function Car(make) {
  this.make = make;
}

const car = new Car("Toyota");
console.log(car.make); // "Toyota"

// Arrow function (lexical this)
const obj = {
  name: "Object",
  regularFunc: function() {
    console.log(this.name); // "Object"

    // Inner regular function loses context
    setTimeout(function() {
      console.log(this.name); // undefined
    }, 100);

    // Arrow function preserves context
    setTimeout(() => {
      console.log(this.name); // "Object"
    }, 100);
  }
};

obj.regularFunc();

// Explicit binding
function introduce(greeting, punctuation) {
  console.log(`${greeting}, I'm ${this.name}${punctuation}`);
}

const user = { name: "Bob" };

// call (arguments individually)
introduce.call(user, "Hi", "!"); // "Hi, I'm Bob!"

// apply (arguments as array)
introduce.apply(user, ["Hello", "."]); // "Hello, I'm Bob."

// bind (returns new function)
const boundIntroduce = introduce.bind(user, "Hey");
boundIntroduce("..."); // "Hey, I'm Bob..."

// Class methods
class Counter {
  constructor() {
    this.count = 0;

    // Bind in constructor for event handlers
    this.increment = this.increment.bind(this);
  }

  increment() {
    this.count++;
    console.log(this.count);
  }

  // Arrow function as class field (always bound)
  decrement = () => {
    this.count--;
    console.log(this.count);
  }
}

const counter = new Counter();
const incrementFunc = counter.increment;
incrementFunc(); // Works (bound in constructor)

const decrementFunc = counter.decrement;
decrementFunc(); // Works (arrow function)
```

**Complexity/Impact:** `this` binding is a common source of bugs, especially in callbacks and event handlers.

**Edge Cases & Tests:**
```javascript
// Global context
console.log(this); // window (browser, non-strict)

// Arrow functions don't have their own this
const arrowObj = {
  name: "Arrow",
  arrowMethod: () => {
    console.log(this.name); // undefined (this from outer scope)
  }
};

// Nested objects
const nested = {
  name: "Outer",
  inner: {
    name: "Inner",
    getName() {
      return this.name; // "Inner" (this = inner object)
    }
  }
};

console.log(nested.inner.getName()); // "Inner"

// this in array methods
const arr = [1, 2, 3];
arr.forEach(function(item) {
  console.log(this); // undefined (no context provided)
});

arr.forEach(function(item) {
  console.log(this.multiplier); // 10
}, { multiplier: 10 }); // thisArg parameter

// DOM event handlers
button.addEventListener('click', function() {
  console.log(this); // button element
});

button.addEventListener('click', () => {
  console.log(this); // lexical this (probably window)
});
```

**How to explain in interview:**
"`this` is determined by how a function is called, not where it's defined. Arrow functions don't have their own `this`—they inherit from the enclosing scope. For methods used as callbacks, use arrow functions or bind to preserve context."

**Common Mistakes:**
- Using arrow functions as methods expecting `this` binding
- Forgetting to bind methods when passing to callbacks
- Not understanding `this` in nested functions

**Quick Practice:**
1. Fix lost `this` context in a callback
2. Demonstrate difference between call, apply, and bind
3. Explain why arrow function methods don't work with `this`

---

### 13. call, apply, bind

**Question:** What's the difference between call, apply, and bind?

**Short Answer:** All three set `this` explicitly. `call` and `apply` invoke immediately (differ in argument passing), `bind` returns a new function.

**Deep Explanation:**
- `call(thisArg, arg1, arg2, ...)`: Invoke with arguments individually
- `apply(thisArg, [args])`: Invoke with arguments as array
- `bind(thisArg, arg1, ...)`: Return new function with bound `this` (partial application)

**Example:**
```javascript
function introduce(greeting, punctuation) {
  return `${greeting}, I'm ${this.name}${punctuation}`;
}

const person = { name: "Alice" };

// call - arguments individually
console.log(introduce.call(person, "Hello", "!"));
// "Hello, I'm Alice!"

// apply - arguments as array
console.log(introduce.apply(person, ["Hi", "."]));
// "Hi, I'm Alice."

// bind - returns new function
const boundIntroduce = introduce.bind(person);
console.log(boundIntroduce("Hey", "?"));
// "Hey, I'm Alice?"

// Partial application with bind
const greetAlice = introduce.bind(person, "Greetings");
console.log(greetAlice("!!!"));
// "Greetings, I'm Alice!!!"

// Borrowing methods
const numbers = { values: [1, 2, 3, 4, 5] };

// Array methods don't exist on plain objects
// Use call/apply to borrow from Array prototype
const sum = Array.prototype.reduce.call(
  numbers.values,
  (acc, n) => acc + n,
  0
);
console.log(sum); // 15

// Common use case: finding max in array
const nums = [5, 1, 9, 3, 7];
console.log(Math.max.apply(null, nums)); // 9
// ES6 alternative: Math.max(...nums)

// Function currying with bind
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);
console.log(double(5));  // 10
console.log(double(10)); // 20

const triple = multiply.bind(null, 3);
console.log(triple(5));  // 15

// Preserving context in callbacks
class Component {
  constructor(name) {
    this.name = name;
  }

  handleClick() {
    console.log(`${this.name} clicked`);
  }

  render() {
    // Without bind, handleClick loses context
    const button = document.createElement('button');

    // Solution 1: bind
    button.onclick = this.handleClick.bind(this);

    // Solution 2: arrow function
    button.onclick = () => this.handleClick();
  }
}

// Polyfill for bind (interview classic)
Function.prototype.myBind = function(context, ...args) {
  const fn = this;
  return function(...newArgs) {
    return fn.apply(context, [...args, ...newArgs]);
  };
};

const boundFn = introduce.myBind(person, "Custom bind");
console.log(boundFn("!!")); // "Custom bind, I'm Alice!!"
```

**Complexity/Impact:** O(1) for call/apply/bind operations. Bind creates a new function (minimal memory overhead).

**Edge Cases & Tests:**
```javascript
// call/apply with primitives
function showType() {
  console.log(typeof this);
}

showType.call(123);    // "object" (primitive wrapped)
showType.call("test"); // "object"

// null/undefined as thisArg
function showThis() {
  console.log(this);
}

showThis.call(null);      // window (non-strict) / null (strict)
showThis.call(undefined); // window (non-strict) / undefined (strict)

// Bind is immutable
const obj1 = { name: "Object 1" };
const obj2 = { name: "Object 2" };

function getName() {
  return this.name;
}

const bound = getName.bind(obj1);
console.log(bound()); // "Object 1"

// Trying to rebind doesn't work
const rebound = bound.bind(obj2);
console.log(rebound()); // "Object 1" (still bound to obj1)

// Arrow functions ignore bind/call/apply
const arrow = () => console.log(this.name);
const boundArrow = arrow.bind({ name: "Test" });
// boundArrow(); // Ignores bind, uses lexical this

// Multiple bind calls
function add(a, b, c) {
  return a + b + c;
}

const add5 = add.bind(null, 5);
const add5And10 = add5.bind(null, 10);
console.log(add5And10(15)); // 30 (5 + 10 + 15)
```

**How to explain in interview:**
"`call` and `apply` immediately invoke a function with specified `this`, differing only in how arguments are passed. `bind` returns a new function with fixed `this` and optional pre-filled arguments, useful for event handlers and partial application."

**Common Mistakes:**
- Using `apply` when `call` would be clearer (or vice versa)
- Thinking you can rebind an already-bound function
- Not understanding arrow functions ignore explicit binding

**Quick Practice:**
1. Implement custom `bind` polyfill
2. Use `call` to borrow array methods
3. Create a curried function using `bind`

---

### 14. Promises

**Question:** What are Promises and how do they work?

**Short Answer:** Promises represent eventual completion (or failure) of an async operation. They have three states: pending, fulfilled, rejected.

**Deep Explanation:**
Promises solve callback hell by providing a chainable API. A Promise is an object with `then`, `catch`, and `finally` methods. They enable async operations to be handled in a more synchronous-looking way. Promises are eager (execute immediately upon creation).

**Example:**
```javascript
// Creating a Promise
const promise = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve("Operation successful!"); // Fulfilled
    } else {
      reject(new Error("Operation failed")); // Rejected
    }
  }, 1000);
});

// Consuming a Promise
promise
  .then(result => {
    console.log(result); // "Operation successful!"
    return result.toUpperCase();
  })
  .then(uppercased => {
    console.log(uppercased); // "OPERATION SUCCESSFUL!"
  })
  .catch(error => {
    console.error(error);
  })
  .finally(() => {
    console.log("Cleanup"); // Always runs
  });

// Promise chaining
function fetchUser(id) {
  return new Promise(resolve => {
    setTimeout(() => resolve({ id, name: "User " + id }), 500);
  });
}

function fetchPosts(userId) {
  return new Promise(resolve => {
    setTimeout(() => resolve([
      { id: 1, title: "Post 1" },
      { id: 2, title: "Post 2" }
    ]), 500);
  });
}

fetchUser(1)
  .then(user => {
    console.log("User:", user);
    return fetchPosts(user.id);
  })
  .then(posts => {
    console.log("Posts:", posts);
  })
  .catch(error => {
    console.error("Error:", error);
  });

// Promise.all (parallel, all must succeed)
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.resolve(3);

Promise.all([p1, p2, p3])
  .then(results => {
    console.log(results); // [1, 2, 3]
  });

// If any rejects, Promise.all rejects
Promise.all([
  Promise.resolve(1),
  Promise.reject("Error"),
  Promise.resolve(3)
])
  .catch(error => {
    console.log(error); // "Error"
  });

// Promise.race (first to settle wins)
Promise.race([
  new Promise(resolve => setTimeout(() => resolve("fast"), 100)),
  new Promise(resolve => setTimeout(() => resolve("slow"), 500))
])
  .then(result => {
    console.log(result); // "fast"
  });

// Promise.allSettled (wait for all, regardless of outcome)
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("Error"),
  Promise.resolve(3)
])
  .then(results => {
    console.log(results);
    // [
    //   { status: "fulfilled", value: 1 },
    //   { status: "rejected", reason: "Error" },
    //   { status: "fulfilled", value: 3 }
    // ]
  });

// Promise.any (first fulfilled, ignore rejections)
Promise.any([
  Promise.reject("Error 1"),
  Promise.resolve("Success"),
  Promise.reject("Error 2")
])
  .then(result => {
    console.log(result); // "Success"
  });

// Converting callback to Promise
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

delay(1000).then(() => console.log("1 second passed"));

// Promise error handling
new Promise((resolve, reject) => {
  throw new Error("Synchronous error");
})
  .catch(error => {
    console.error("Caught:", error.message);
  });

// Returning promises in then
Promise.resolve(1)
  .then(value => {
    return Promise.resolve(value * 2); // Can return promise
  })
  .then(value => {
    console.log(value); // 2
  });
```

**Complexity/Impact:**
- Promise creation: O(1)
- Chaining: O(1) per then/catch
- Promise.all/race: O(n) where n is number of promises

**Edge Cases & Tests:**
```javascript
// Already settled promises
const resolved = Promise.resolve(42);
resolved.then(x => console.log(x)); // 42 (executes immediately)

// Thenable (promise-like object)
const thenable = {
  then(resolve) {
    resolve(100);
  }
};

Promise.resolve(thenable).then(x => console.log(x)); // 100

// Catching in the middle of chain
Promise.resolve(1)
  .then(x => {
    throw new Error("Oops");
  })
  .catch(error => {
    console.log("Caught:", error.message);
    return 99; // Recover from error
  })
  .then(x => {
    console.log("Recovered:", x); // 99
  });

// Forgotten catch (unhandled rejection)
Promise.reject("Unhandled")
  .then(x => console.log(x));
// UnhandledPromiseRejectionWarning

// Proper error handling
Promise.reject("Handled")
  .catch(error => console.error(error));

// Promise constructor runs immediately
console.log("Before");
new Promise(resolve => {
  console.log("Inside promise");
  resolve();
});
console.log("After");
// Output: Before, Inside promise, After

// Microtask queue
console.log("1");
Promise.resolve().then(() => console.log("2"));
console.log("3");
// Output: 1, 3, 2 (promises run in microtask queue)
```

**How to explain in interview:**
"Promises provide a cleaner way to handle async operations than callbacks. They represent a value that will be available in the future. Use `Promise.all` for parallel operations that all must succeed, `allSettled` when you need all results regardless, and `race` for timeout patterns."

**Common Mistakes:**
- Forgetting to return promises in `then` chains
- Not adding `catch` for error handling
- Creating promise inside a loop instead of collecting and using `Promise.all`

**Quick Practice:**
1. Convert callback-based function to Promise
2. Implement Promise.all from scratch
3. Handle errors in a promise chain

---

### 15. async/await

**Question:** How does async/await work?

**Short Answer:** `async/await` is syntactic sugar over Promises, making async code look synchronous. `async` functions return promises; `await` pauses execution until promise resolves.

**Deep Explanation:**
`async` functions always return a Promise. `await` can only be used inside `async` functions (or top-level in modules). It pauses function execution until the promise settles, then resumes with the resolved value. Errors can be caught with try/catch.

**Example:**
```javascript
// Basic async/await
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  return data;
}

fetchData().then(data => console.log(data));

// Error handling with try/catch
async function fetchWithError() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Fetch error:", error);
    throw error; // Re-throw or handle
  }
}

// Sequential vs parallel
async function sequential() {
  const user = await fetchUser(1);    // Wait
  const posts = await fetchPosts(1);  // Then wait
  return { user, posts };
}

async function parallel() {
  const [user, posts] = await Promise.all([
    fetchUser(1),
    fetchPosts(1)
  ]); // Both run concurrently
  return { user, posts };
}

// Comparing Promise chain vs async/await
// Promise chain
function getDataPromise() {
  return fetchUser(1)
    .then(user => {
      return fetchPosts(user.id)
        .then(posts => {
          return { user, posts };
        });
    })
    .catch(error => {
      console.error(error);
    });
}

// async/await (cleaner!)
async function getDataAsync() {
  try {
    const user = await fetchUser(1);
    const posts = await fetchPosts(user.id);
    return { user, posts };
  } catch (error) {
    console.error(error);
  }
}

// Async function returns Promise
async function getValue() {
  return 42; // Automatically wrapped in Promise.resolve(42)
}

getValue().then(x => console.log(x)); // 42

// Equivalent to:
function getValuePromise() {
  return Promise.resolve(42);
}

// await with non-Promise values
async function test() {
  const x = await 42; // Works, immediately resolves
  console.log(x); // 42
}

// Looping with await
async function processItems(items) {
  // Sequential processing
  for (const item of items) {
    await processItem(item); // Waits for each
  }

  // Parallel processing
  await Promise.all(items.map(item => processItem(item)));
}

// Top-level await (ES2022, in modules)
// const data = await fetchData(); // Works at module top level

// Async IIFE (before top-level await)
(async () => {
  const data = await fetchData();
  console.log(data);
})();

// Multiple awaits
async function multipleOperations() {
  const a = await operation1();
  const b = await operation2(a);
  const c = await operation3(b);
  return c;
}

// Concurrent operations
async function concurrent() {
  const promise1 = operation1(); // Start immediately
  const promise2 = operation2(); // Start immediately

  const result1 = await promise1; // Wait for first
  const result2 = await promise2; // Wait for second

  return [result1, result2];
}

// Error handling patterns
async function handleErrors() {
  try {
    const data = await riskyOperation();
    return data;
  } catch (error) {
    if (error instanceof NetworkError) {
      // Handle network errors
      return defaultData;
    }
    throw error; // Re-throw other errors
  } finally {
    cleanup(); // Always runs
  }
}
```

**Complexity/Impact:**
- Same as Promises, just cleaner syntax
- Can make code easier to reason about

**Edge Cases & Tests:**
```javascript
// Forgetting await
async function forgottenAwait() {
  const promise = fetchData(); // Returns promise, not data!
  console.log(promise); // Promise object

  const data = await fetchData(); // Correct
  console.log(data); // Actual data
}

// Await in loops (sequential)
async function sequentialLoop(items) {
  for (const item of items) {
    await delay(100); // Each waits 100ms
  }
  // Total time: items.length * 100ms
}

// Better: parallel
async function parallelLoop(items) {
  await Promise.all(items.map(item => delay(100)));
  // Total time: ~100ms
}

// Return await vs return
async function returnAwait() {
  return await fetchData(); // Waits for promise
}

async function returnDirect() {
  return fetchData(); // Returns promise directly (faster)
}

// Use "return await" only in try/catch
async function needsReturnAwait() {
  try {
    return await riskyOperation(); // Correct
  } catch (error) {
    console.error(error);
  }
}

async function wrongReturnAwait() {
  try {
    return riskyOperation(); // Bug! Promise escapes try/catch
  } catch (error) {
    console.error(error); // Won't catch error
  }
}

// Async arrow functions
const asyncArrow = async () => {
  return await fetchData();
};

const asyncArrowConcise = async () => await fetchData();

// Async methods
class DataService {
  async fetchData() {
    return await fetch('/api/data');
  }
}

// Mixing callbacks and async/await
async function mixedApproach() {
  // Don't do this:
  setTimeout(async () => {
    await doSomething();
  }, 1000);

  // Better:
  await delay(1000);
  await doSomething();
}
```

**How to explain in interview:**
"`async/await` makes asynchronous code read like synchronous code. It's syntactic sugar over Promises. Key benefits are cleaner error handling with try/catch and avoiding callback/promise chains. Always remember to `await` Promise operations and use `Promise.all` for parallel execution."

**Common Mistakes:**
- Forgetting `await` keyword
- Using `await` in loops instead of `Promise.all` for parallel ops
- Not handling errors with try/catch
- Using `return await` unnecessarily outside try/catch

**Quick Practice:**
1. Convert promise chain to async/await
2. Fetch multiple resources in parallel
3. Add proper error handling to async function

---

### 16. Event Loop

**Question:** How does the JavaScript event loop work?

**Short Answer:** The event loop manages async operations. Call stack executes code, Web APIs handle async tasks, callback/microtask queues hold pending operations. Event loop prioritizes microtasks over macrotasks.

**Deep Explanation:**
JavaScript is single-threaded. The event loop coordinates:
1. **Call Stack:** Executes synchronous code (LIFO)
2. **Web APIs:** Browser features (setTimeout, fetch, DOM events)
3. **Callback Queue (Macrotask):** setTimeout, setInterval, I/O callbacks
4. **Microtask Queue:** Promises, queueMicrotask, MutationObserver

Loop checks: Execute all microtasks → Execute one macrotask → Repeat

**Example:**
```javascript
// Event loop demonstration
console.log("1"); // Sync

setTimeout(() => {
  console.log("2"); // Macrotask
}, 0);

Promise.resolve().then(() => {
  console.log("3"); // Microtask
});

console.log("4"); // Sync

// Output: 1, 4, 3, 2
// Explanation:
// - Sync code runs first: 1, 4
// - Microtasks run before macrotasks: 3
// - Then macrotasks: 2

// Complex example
console.log("Start");

setTimeout(() => {
  console.log("Timeout 1");
  Promise.resolve().then(() => console.log("Promise in Timeout 1"));
}, 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
  setTimeout(() => console.log("Timeout in Promise 1"), 0);
});

Promise.resolve().then(() => {
  console.log("Promise 2");
});

setTimeout(() => {
  console.log("Timeout 2");
}, 0);

console.log("End");

// Output:
// Start
// End
// Promise 1
// Promise 2
// Timeout 1
// Promise in Timeout 1
// Timeout in Promise 1
// Timeout 2

// Microtask vs Macrotask
console.log("1");

// Macrotask (setTimeout)
setTimeout(() => console.log("2"), 0);

// Microtask (Promise)
Promise.resolve()
  .then(() => console.log("3"))
  .then(() => console.log("4"));

// Macrotask (setTimeout)
setTimeout(() => console.log("5"), 0);

// Microtask (queueMicrotask)
queueMicrotask(() => console.log("6"));

console.log("7");

// Output: 1, 7, 3, 6, 4, 2, 5
// Sync: 1, 7
// All microtasks: 3, 6, 4
// Macrotasks one by one: 2, 5

// Stack overflow example
function recursiveCall() {
  recursiveCall(); // Synchronous, fills call stack
}
// recursiveCall(); // RangeError: Maximum call stack size exceeded

// Non-blocking async recursion
function asyncRecursive(n) {
  if (n === 0) return;
  setTimeout(() => asyncRecursive(n - 1), 0); // Macrotask, won't overflow
}
asyncRecursive(10000); // Works!

// Blocking the event loop (bad)
function blockEventLoop() {
  const start = Date.now();
  while (Date.now() - start < 3000) {
    // Blocks for 3 seconds
  }
  console.log("Finally!");
}

// Non-blocking alternative
async function nonBlocking() {
  await new Promise(resolve => setTimeout(resolve, 3000));
  console.log("Finally!");
}

// Starving macrotasks
function starveMacrotasks() {
  setTimeout(() => console.log("I may never run"), 0);

  function addMicrotask() {
    Promise.resolve().then(() => {
      console.log("Microtask");
      addMicrotask(); // Keeps adding microtasks
    });
  }

  addMicrotask(); // Infinite microtasks prevent macrotask
}
```

**Complexity/Impact:**
- Understanding event loop prevents blocking UI
- Microtask priority can cause starvation

**Edge Cases & Tests:**
```javascript
// Process.nextTick (Node.js only, higher priority than microtasks)
console.log("1");
process.nextTick(() => console.log("2"));
Promise.resolve().then(() => console.log("3"));
console.log("4");
// Output: 1, 4, 2, 3

// SetImmediate vs setTimeout (Node.js)
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
// Order depends on when event loop started

// Rendering and event loop
// Browser renders after microtasks but before next macrotask
button.addEventListener('click', () => {
  // Microtasks complete before paint
  Promise.resolve().then(() => {
    element.style.background = 'red';
  });

  // Macrotasks may allow paint between
  setTimeout(() => {
    element.style.background = 'blue';
  }, 0);
});

// RequestAnimationFrame (runs before paint)
requestAnimationFrame(() => {
  console.log("RAF");
});

setTimeout(() => console.log("Timeout"), 0);
Promise.resolve().then(() => console.log("Promise"));

// Output: Promise, RAF, Timeout
```

**How to explain in interview:**
"JavaScript is single-threaded with an event loop that manages async operations. Synchronous code runs first, then all microtasks (promises), then one macrotask (setTimeout), then repeat. This is why promises resolve before setTimeout even with 0 delay."

**Common Mistakes:**
- Thinking setTimeout(fn, 0) runs immediately
- Blocking event loop with long synchronous operations
- Not understanding microtask priority over macrotasks

**Quick Practice:**
1. Predict output of mixed sync/async code
2. Explain why setTimeout(..., 0) doesn't guarantee order
3. Create infinite microtask queue (demonstrate starvation)

---

## Do you want to go deeper on Intermediate topics, practice coding now, or move to Advanced level?

---

## ADVANCED

### 17. Performance & Memory Leaks

**Question:** How do you optimize JavaScript performance and prevent memory leaks?

**Short Answer:** Avoid memory leaks by cleaning up listeners, clearing timers, and managing closures. Optimize with debouncing, throttling, lazy loading, and minimizing DOM operations.

**Deep Explanation:**
Memory leaks occur when references prevent garbage collection. Common causes: forgotten event listeners, circular references, closures retaining large objects, and global variables. Performance optimization involves reducing unnecessary work, batching operations, and using efficient algorithms.

**Example:**
```javascript
// MEMORY LEAK: Forgotten event listener
function createLeak() {
  const button = document.querySelector('button');
  const hugeArray = new Array(1000000).fill('data');

  button.addEventListener('click', () => {
    console.log(hugeArray.length); // Closure retains hugeArray
  });

  // If button is removed from DOM, listener still exists
  // Solution: Remove listener
  // button.removeEventListener('click', handler);
}

// MEMORY LEAK: Timer not cleared
function leakyTimer() {
  let count = 0;
  setInterval(() => {
    count++;
    console.log(count);
  }, 1000);

  // Solution: Store timer ID and clear it
  // const timerId = setInterval(...);
  // clearInterval(timerId);
}

// MEMORY LEAK: Closure retaining large object
function closureLeak() {
  const bigData = new Array(1000000).fill('x');

  return {
    // If bigData isn't used, modern engines optimize
    // But if it's used anywhere, entire array retained
    getData: () => bigData[0]
  };
}

// Solution: Nullify when done
function fixedClosure() {
  let bigData = new Array(1000000).fill('x');
  const result = processData(bigData);
  bigData = null; // Allow GC
  return result;
}

// PERFORMANCE: Debouncing (delay execution until pause)
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

const searchInput = document.querySelector('#search');
const debouncedSearch = debounce((query) => {
  console.log('Searching for:', query);
  // API call here
}, 300);

searchInput.addEventListener('input', (e) => {
  debouncedSearch(e.target.value);
});

// PERFORMANCE: Throttling (limit execution rate)
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

const throttledScroll = throttle(() => {
  console.log('Scroll event');
}, 1000);

window.addEventListener('scroll', throttledScroll);

// PERFORMANCE: Batch DOM updates
// Bad: Multiple reflows
function badDOMUpdate() {
  for (let i = 0; i < 100; i++) {
    const div = document.createElement('div');
    div.textContent = i;
    document.body.appendChild(div); // Reflow each time
  }
}

// Good: Single reflow
function goodDOMUpdate() {
  const fragment = document.createDocumentFragment();
  for (let i = 0; i < 100; i++) {
    const div = document.createElement('div');
    div.textContent = i;
    fragment.appendChild(div);
  }
  document.body.appendChild(fragment); // Single reflow
}

// PERFORMANCE: Lazy loading
function lazyLoad(selector) {
  const images = document.querySelectorAll(selector);

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const img = entry.target;
        img.src = img.dataset.src;
        observer.unobserve(img);
      }
    });
  });

  images.forEach(img => observer.observe(img));
}

// PERFORMANCE: Memoization
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const expensiveCalculation = memoize((n) => {
  console.log('Computing...');
  let sum = 0;
  for (let i = 0; i < n; i++) {
    sum += i;
  }
  return sum;
});

console.log(expensiveCalculation(1000000)); // Computing... (slow)
console.log(expensiveCalculation(1000000)); // Instant (cached)

// PERFORMANCE: Web Workers for heavy computation
// main.js
const worker = new Worker('worker.js');
worker.postMessage({ data: largeDataset });
worker.onmessage = (e) => {
  console.log('Result:', e.data);
};

// worker.js
self.onmessage = (e) => {
  const result = heavyComputation(e.data);
  self.postMessage(result);
};
```

**Complexity/Impact:**
- Memory leaks grow over time, causing crashes
- DOM operations are expensive (reflow/repaint)
- Debouncing/throttling reduce function calls significantly

**Edge Cases & Tests:**
```javascript
// Detecting memory leaks
// Use Chrome DevTools Memory Profiler
// Take heap snapshots and compare

// WeakMap prevents memory leaks
const cache = new WeakMap();

function cacheData(obj, data) {
  cache.set(obj, data);
  // When obj is GC'd, cache entry is automatically removed
}

// VS regular Map (keeps references)
const regularCache = new Map();
function badCache(obj, data) {
  regularCache.set(obj, data);
  // obj is never GC'd even if no other references
}

// Performance testing
console.time('Operation');
expensiveOperation();
console.timeEnd('Operation');

// Performance API
const start = performance.now();
expensiveOperation();
const end = performance.now();
console.log(`Took ${end - start}ms`);

// Memory usage (Chrome only)
console.log(performance.memory.usedJSHeapSize);
```

**How to explain in interview:**
"Memory leaks occur when references prevent garbage collection. Always remove event listeners, clear timers, and nullify large objects when done. For performance, debounce/throttle frequent events, batch DOM updates, and use Web Workers for heavy computation."

**Common Mistakes:**
- Not removing event listeners on cleanup
- Keeping unnecessary references in closures
- Performing many small DOM updates instead of batching

**Quick Practice:**
1. Implement debounce and throttle from scratch
2. Identify and fix memory leak in provided code
3. Optimize a slow DOM manipulation function

---

## Do you want to go deeper on Advanced topics, practice coding now, or move to Coding Problems and Tricky Questions?

---

## CODING PROBLEMS

### Easy Problems

#### Problem 1: Array Sum
**Difficulty:** Easy

**Question:** Write a function that returns the sum of all numbers in an array.

**Approach:** Iterate through array and accumulate sum, or use `reduce`.

**Solution:**
```javascript
// Solution 1: Loop
function arraySum(arr) {
  let sum = 0;
  for (const num of arr) {
    sum += num;
  }
  return sum;
}

// Solution 2: Reduce
function arraySumReduce(arr) {
  return arr.reduce((acc, num) => acc + num, 0);
}

// Tests
console.log(arraySum([1, 2, 3, 4, 5])); // 15
console.log(arraySum([]));              // 0
console.log(arraySum([-1, -2, 3]));     // 0
```

**Complexity:** Time O(n), Space O(1)

**Interviewer Checklist:**
- Handles empty array
- Works with negative numbers
- Initializes accumulator to 0

**Follow-ups:**
- What if array contains non-numbers?
- How would you handle nested arrays?

---

#### Problem 2: Reverse String
**Difficulty:** Easy

**Question:** Write a function to reverse a string.

**Solution:**
```javascript
// Solution 1: Built-in
function reverseString(str) {
  return str.split('').reverse().join('');
}

// Solution 2: Loop
function reverseStringLoop(str) {
  let reversed = '';
  for (let i = str.length - 1; i >= 0; i--) {
    reversed += str[i];
  }
  return reversed;
}

// Solution 3: Reduce
function reverseStringReduce(str) {
  return str.split('').reduce((acc, char) => char + acc, '');
}

// Tests
console.log(reverseString('hello'));  // 'olleh'
console.log(reverseString(''));       // ''
console.log(reverseString('a'));      // 'a'
```

**Complexity:** Time O(n), Space O(n)

---

#### Problem 3: Palindrome Check
**Difficulty:** Easy

**Question:** Check if a string is a palindrome.

**Solution:**
```javascript
function isPalindrome(str) {
  // Clean string: lowercase, remove non-alphanumeric
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  const reversed = cleaned.split('').reverse().join('');
  return cleaned === reversed;
}

// Optimized: Two pointers
function isPalindromeTwoPointer(str) {
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  let left = 0;
  let right = cleaned.length - 1;

  while (left < right) {
    if (cleaned[left] !== cleaned[right]) {
      return false;
    }
    left++;
    right--;
  }

  return true;
}

// Tests
console.log(isPalindrome('racecar'));           // true
console.log(isPalindrome('hello'));             // false
console.log(isPalindrome('A man, a plan, a canal: Panama')); // true
```

**Complexity:**
- First: Time O(n), Space O(n)
- Two-pointer: Time O(n), Space O(n) for cleaned string

---

### Medium Problems

#### Problem 4: Debounce Function
**Difficulty:** Medium

**Question:** Implement a debounce function.

**Solution:**
```javascript
function debounce(func, delay) {
  let timeoutId;

  return function(...args) {
    const context = this;

    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      func.apply(context, args);
    }, delay);
  };
}

// Test
const log = debounce((msg) => console.log(msg), 1000);
log('a'); // Cancelled
log('b'); // Cancelled
log('c'); // After 1s: 'c'
```

**Complexity:** Time O(1) per call, Space O(1)

**Follow-ups:**
- Add immediate execution option
- Add cancel method

---

#### Problem 5: Flatten Array
**Difficulty:** Medium

**Question:** Flatten a nested array to any depth.

**Solution:**
```javascript
// Solution 1: Recursive
function flatten(arr) {
  const result = [];

  for (const item of arr) {
    if (Array.isArray(item)) {
      result.push(...flatten(item));
    } else {
      result.push(item);
    }
  }

  return result;
}

// Solution 2: Stack (iterative)
function flattenIterative(arr) {
  const stack = [...arr];
  const result = [];

  while (stack.length) {
    const item = stack.pop();

    if (Array.isArray(item)) {
      stack.push(...item);
    } else {
      result.unshift(item);
    }
  }

  return result;
}

// Solution 3: Built-in
function flattenBuiltIn(arr) {
  return arr.flat(Infinity);
}

// Tests
const nested = [1, [2, [3, [4]], 5]];
console.log(flatten(nested)); // [1, 2, 3, 4, 5]
console.log(flatten([]));     // []
console.log(flatten([1, 2])); // [1, 2]
```

**Complexity:** Time O(n), Space O(d) for recursion depth

---

### Hard Problems

#### Problem 6: Deep Clone
**Difficulty:** Hard

**Question:** Implement deep clone for objects handling circular references.

**Solution:**
```javascript
function deepClone(obj, hash = new WeakMap()) {
  // Handle primitives
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }

  // Handle circular references
  if (hash.has(obj)) {
    return hash.get(obj);
  }

  // Handle Date
  if (obj instanceof Date) {
    return new Date(obj.getTime());
  }

  // Handle Array
  if (Array.isArray(obj)) {
    const arrCopy = [];
    hash.set(obj, arrCopy);
    obj.forEach((item, index) => {
      arrCopy[index] = deepClone(item, hash);
    });
    return arrCopy;
  }

  // Handle Object
  const objCopy = {};
  hash.set(obj, objCopy);
  Object.keys(obj).forEach(key => {
    objCopy[key] = deepClone(obj[key], hash);
  });

  return objCopy;
}

// Test with circular reference
const obj = { a: 1, b: { c: 2 } };
obj.self = obj;

const cloned = deepClone(obj);
console.log(cloned.a); // 1
console.log(cloned.b.c); // 2
console.log(cloned.self === cloned); // true (circular preserved)
console.log(cloned !== obj); // true (different object)
```

**Complexity:** Time O(n), Space O(n)

---

## TRICKY QUESTIONS & GOTCHAS

### 1. `== ` vs `===` Surprises

**Question:** Explain why `[] == ![]` is true.

**Explanation:**
```javascript
[] == ![]; // true

// Why?
// 1. ![] is evaluated first
![] // false (empty array is truthy, so ![] is false)

// 2. Now we have: [] == false
// 3. [] is converted to primitive (valueOf, then toString)
[].toString() // "" (empty string)

// 4. Now: "" == false
// 5. false coerced to number: 0
// 6. "" coerced to number: 0

// 7. 0 == 0 → true!

// Other weird coercions:
console.log([] == 0);     // true
console.log("" == 0);     // true
console.log(0 == false);  // true
console.log(null == 0);   // false (special case)
console.log(null == undefined); // true (special case)
```

**Interview Answer:** "Loose equality performs type coercion. `[]` becomes empty string, then 0. `![]` is false, which becomes 0. So 0 == 0 is true. Always use `===` to avoid this."

---

### 2. Closure in Loops

**Question:** Why does this print "3, 3, 3" and how to fix?

```javascript
// Problem
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

// Why? var is function-scoped, so there's only ONE i
// All timeouts reference the same i, which is 3 after loop ends

// Solution 1: Use let (block-scoped)
for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 100);
}
// Output: 0, 1, 2

// Solution 2: IIFE to capture i
for (var k = 0; k < 3; k++) {
  (function(k) {
    setTimeout(() => console.log(k), 100);
  })(k);
}
// Output: 0, 1, 2

// Solution 3: Bind
for (var m = 0; m < 3; m++) {
  setTimeout(console.log.bind(null, m), 100);
}
// Output: 0, 1, 2
```

**Interview Answer:** "`var` is function-scoped, so there's one shared `i`. Use `let` for block scope, creating a new binding per iteration."

---

### 3. `this` in Arrow Functions

**Question:** Why doesn't `this` work in arrow functions as methods?

```javascript
const obj = {
  name: "Object",

  regularMethod() {
    console.log(this.name); // "Object"
  },

  arrowMethod: () => {
    console.log(this.name); // undefined (or global)
  }
};

obj.regularMethod(); // "Object"
obj.arrowMethod();   // undefined

// Why? Arrow functions don't have their own `this`
// They inherit from enclosing scope (lexical)

// Common mistake in callbacks:
const obj2 = {
  name: "Object2",

  logAfterDelay() {
    setTimeout(function() {
      console.log(this.name); // undefined (lost context)
    }, 100);
  },

  logAfterDelayFixed() {
    setTimeout(() => {
      console.log(this.name); // "Object2" (lexical this)
    }, 100);
  }
};
```

**Interview Answer:** "Arrow functions inherit `this` from their surrounding scope, not from how they're called. Use regular functions for methods needing dynamic `this`, and arrow functions for callbacks to preserve context."

---

### 4. Event Loop Ordering

**Question:** What's the output order?

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

console.log('4');

// Output: 1, 4, 3, 2

// Why?
// 1. Synchronous code: 1, 4
// 2. Microtasks (promises): 3
// 3. Macrotasks (setTimeout): 2
```

**Interview Answer:** "Synchronous code runs first, then all microtasks (promises), then macrotasks (setTimeout). Even with 0 delay, setTimeout waits for microtasks."

---

### 5. Prototype Chain Pitfalls

**Question:** What happens here?

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.friends = [];

const alice = new Person('Alice');
const bob = new Person('Bob');

alice.friends.push('Charlie');

console.log(bob.friends); // ['Charlie'] (!)

// Why? friends array is on the prototype, shared by all instances

// Solution: Put in constructor
function FixedPerson(name) {
  this.name = name;
  this.friends = []; // Each instance gets own array
}
```

**Interview Answer:** "Properties on the prototype are shared across all instances. Mutable objects like arrays should be in the constructor for instance-specific data."

---

## MOCK INTERVIEW SCRIPTS

### Fresher Level (15 minutes)

**Question 1:** What are the primitive data types in JavaScript?
- **Expected:** 7 types, explain string/number/boolean/null/undefined
- **Follow-up:** What does `typeof null` return and why?

**Question 2:** Explain `var`, `let`, and `const`.
- **Expected:** Scope differences, hoisting, reassignment
- **Follow-up:** Fix a var loop closure bug

**Question 3:** What's the difference between `==` and `===`?
- **Expected:** Type coercion explanation
- **Follow-up:** Explain why `[] == ![]` is true

**Question 4:** Write a function to reverse a string.
- **Expected:** Working solution with tests
- **Follow-up:** Time/space complexity

**Question 5:** How do closures work?
- **Expected:** Inner function accessing outer scope
- **Follow-up:** Implement a counter with private state

---

### Intermediate Level (30 minutes)

**Question 1:** Explain the event loop and microtasks vs macrotasks.
- **Expected:** Call stack, queues, execution order
- **Follow-up:** Predict output of mixed sync/async code

**Question 2:** What are Promises? How do you handle errors?
- **Expected:** States, chaining, catch
- **Follow-up:** Convert callback to Promise

**Question 3:** Explain `this` binding rules.
- **Expected:** Method, standalone, constructor, arrow, explicit
- **Follow-up:** Fix lost context in callback

**Question 4:** Implement debounce from scratch.
- **Expected:** Timeout management, closure
- **Follow-up:** Add immediate execution option

**Question 5:** Flatten a nested array.
- **Expected:** Recursive or iterative solution
- **Follow-up:** Handle custom depth limit

---

### Senior Level (45 minutes)

**Question 1:** How do you prevent memory leaks in JavaScript?
- **Expected:** Event listeners, timers, closures, circular refs
- **Follow-up:** Identify leak in provided code

**Question 2:** Explain prototypal inheritance and ES6 classes.
- **Expected:** Prototype chain, syntactic sugar
- **Follow-up:** Implement inheritance without classes

**Question 3:** Implement deep clone handling circular references.
- **Expected:** WeakMap for tracking, recursion
- **Follow-up:** Handle special objects (Date, RegExp)

**Question 4:** Design a rate limiter using throttle/debounce.
- **Expected:** Strategy selection, implementation
- **Follow-up:** Combine both strategies

**Question 5:** Optimize a slow DOM-heavy application.
- **Expected:** Virtual DOM, batching, event delegation
- **Follow-up:** Profile and identify bottlenecks

---

## Final Checklist

### Core Concepts
- [ ] Understand primitives vs reference types
- [ ] Master scope, hoisting, and closures
- [ ] Explain event loop confidently
- [ ] Know `this` binding rules
- [ ] Understand prototypal inheritance

### Async JavaScript
- [ ] Promises vs async/await
- [ ] Microtasks vs macrotasks
- [ ] Error handling patterns
- [ ] Parallel vs sequential execution

### Practical Skills
- [ ] DOM manipulation efficiently
- [ ] Event handling and delegation
- [ ] Debounce and throttle
- [ ] Memory leak prevention
- [ ] Performance optimization

### Coding
- [ ] Array/string manipulation
- [ ] Recursion and iteration
- [ ] Higher-order functions
- [ ] Algorithm complexity analysis

### Communication
- [ ] Explain trade-offs clearly
- [ ] Think aloud during coding
- [ ] Ask clarifying questions
- [ ] Provide examples

---

## How to Use This Guide

1. **For Freshers:** Start with Basics, master fundamentals before moving on
2. **For Intermediate:** Review Basics quickly, focus on Intermediate and coding
3. **For Senior:** Skim all, deep-dive Advanced and system design aspects
4. **Practice:** Code every example, don't just read
5. **Mock Interviews:** Have a friend quiz you using the scripts
6. **Build Projects:** Apply concepts in real applications

Good luck with your interview preparation! 🚀




# JavaScript Interview Preparation - Part 2
### Coding Problems, Tricky Questions & Mock Interviews

---

## Table of Contents
1. [Frontend Coding Challenges](#1-frontend-coding-challenges)
2. [Algorithmic Problems](#2-algorithmic-problems)
3. [Advanced Tricky Questions](#3-advanced-tricky-questions)
4. [Comprehensive Mock Interviews](#4-comprehensive-mock-interviews)
5. [System Design Questions](#5-system-design-questions)
6. [Performance Optimization](#6-performance-optimization)

---

## 1. FRONTEND CODING CHALLENGES

### 1.1 Todo List with Local Storage
**Difficulty:** Easy-Medium

**Question:** Build a todo list that persists data to localStorage with add, delete, and toggle completion features.

**Expected Approach:**
1. Create state management for todos
2. Implement CRUD operations
3. Sync with localStorage on every change
4. Handle edge cases (duplicates, empty input)

**Solution:**
```javascript
class TodoApp {
  constructor() {
    this.todos = this.loadTodos();
    this.init();
  }

  loadTodos() {
    try {
      const data = localStorage.getItem('todos');
      return data ? JSON.parse(data) : [];
    } catch (error) {
      console.error('Failed to load todos:', error);
      return [];
    }
  }

  saveTodos() {
    try {
      localStorage.setItem('todos', JSON.stringify(this.todos));
    } catch (error) {
      console.error('Failed to save todos:', error);
    }
  }

  addTodo(text) {
    if (!text || text.trim().length === 0) {
      throw new Error('Todo text cannot be empty');
    }

    const todo = {
      id: Date.now().toString(),
      text: text.trim(),
      completed: false,
      createdAt: new Date().toISOString()
    };

    this.todos.push(todo);
    this.saveTodos();
    return todo;
  }

  deleteTodo(id) {
    const index = this.todos.findIndex(todo => todo.id === id);
    if (index === -1) {
      throw new Error('Todo not found');
    }

    this.todos.splice(index, 1);
    this.saveTodos();
  }

  toggleTodo(id) {
    const todo = this.todos.find(todo => todo.id === id);
    if (!todo) {
      throw new Error('Todo not found');
    }

    todo.completed = !todo.completed;
    this.saveTodos();
    return todo;
  }

  getTodos(filter = 'all') {
    switch (filter) {
      case 'active':
        return this.todos.filter(todo => !todo.completed);
      case 'completed':
        return this.todos.filter(todo => todo.completed);
      default:
        return this.todos;
    }
  }

  clearCompleted() {
    this.todos = this.todos.filter(todo => !todo.completed);
    this.saveTodos();
  }

  init() {
    // DOM manipulation example
    const form = document.getElementById('todo-form');
    const input = document.getElementById('todo-input');
    const list = document.getElementById('todo-list');

    if (!form || !input || !list) return;

    form.addEventListener('submit', (e) => {
      e.preventDefault();
      try {
        this.addTodo(input.value);
        input.value = '';
        this.render();
      } catch (error) {
        alert(error.message);
      }
    });

    this.render();
  }

  render() {
    const list = document.getElementById('todo-list');
    if (!list) return;

    list.innerHTML = this.todos
      .map(todo => `
        <li class="${todo.completed ? 'completed' : ''}" data-id="${todo.id}">
          <input
            type="checkbox"
            ${todo.completed ? 'checked' : ''}
            onchange="app.toggleTodo('${todo.id}')"
          >
          <span>${this.escapeHtml(todo.text)}</span>
          <button onclick="app.deleteTodo('${todo.id}')">Delete</button>
        </li>
      `)
      .join('');
  }

  escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  }
}

// Initialize app
const app = new TodoApp();
```

**HTML Structure:**
```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .completed { text-decoration: line-through; opacity: 0.6; }
    li { list-style: none; padding: 8px; margin: 4px 0; }
  </style>
</head>
<body>
  <form id="todo-form">
    <input id="todo-input" type="text" placeholder="Add todo..." required>
    <button type="submit">Add</button>
  </form>
  <ul id="todo-list"></ul>
  <script src="todo.js"></script>
</body>
</html>
```

**Tests:**
```javascript
// Test suite
describe('TodoApp', () => {
  let app;

  beforeEach(() => {
    localStorage.clear();
    app = new TodoApp();
  });

  test('adds todo successfully', () => {
    const todo = app.addTodo('Buy milk');
    expect(todo.text).toBe('Buy milk');
    expect(todo.completed).toBe(false);
    expect(app.todos.length).toBe(1);
  });

  test('throws error for empty todo', () => {
    expect(() => app.addTodo('')).toThrow('Todo text cannot be empty');
    expect(() => app.addTodo('   ')).toThrow('Todo text cannot be empty');
  });

  test('toggles todo completion', () => {
    const todo = app.addTodo('Test');
    app.toggleTodo(todo.id);
    expect(app.todos[0].completed).toBe(true);
  });

  test('deletes todo', () => {
    const todo = app.addTodo('Test');
    app.deleteTodo(todo.id);
    expect(app.todos.length).toBe(0);
  });

  test('persists to localStorage', () => {
    app.addTodo('Persistent todo');
    const newApp = new TodoApp();
    expect(newApp.todos.length).toBe(1);
    expect(newApp.todos[0].text).toBe('Persistent todo');
  });

  test('filters todos correctly', () => {
    app.addTodo('Active 1');
    const completed = app.addTodo('Completed');
    app.toggleTodo(completed.id);
    app.addTodo('Active 2');

    expect(app.getTodos('active').length).toBe(2);
    expect(app.getTodos('completed').length).toBe(1);
    expect(app.getTodos('all').length).toBe(3);
  });
});
```

**Complexity:**
- Add/Delete/Toggle: O(n) for finding, O(1) for operation
- Space: O(n) for storing todos

**Interviewer Evaluates:**
- ✅ Data structure choice
- ✅ Error handling (empty input, localStorage failures)
- ✅ XSS prevention (escapeHtml)
- ✅ Clean API design
- ✅ Persistence implementation

**Follow-up Questions:**
- "How would you implement undo/redo?"
- "How would you add due dates and sorting?"
- "What if localStorage quota is exceeded?"

**Interview Tip:** *"I'm implementing a todo app with localStorage persistence. Key considerations are input validation to prevent empty todos, XSS protection by escaping HTML, and error handling for localStorage operations since they can fail if quota is exceeded."*

---

### 1.2 Debounce & Throttle (Advanced)
**Difficulty:** Medium

**Question:** Implement both debounce and throttle with support for leading/trailing edge execution and cancellation.

**Solution:**
```javascript
// Advanced Debounce
function debounce(func, wait, options = {}) {
  const { leading = false, trailing = true, maxWait = null } = options;

  let timeoutId;
  let lastCallTime = 0;
  let lastInvokeTime = 0;
  let lastArgs;
  let lastThis;
  let result;

  function invokeFunc(time) {
    const args = lastArgs;
    const thisArg = lastThis;

    lastArgs = lastThis = undefined;
    lastInvokeTime = time;
    result = func.apply(thisArg, args);
    return result;
  }

  function shouldInvoke(time) {
    const timeSinceLastCall = time - lastCallTime;
    const timeSinceLastInvoke = time - lastInvokeTime;

    return (
      lastCallTime === 0 ||
      timeSinceLastCall >= wait ||
      (maxWait !== null && timeSinceLastInvoke >= maxWait)
    );
  }

  function leadingEdge(time) {
    lastInvokeTime = time;
    timeoutId = setTimeout(timerExpired, wait);
    return leading ? invokeFunc(time) : result;
  }

  function trailingEdge(time) {
    timeoutId = undefined;

    if (trailing && lastArgs) {
      return invokeFunc(time);
    }
    lastArgs = lastThis = undefined;
    return result;
  }

  function timerExpired() {
    const time = Date.now();
    if (shouldInvoke(time)) {
      return trailingEdge(time);
    }
    // Restart timer
    const timeSinceLastCall = time - lastCallTime;
    const timeWaiting = wait - timeSinceLastCall;
    timeoutId = setTimeout(timerExpired, timeWaiting);
  }

  function debounced(...args) {
    const time = Date.now();
    const isInvoking = shouldInvoke(time);

    lastArgs = args;
    lastThis = this;
    lastCallTime = time;

    if (isInvoking) {
      if (timeoutId === undefined) {
        return leadingEdge(lastCallTime);
      }
      if (maxWait !== null) {
        // Handle maxWait
        timeoutId = setTimeout(timerExpired, wait);
        return invokeFunc(lastCallTime);
      }
    }

    if (timeoutId === undefined) {
      timeoutId = setTimeout(timerExpired, wait);
    }
    return result;
  }

  debounced.cancel = function() {
    if (timeoutId !== undefined) {
      clearTimeout(timeoutId);
    }
    lastInvokeTime = 0;
    lastArgs = lastCallTime = lastThis = timeoutId = undefined;
  };

  debounced.flush = function() {
    return timeoutId === undefined ? result : trailingEdge(Date.now());
  };

  debounced.pending = function() {
    return timeoutId !== undefined;
  };

  return debounced;
}

// Advanced Throttle
function throttle(func, wait, options = {}) {
  const { leading = true, trailing = true } = options;

  let timeoutId;
  let lastCallTime = 0;
  let lastInvokeTime = 0;
  let lastArgs;
  let lastThis;
  let result;

  function invokeFunc(time) {
    const args = lastArgs;
    const thisArg = lastThis;

    lastArgs = lastThis = undefined;
    lastInvokeTime = time;
    result = func.apply(thisArg, args);
    return result;
  }

  function shouldInvoke(time) {
    const timeSinceLastInvoke = time - lastInvokeTime;
    return lastInvokeTime === 0 || timeSinceLastInvoke >= wait;
  }

  function trailingEdge() {
    timeoutId = undefined;

    if (trailing && lastArgs) {
      return invokeFunc(Date.now());
    }
    lastArgs = lastThis = undefined;
    return result;
  }

  function throttled(...args) {
    const time = Date.now();
    const isInvoking = shouldInvoke(time);

    lastArgs = args;
    lastThis = this;
    lastCallTime = time;

    if (isInvoking) {
      if (leading) {
        lastInvokeTime = time;
        result = func.apply(this, args);
      }

      if (trailing) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(trailingEdge, wait);
      }

      return result;
    }

    if (trailing && timeoutId === undefined) {
      timeoutId = setTimeout(trailingEdge, wait);
    }

    return result;
  }

  throttled.cancel = function() {
    if (timeoutId !== undefined) {
      clearTimeout(timeoutId);
    }
    lastInvokeTime = lastCallTime = 0;
    lastArgs = lastThis = timeoutId = undefined;
  };

  throttled.flush = function() {
    return timeoutId === undefined ? result : trailingEdge();
  };

  return throttled;
}
```

**Tests:**
```javascript
describe('Debounce & Throttle', () => {
  jest.useFakeTimers();

  describe('debounce', () => {
    test('delays execution until wait time passes', () => {
      let callCount = 0;
      const debounced = debounce(() => callCount++, 100);

      debounced();
      expect(callCount).toBe(0);

      jest.advanceTimersByTime(99);
      expect(callCount).toBe(0);

      jest.advanceTimersByTime(1);
      expect(callCount).toBe(1);
    });

    test('resets timer on repeated calls', () => {
      let callCount = 0;
      const debounced = debounce(() => callCount++, 100);

      debounced();
      jest.advanceTimersByTime(50);
      debounced(); // Resets timer
      jest.advanceTimersByTime(50);
      expect(callCount).toBe(0); // Still waiting

      jest.advanceTimersByTime(50);
      expect(callCount).toBe(1);
    });

    test('leading edge execution', () => {
      let callCount = 0;
      const debounced = debounce(() => callCount++, 100, { leading: true });

      debounced();
      expect(callCount).toBe(1); // Immediate execution

      jest.advanceTimersByTime(100);
      expect(callCount).toBe(1); // No trailing by default when leading
    });

    test('maxWait option', () => {
      let callCount = 0;
      const debounced = debounce(() => callCount++, 100, { maxWait: 200 });

      debounced();
      jest.advanceTimersByTime(150);
      debounced();
      jest.advanceTimersByTime(60); // Total 210ms, exceeds maxWait
      expect(callCount).toBe(1);
    });

    test('cancel method', () => {
      let callCount = 0;
      const debounced = debounce(() => callCount++, 100);

      debounced();
      debounced.cancel();
      jest.advanceTimersByTime(100);
      expect(callCount).toBe(0);
    });

    test('flush method', () => {
      let callCount = 0;
      const debounced = debounce(() => callCount++, 100);

      debounced();
      jest.advanceTimersByTime(50);
      debounced.flush(); // Force execution
      expect(callCount).toBe(1);
    });
  });

  describe('throttle', () => {
    test('executes at most once per wait period', () => {
      let callCount = 0;
      const throttled = throttle(() => callCount++, 100);

      throttled(); // Immediate (leading)
      expect(callCount).toBe(1);

      throttled();
      throttled();
      expect(callCount).toBe(1); // Still in wait period

      jest.advanceTimersByTime(100);
      throttled();
      expect(callCount).toBe(2);
    });

    test('trailing edge execution', () => {
      let callCount = 0;
      const throttled = throttle(() => callCount++, 100);

      throttled();
      expect(callCount).toBe(1);

      throttled(); // Queued for trailing
      jest.advanceTimersByTime(100);
      expect(callCount).toBe(2);
    });

    test('no leading edge', () => {
      let callCount = 0;
      const throttled = throttle(() => callCount++, 100, { leading: false });

      throttled();
      expect(callCount).toBe(0);

      jest.advanceTimersByTime(100);
      expect(callCount).toBe(1);
    });
  });
});
```

**Real-world Usage:**
```javascript
// Search input with debounce
const searchInput = document.getElementById('search');
const debouncedSearch = debounce(async (query) => {
  const results = await fetch(`/api/search?q=${query}`).then(r => r.json());
  displayResults(results);
}, 300, { maxWait: 1000 }); // Max 1s wait even with continuous typing

searchInput.addEventListener('input', (e) => {
  debouncedSearch(e.target.value);
});

// Scroll handler with throttle
const throttledScroll = throttle(() => {
  const scrollPercent = (window.scrollY / document.body.scrollHeight) * 100;
  updateProgressBar(scrollPercent);
}, 200);

window.addEventListener('scroll', throttledScroll);

// Window resize with debounce (wait for resize to finish)
const debouncedResize = debounce(() => {
  recalculateLayout();
}, 250);

window.addEventListener('resize', debouncedResize);
```

**Interview Tip:** *"Debounce delays execution until activity stops—perfect for search inputs where I only want to query after the user stops typing. Throttle guarantees execution at regular intervals—ideal for scroll handlers where I want updates but not on every pixel. The key difference: debounce waits for silence, throttle enforces a rate limit."*

---

### 1.3 Infinite Scroll
**Difficulty:** Medium

**Question:** Implement infinite scroll that loads more items when user reaches the bottom.

**Expected Approach:**
1. Detect when user is near bottom using Intersection Observer or scroll events
2. Fetch new data asynchronously
3. Handle loading states and errors
4. Prevent duplicate requests
5. Optimize performance

**Solution:**
```javascript
class InfiniteScroll {
  constructor(options) {
    this.container = options.container;
    this.loadMore = options.loadMore;
    this.threshold = options.threshold || 0.8; // Load when 80% scrolled
    this.isLoading = false;
    this.hasMore = true;
    this.page = 1;
    this.observer = null;

    this.init();
  }

  init() {
    // Create sentinel element to observe
    this.sentinel = document.createElement('div');
    this.sentinel.className = 'infinite-scroll-sentinel';
    this.sentinel.style.height = '1px';
    this.container.appendChild(this.sentinel);

    // Use Intersection Observer (more performant than scroll events)
    this.observer = new IntersectionObserver(
      (entries) => this.handleIntersection(entries),
      {
        root: null, // viewport
        rootMargin: '100px', // Load 100px before reaching sentinel
        threshold: 0
      }
    );

    this.observer.observe(this.sentinel);
  }

  async handleIntersection(entries) {
    const entry = entries[0];

    if (entry.isIntersecting && !this.isLoading && this.hasMore) {
      await this.load();
    }
  }

  async load() {
    if (this.isLoading || !this.hasMore) return;

    this.isLoading = true;
    this.showLoading();

    try {
      const data = await this.loadMore(this.page);

      if (!data || data.length === 0) {
        this.hasMore = false;
        this.showNoMore();
      } else {
        this.renderItems(data);
        this.page++;
      }
    } catch (error) {
      console.error('Failed to load more items:', error);
      this.showError(error);
    } finally {
      this.isLoading = false;
      this.hideLoading();
    }
  }

  renderItems(items) {
    const fragment = document.createDocumentFragment();

    items.forEach(item => {
      const element = this.createItemElement(item);
      fragment.appendChild(element);
    });

    // Insert before sentinel
    this.container.insertBefore(fragment, this.sentinel);
  }

  createItemElement(item) {
    const div = document.createElement('div');
    div.className = 'item';
    div.textContent = item.title || item.name || JSON.stringify(item);
    return div;
  }

  showLoading() {
    let loader = this.container.querySelector('.loader');
    if (!loader) {
      loader = document.createElement('div');
      loader.className = 'loader';
      loader.textContent = 'Loading...';
      this.container.insertBefore(loader, this.sentinel);
    }
    loader.style.display = 'block';
  }

  hideLoading() {
    const loader = this.container.querySelector('.loader');
    if (loader) {
      loader.style.display = 'none';
    }
  }

  showNoMore() {
    const message = document.createElement('div');
    message.className = 'no-more';
    message.textContent = 'No more items to load';
    this.container.insertBefore(message, this.sentinel);
  }

  showError(error) {
    const errorDiv = document.createElement('div');
    errorDiv.className = 'error';
    errorDiv.textContent = `Error: ${error.message}`;
    this.container.insertBefore(errorDiv, this.sentinel);
  }

  reset() {
    this.page = 1;
    this.hasMore = true;
    this.isLoading = false;

    // Clear items (except sentinel)
    while (this.container.firstChild && this.container.firstChild !== this.sentinel) {
      this.container.removeChild(this.container.firstChild);
    }
  }

  destroy() {
    if (this.observer) {
      this.observer.disconnect();
    }
    if (this.sentinel && this.sentinel.parentNode) {
      this.sentinel.parentNode.removeChild(this.sentinel);
    }
  }
}

// Usage Example
async function loadMorePosts(page) {
  const response = await fetch(`/api/posts?page=${page}&limit=20`);
  if (!response.ok) {
    throw new Error('Failed to fetch posts');
  }
  return response.json();
}

const container = document.getElementById('posts-container');
const infiniteScroll = new InfiniteScroll({
  container,
  loadMore: loadMorePosts
});

// Alternative: Scroll Event Based (less efficient but more control)
class InfiniteScrollLegacy {
  constructor(options) {
    this.container = options.container;
    this.loadMore = options.loadMore;
    this.threshold = options.threshold || 0.8;
    this.isLoading = false;
    this.hasMore = true;
    this.page = 1;

    this.handleScroll = this.handleScroll.bind(this);
    this.throttledScroll = this.throttle(this.handleScroll, 200);

    window.addEventListener('scroll', this.throttledScroll);
  }

  handleScroll() {
    if (this.isLoading || !this.hasMore) return;

    const scrollTop = window.scrollY || document.documentElement.scrollTop;
    const scrollHeight = document.documentElement.scrollHeight;
    const clientHeight = window.innerHeight;

    const scrollPercent = scrollTop / (scrollHeight - clientHeight);

    if (scrollPercent >= this.threshold) {
      this.load();
    }
  }

  throttle(func, wait) {
    let timeout;
    return function(...args) {
      if (!timeout) {
        timeout = setTimeout(() => {
          timeout = null;
          func.apply(this, args);
        }, wait);
      }
    };
  }

  async load() {
    // Same as above
  }

  destroy() {
    window.removeEventListener('scroll', this.throttledScroll);
  }
}
```

**Performance Comparison:**
```javascript
// Intersection Observer (Recommended)
// ✅ Better performance - runs off main thread
// ✅ Automatic handling of visibility
// ✅ No need for throttling
// ✅ Works with lazy loading images

// Scroll Events
// ⚠️ Runs on main thread
// ⚠️ Needs throttling/debouncing
// ✅ More granular control
// ✅ Works in older browsers
```

**Tests:**
```javascript
describe('InfiniteScroll', () => {
  let container;
  let mockLoadMore;

  beforeEach(() => {
    container = document.createElement('div');
    document.body.appendChild(container);
    mockLoadMore = jest.fn();
  });

  afterEach(() => {
    document.body.removeChild(container);
  });

  test('initializes with sentinel element', () => {
    const scroll = new InfiniteScroll({ container, loadMore: mockLoadMore });
    const sentinel = container.querySelector('.infinite-scroll-sentinel');
    expect(sentinel).toBeTruthy();
  });

  test('loads more when sentinel is visible', async () => {
    mockLoadMore.mockResolvedValue([{ id: 1 }, { id: 2 }]);

    const scroll = new InfiniteScroll({ container, loadMore: mockLoadMore });

    // Simulate intersection
    const entries = [{ isIntersecting: true }];
    await scroll.handleIntersection(entries);

    expect(mockLoadMore).toHaveBeenCalledWith(1);
    expect(container.querySelectorAll('.item').length).toBe(2);
  });

  test('does not load when already loading', async () => {
    mockLoadMore.mockImplementation(() => new Promise(resolve => {
      setTimeout(() => resolve([{ id: 1 }]), 100);
    }));

    const scroll = new InfiniteScroll({ container, loadMore: mockLoadMore });

    scroll.load();
    scroll.load(); // Second call should be ignored

    await new Promise(resolve => setTimeout(resolve, 150));
    expect(mockLoadMore).toHaveBeenCalledTimes(1);
  });

  test('stops loading when no more data', async () => {
    mockLoadMore.mockResolvedValue([]);

    const scroll = new InfiniteScroll({ container, loadMore: mockLoadMore });
    await scroll.load();

    expect(scroll.hasMore).toBe(false);
    expect(container.querySelector('.no-more')).toBeTruthy();
  });

  test('handles errors gracefully', async () => {
    mockLoadMore.mockRejectedValue(new Error('Network error'));

    const scroll = new InfiniteScroll({ container, loadMore: mockLoadMore });
    await scroll.load();

    expect(container.querySelector('.error')).toBeTruthy();
  });
});
```

**Complexity:**
- Time: O(n) for rendering n items
- Space: O(n) for storing items in DOM
- Network: Batch requests to reduce overhead

**Interview Tip:** *"I'm using Intersection Observer for infinite scroll because it's more performant than scroll events—it runs off the main thread and doesn't need throttling. I track loading state to prevent duplicate requests, handle errors gracefully, and use a sentinel element that triggers loading when it becomes visible near the bottom."*

**Follow-up Questions:**
- "How would you implement bidirectional infinite scroll?"
- "How would you handle data updates while scrolling?"
- "What if items have dynamic heights?"

---

### 1.4 Auto-complete Search
**Difficulty:** Medium

**Question:** Build an autocomplete component with debouncing, keyboard navigation, and highlighting.

**Solution:**
```javascript
class AutoComplete {
  constructor(options) {
    this.input = options.input;
    this.fetchSuggestions = options.fetchSuggestions;
    this.onSelect = options.onSelect;
    this.minChars = options.minChars || 2;
    this.debounceTime = options.debounceTime || 300;
    this.maxResults = options.maxResults || 10;

    this.selectedIndex = -1;
    this.suggestions = [];
    this.isOpen = false;

    this.init();
  }

  init() {
    // Create results container
    this.resultsContainer = document.createElement('ul');
    this.resultsContainer.className = 'autocomplete-results';
    this.resultsContainer.style.display = 'none';
    this.input.parentNode.appendChild(this.resultsContainer);

    // Debounced search
    this.debouncedSearch = this.debounce(
      this.search.bind(this),
      this.debounceTime
    );

    // Event listeners
    this.input.addEventListener('input', (e) => {
      this.handleInput(e.target.value);
    });

    this.input.addEventListener('keydown', (e) => {
      this.handleKeydown(e);
    });

    this.input.addEventListener('blur', () => {
      // Delay to allow click on results
      setTimeout(() => this.close(), 200);
    });

    // Click outside to close
    document.addEventListener('click', (e) => {
      if (!this.input.contains(e.target) && !this.resultsContainer.contains(e.target)) {
        this.close();
      }
    });
  }

  handleInput(value) {
    if (value.length < this.minChars) {
      this.close();
      return;
    }

    this.debouncedSearch(value);
  }

  async search(query) {
    try {
      const results = await this.fetchSuggestions(query);
      this.suggestions = results.slice(0, this.maxResults);
      this.render();
    } catch (error) {
      console.error('Search failed:', error);
      this.suggestions = [];
      this.close();
    }
  }

  render() {
    if (this.suggestions.length === 0) {
      this.close();
      return;
    }

    const query = this.input.value.toLowerCase();

    this.resultsContainer.innerHTML = this.suggestions
      .map((item, index) => {
        const text = typeof item === 'string' ? item : item.label || item.name;
        const highlighted = this.highlightMatch(text, query);
        const isSelected = index === this.selectedIndex;

        return `
          <li
            class="autocomplete-item ${isSelected ? 'selected' : ''}"
            data-index="${index}"
          >
            ${highlighted}
          </li>
        `;
      })
      .join('');

    // Add click handlers
    this.resultsContainer.querySelectorAll('.autocomplete-item').forEach(item => {
      item.addEventListener('click', () => {
        const index = parseInt(item.dataset.index);
        this.select(index);
      });
    });

    this.open();
  }

  highlightMatch(text, query) {
    const index = text.toLowerCase().indexOf(query);
    if (index === -1) return this.escapeHtml(text);

    const before = text.substring(0, index);
    const match = text.substring(index, index + query.length);
    const after = text.substring(index + query.length);

    return `${this.escapeHtml(before)}<strong>${this.escapeHtml(match)}</strong>${this.escapeHtml(after)}`;
  }

  handleKeydown(e) {
    if (!this.isOpen) return;

    switch (e.key) {
      case 'ArrowDown':
        e.preventDefault();
        this.selectedIndex = Math.min(
          this.selectedIndex + 1,
          this.suggestions.length - 1
        );
        this.render();
        break;

      case 'ArrowUp':
        e.preventDefault();
        this.selectedIndex = Math.max(this.selectedIndex - 1, -1);
        this.render();
        break;

      case 'Enter':
        e.preventDefault();
        if (this.selectedIndex >= 0) {
          this.select(this.selectedIndex);
        }
        break;

      case 'Escape':
        this.close();
        break;
    }
  }

  select(index) {
    const item = this.suggestions[index];
    const value = typeof item === 'string' ? item : item.value || item.label;

    this.input.value = value;
    this.close();

    if (this.onSelect) {
      this.onSelect(item);
    }
  }

  open() {
    this.isOpen = true;
    this.resultsContainer.style.display = 'block';
  }

  close() {
    this.isOpen = false;
    this.selectedIndex = -1;
    this.resultsContainer.style.display = 'none';
  }

  debounce(func, wait) {
    let timeout;
    return function(...args) {
      clearTimeout(timeout);
      timeout = setTimeout(() => func.apply(this, args), wait);
    };
  }

  escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  }

  destroy() {
    if (this.resultsContainer && this.resultsContainer.parentNode) {
      this.resultsContainer.parentNode.removeChild(this.resultsContainer);
    }
  }
}

// Usage
const input = document.getElementById('search-input');

const autoComplete = new AutoComplete({
  input,
  minChars: 2,
  debounceTime: 300,
  fetchSuggestions: async (query) => {
    const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`);
    const data = await response.json();
    return data.results; // ['Apple', 'Application', 'Approach']
  },
  onSelect: (item) => {
    console.log('Selected:', item);
    // Navigate or perform action
  }
});
```

**CSS:**
```css
.autocomplete-results {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  max-height: 300px;
  overflow-y: auto;
  background: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  list-style: none;
  padding: 0;
  margin: 4px 0 0 0;
  z-index: 1000;
}

.autocomplete-item {
  padding: 8px 12px;
  cursor: pointer;
  transition: background 0.2s;
}

.autocomplete-item:hover,
.autocomplete-item.selected {
  background: #f0f0f0;
}

.autocomplete-item strong {
  font-weight: 600;
  color: #2563eb;
}
```

**Interview Tip:** *"I'm implementing autocomplete with debouncing to reduce API calls—only searching after the user stops typing for 300ms. Keyboard navigation with arrow keys and Enter improves accessibility. I highlight matching text for better UX and handle edge cases like clicking outside to close and preventing race conditions with the selected index."*

---

## 2. ALGORITHMIC PROBLEMS

### 2.1 Longest Substring Without Repeating Characters
**Difficulty:** Medium

**Question:** Find the length of the longest substring without repeating characters.

**Approach:** Sliding window with hash map to track character positions.

**Solution:**
```javascript
function lengthOfLongestSubstring(s) {
  const seen = new Map();
  let maxLength = 0;
  let start = 0;

  for (let end = 0; end < s.length; end++) {
    const char = s[end];

    // If character seen and within current window
    if (seen.has(char) && seen.get(char) >= start) {
      start = seen.get(char) + 1;
    }

    seen.set(char, end);
    maxLength = Math.max(maxLength, end - start + 1);
  }

  return maxLength;
}

// Alternative: Set-based approach
function lengthOfLongestSubstringSet(s) {
  const window = new Set();
  let maxLength = 0;
  let left = 0;

  for (let right = 0; right < s.length; right++) {
    // Remove characters until no duplicate
    while (window.has(s[right])) {
      window.delete(s[left]);
      left++;
    }

    window.add(s[right]);
    maxLength = Math.max(maxLength, right - left + 1);
  }

  return maxLength;
}
```

**Tests:**
```javascript
console.log(lengthOfLongestSubstring("abcabcbb"));  // 3 ("abc")
console.log(lengthOfLongestSubstring("bbbbb"));     // 1 ("b")
console.log(lengthOfLongestSubstring("pwwkew"));    // 3 ("wke")
console.log(lengthOfLongestSubstring(""));          // 0
console.log(lengthOfLongestSubstring(" "));         // 1
console.log(lengthOfLongestSubstring("abba"));      // 2 ("ab" or "ba")
```

**Complexity:**
- Time: O(n) - single pass through string
- Space: O(min(n, m)) where m is charset size

**Interview Tip:** *"I'm using a sliding window technique with a hash map to track the last seen position of each character. When I encounter a duplicate, I move the window start to after the previous occurrence. This gives O(n) time complexity with a single pass through the string."*

---

### 2.2 Valid Parentheses
**Difficulty:** Easy

**Question:** Determine if string with brackets `()[]{}` is valid (properly closed and nested).

**Solution:**
```javascript
function isValid(s) {
  const stack = [];
  const pairs = {
    ')': '(',
    ']': '[',
    '}': '{'
  };

  for (const char of s) {
    if (char === '(' || char === '[' || char === '{') {
      stack.push(char);
    } else {
      if (stack.length === 0 || stack.pop() !== pairs[char]) {
        return false;
      }
    }
  }

  return stack.length === 0;
}

// Cleaner version
function isValidClean(s) {
  const stack = [];
  const map = { '(': ')', '[': ']', '{': '}' };

  for (const char of s) {
    if (char in map) {
      stack.push(char);
    } else {
      if (stack.length === 0 || map[stack.pop()] !== char) {
        return false;
      }
    }
  }

  return stack.length === 0;
}
```

**Tests:**
```javascript
console.log(isValid("()"));        // true
console.log(isValid("()[]{}"));    // true
console.log(isValid("(]"));        // false
console.log(isValid("([)]"));      // false
console.log(isValid("{[]}"));      // true
console.log(isValid(""));          // true (empty is valid)
console.log(isValid("(("));        // false (unclosed)
console.log(isValid("))"));        // false (no opening)
```

**Complexity:**
- Time: O(n)
- Space: O(n) for stack

---

### 2.3 Group Anagrams
**Difficulty:** Medium

**Question:** Group strings that are anagrams of each other.

**Solution:**
```javascript
function groupAnagrams(strs) {
  const map = new Map();

  for (const str of strs) {
    // Sort as key
    const key = str.split('').sort().join('');

    if (!map.has(key)) {
      map.set(key, []);
    }

    map.get(key).push(str);
  }

  return Array.from(map.values());
}

// Alternative: Character count as key (faster for long strings)
function groupAnagramsCount(strs) {
  const map = new Map();

  for (const str of strs) {
    const count = new Array(26).fill(0);

    for (const char of str) {
      count[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    const key = count.join('#');

    if (!map.has(key)) {
      map.set(key, []);
    }

    map.get(key).push(str);
  }

  return Array.from(map.values());
}
```

**Tests:**
```javascript
console.log(groupAnagrams(["eat","tea","tan","ate","nat","bat"]));
// [["eat","tea","ate"], ["tan","nat"], ["bat"]]

console.log(groupAnagrams([""]));  // [[""]]
console.log(groupAnagrams(["a"])); // [["a"]]
```

**Complexity:**
- Sort approach: O(n * k log k) where n = strings, k = max length
- Count approach: O(n * k)
- Space: O(n * k)

---

### 2.4 Product of Array Except Self
**Difficulty:** Medium

**Question:** Return array where each element is the product of all other elements (without division).

**Solution:**
```javascript
function productExceptSelf(nums) {
  const n = nums.length;
  const result = new Array(n);

  // Left products
  result[0] = 1;
  for (let i = 1; i < n; i++) {
    result[i] = result[i - 1] * nums[i - 1];
  }

  // Right products
  let rightProduct = 1;
  for (let i = n - 1; i >= 0; i--) {
    result[i] *= rightProduct;
    rightProduct *= nums[i];
  }

  return result;
}

// With explanation
function productExceptSelfExplained(nums) {
  const n = nums.length;
  const result = new Array(n);

  // Calculate left products
  // result[i] = product of all elements to left of i
  let leftProduct = 1;
  for (let i = 0; i < n; i++) {
    result[i] = leftProduct;
    leftProduct *= nums[i];
  }
  // Example: [1,2,3,4] -> result = [1,1,2,6]

  // Calculate right products and combine
  // Multiply each position by product of all elements to right
  let rightProduct = 1;
  for (let i = n - 1; i >= 0; i--) {
    result[i] *= rightProduct;
    rightProduct *= nums[i];
  }
  // Example: [1,1,2,6] * [24,12,4,1] = [24,12,8,6]

  return result;
}
```

**Tests:**
```javascript
console.log(productExceptSelf([1,2,3,4]));     // [24,12,8,6]
console.log(productExceptSelf([-1,1,0,-3,3])); // [0,0,9,0,0]
console.log(productExceptSelf([1,1]));         // [1,1]
```

**Complexity:**
- Time: O(n)
- Space: O(1) excluding output array

---

### 2.5 Merge Intervals
**Difficulty:** Medium

**Question:** Merge overlapping intervals.

**Solution:**
```javascript
function merge(intervals) {
  if (intervals.length <= 1) return intervals;

  // Sort by start time
  intervals.sort((a, b) => a[0] - b[0]);

  const result = [intervals[0]];

  for (let i = 1; i < intervals.length; i++) {
    const current = intervals[i];
    const last = result[result.length - 1];

    if (current[0] <= last[1]) {
      // Overlapping - merge
      last[1] = Math.max(last[1], current[1]);
    } else {
      // Non-overlapping - add new interval
      result.push(current);
    }
  }

  return result;
}
```

**Tests:**
```javascript
console.log(merge([[1,3],[2,6],[8,10],[15,18]])); // [[1,6],[8,10],[15,18]]
console.log(merge([[1,4],[4,5]]));                 // [[1,5]]
console.log(merge([[1,4],[2,3]]));                 // [[1,4]]
console.log(merge([[1,4],[0,4]]));                 // [[0,4]]
```

**Complexity:**
- Time: O(n log n) for sorting
- Space: O(n) for result

---

### 2.6 Binary Tree Level Order Traversal
**Difficulty:** Medium

**Question:** Return level-order traversal of binary tree (BFS).

**Solution:**
```javascript
class TreeNode {
  constructor(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

function levelOrder(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];

  while (queue.length > 0) {
    const levelSize = queue.length;
    const currentLevel = [];

    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      currentLevel.push(node.val);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.push(currentLevel);
  }

  return result;
}

// Recursive approach
function levelOrderRecursive(root) {
  const result = [];

  function traverse(node, level) {
    if (!node) return;

    if (result.length === level) {
      result.push([]);
    }

    result[level].push(node.val);

    traverse(node.left, level + 1);
    traverse(node.right, level + 1);
  }

  traverse(root, 0);
  return result;
}
```

**Tests:**
```javascript
//     3
//    / \
//   9  20
//     /  \
//    15   7

const tree = new TreeNode(3,
  new TreeNode(9),
  new TreeNode(20,
    new TreeNode(15),
    new TreeNode(7)
  )
);

console.log(levelOrder(tree)); // [[3],[9,20],[15,7]]
```

**Complexity:**
- Time: O(n)
- Space: O(n)

---

## 3. ADVANCED TRICKY QUESTIONS

### 3.1 JavaScript Execution Context

**Question:** What happens when this code runs? Explain the execution order.

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

console.log('4');

Promise.resolve().then(() => {
  console.log('5');
  setTimeout(() => console.log('6'), 0);
});

setTimeout(() => {
  console.log('7');
  Promise.resolve().then(() => console.log('8'));
}, 0);

console.log('9');
```

**Answer:**
```
Output: 1, 4, 9, 3, 5, 2, 7, 8, 6

Explanation:
1. Synchronous: 1, 4, 9
2. Microtasks (promises): 3, 5
3. Macrotask (setTimeout): 2
4. Macrotask: 7
5. Microtask from 7: 8
6. Macrotask from 5: 6
```

**Deep Explanation:**

**Execution phases:**
1. **Sync code runs first:** console.log('1'), '4', '9'
2. **Call stack empties → Process microtask queue:** Promises 3 and 5
3. **Process first macrotask:** setTimeout with '2'
4. **Process second macrotask:** setTimeout with '7'
5. **That macrotask queues microtask:** Promise with '8'
6. **Process microtask from step 4:** '8'
7. **Process macrotask from step 2:** setTimeout with '6'

**Key Rules:**
- Synchronous code executes immediately
- When call stack is empty, all microtasks run
- Then one macrotask runs
- Then microtasks again
- Repeat

**Interview Tip:** *"JavaScript's event loop processes synchronous code first, then all microtasks (promises), then one macrotask (setTimeout/setInterval), then microtasks again. This cycle repeats. Microtasks always have priority over the next macrotask, which is why promises execute before setTimeout even with 0 delay."*

---

### 3.2 The `this` Labyrinth

**Question:** What does each console.log output?

```javascript
const obj = {
  name: 'Object',
  regular: function() {
    console.log(this.name);
  },
  arrow: () => {
    console.log(this.name);
  },
  nested: function() {
    const inner = () => {
      console.log(this.name);
    };
    inner();
  },
  timeout: function() {
    setTimeout(function() {
      console.log(this.name);
    }, 0);
  },
  timeoutArrow: function() {
    setTimeout(() => {
      console.log(this.name);
    }, 0);
  }
};

obj.regular();        // 1
obj.arrow();          // 2
obj.nested();         // 3
obj.timeout();        // 4
obj.timeoutArrow();   // 5

const regular = obj.regular;
regular();            // 6

const bound = obj.regular.bind({ name: 'Bound' });
bound();              // 7
```

**Answers:**
```javascript
1. "Object"     // Method call - this is obj
2. undefined    // Arrow function - lexical this (window/global)
3. "Object"     // Arrow inner captures this from nested
4. undefined    // Regular function in setTimeout loses context
5. "Object"     // Arrow in setTimeout preserves context
6. undefined    // Lost context - this is window/undefined
7. "Bound"      // Explicitly bound
```

**Interview Tip:** *"Arrow functions don't have their own this—they capture it lexically from the enclosing scope. Regular functions get this from how they're called. When you pass a method as a callback, it loses its context unless you use arrow functions or bind."*

---

### 3.3 Closure Memory Quiz

**Question:** Explain the memory behavior and output.

```javascript
function createFunctions() {
  const functions = [];

  for (var i = 0; i < 3; i++) {
    functions.push(function() {
      console.log(i);
    });
  }

  return functions;
}

const fns = createFunctions();
fns[0](); // ?
fns[1](); // ?
fns[2](); // ?
```

**Answer:**
```
3
3
3

Why: var is function-scoped. All closures share the same i variable.
When functions execute, i is already 3.
```

**Fix 1: Use let (block-scoped)**
```javascript
for (let i = 0; i < 3; i++) {
  functions.push(function() {
    console.log(i);
  });
}
// Output: 0, 1, 2
```

**Fix 2: IIFE to capture value**
```javascript
for (var i = 0; i < 3; i++) {
  (function(j) {
    functions.push(function() {
      console.log(j);
    });
  })(i);
}
```

**Fix 3: forEach**
```javascript
[0, 1, 2].forEach(i => {
  functions.push(function() {
    console.log(i);
  });
});
```

**Interview Tip:** *"With var, all closures capture the same variable reference, not its value at that moment. By the time callbacks execute, the loop has finished and i is 3. Using let creates a new binding for each iteration, capturing the correct value."*

---

### 3.4 Type Coercion Extremes

**Question:** Explain these weird JavaScript behaviors.

```javascript
console.log([] + []);           // 1
console.log([] + {});           // 2
console.log({} + []);           // 3
console.log({} + {});           // 4
console.log(true + false);      // 5
console.log([1, 2] + [3, 4]);   // 6
console.log('5' + 3);           // 7
console.log('5' - 3);           // 8
console.log([] == ![]);         // 9
console.log(null == undefined); // 10
console.log(null === undefined);// 11
```

**Answers:**
```javascript
1. ""             // Both arrays → empty strings, concatenated
2. "[object Object]" // [] → "", {} → "[object Object]"
3. 0 or "[object Object]" // Depends on context!
4. NaN or "[object Object][object Object]"
5. 1              // true → 1, false → 0
6. "1,23,4"       // Arrays → strings, concatenated
7. "53"           // + with string → concatenation
8. 2              // - forces numeric coercion
9. true           // Complex coercion: [] → "" → 0, ![] → false → 0
10. true          // Special case in ==
11. false         // Different types
```

**Detailed Explanation for #9 ([] == ![]):**
```javascript
// Step by step:
[] == ![]
[] == false          // ![] is false (any object is truthy)
"" == false          // [] converts to "" (ToPrimitive)
0 == 0               // Both convert to 0
true                 // 0 === 0
```

**Interview Tip:** *"JavaScript's type coercion with == follows complex rules. Arrays convert to strings via ToPrimitive, then potentially to numbers. The + operator performs concatenation with strings but addition with numbers, while - always does numeric coercion. These quirks are why I always use === for predictable comparisons."*

---

### 3.5 Async/Await Error Handling

**Question:** What happens in each scenario?

```javascript
// Scenario 1: Missing await
async function scenario1() {
  const result = fetch('/api/data').then(r => r.json());
  console.log(result); // ?
}

// Scenario 2: Try-catch position
async function scenario2() {
  try {
    const data = await fetch('/api/data');
  } catch (error) {
    console.error(error);
  }
  console.log(data); // ?
}

// Scenario 3: Promise.all error handling
async function scenario3() {
  try {
    const results = await Promise.all([
      fetch('/api/1'),
      fetch('/api/2'), // This fails
      fetch('/api/3')
    ]);
  } catch (error) {
    // What happens to other requests?
  }
}

// Scenario 4: Unhandled rejection
async function scenario4() {
  const promise = Promise.reject('Error');
  // No await, no catch
  console.log('Done');
}
```

**Answers:**
```javascript
// Scenario 1:
// Logs: Promise { <pending> }
// Missing await - gets promise object, not resolved value

// Scenario 2:
// ReferenceError: data is not defined
// data is scoped to try block

// Scenario 3:
// If any promise rejects, Promise.all fails immediately
// Other requests may still be in-flight
// They'll complete but results are lost

// Scenario 4:
// Logs "Done", then Unhandled Promise Rejection warning
// Promise rejection not caught
```

**Correct Patterns:**
```javascript
// 1. Always await
const result = await fetch('/api/data').then(r => r.json());

// 2. Scope variables correctly
let data;
try {
  data = await fetch('/api/data');
} catch (error) {
  console.error(error);
}
console.log(data);

// 3. Use allSettled for independent operations
const results = await Promise.allSettled([
  fetch('/api/1'),
  fetch('/api/2'),
  fetch('/api/3')
]);

results.forEach((result, i) => {
  if (result.status === 'fulfilled') {
    console.log(`Request ${i} succeeded:`, result.value);
  } else {
    console.error(`Request ${i} failed:`, result.reason);
  }
});

// 4. Always handle promises
async function scenario4Fixed() {
  try {
    await Promise.reject('Error');
  } catch (error) {
    console.error(error);
  }
  console.log('Done');
}
```

**Interview Tip:** *"Common async/await mistakes: forgetting await returns a promise object, variable scoping in try-catch blocks, and using Promise.all when operations are independent. Promise.allSettled is better when you need all results regardless of failures. Always handle promise rejections to avoid unhandled rejection warnings."*

---

### 3.6 Prototype Chain Surprises

**Question:** Predict the output.

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  return `${this.name} makes a sound`;
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
  return `${this.name} barks`;
};

const dog = new Dog('Rex', 'Labrador');

console.log(dog.speak());                    // 1
console.log(dog instanceof Dog);             // 2
console.log(dog instanceof Animal);          // 3
console.log(dog.hasOwnProperty('name'));     // 4
console.log(dog.hasOwnProperty('speak'));    // 5
console.log(Dog.prototype.isPrototypeOf(dog)); // 6

// Tricky mutation
Animal.prototype.species = 'Mammal';
console.log(dog.species);                    // 7

delete Dog.prototype.speak;
console.log(dog.speak());                    // 8
```

**Answers:**
```javascript
1. "Rex barks"              // Dog's speak overrides Animal's
2. true                     // dog is instance of Dog
3. true                     // Prototype chain includes Animal
4. true                     // name is own property
5. false                    // speak is on prototype
6. true                     // Dog.prototype in chain
7. "Mammal"                 // Inherited through Animal.prototype
8. "Rex makes a sound"      // Falls back to Animal.prototype.speak
```

**Visualization:**
```
dog
  ├─ name: "Rex" (own)
  ├─ breed: "Labrador" (own)
  └─ [[Prototype]]: Dog.prototype
      ├─ speak: function() { ... }
      ├─ constructor: Dog
      └─ [[Prototype]]: Animal.prototype
          ├─ speak: function() { ... }
          ├─ species: "Mammal"
          └─ [[Prototype]]: Object.prototype
              └─ [[Prototype]]: null
```

**Interview Tip:** *"JavaScript's prototype chain allows inheritance and property lookup. When accessing a property, JS checks the object first, then walks up the prototype chain until found or reaching null. Methods on prototypes are shared across instances for memory efficiency. Modifying a prototype affects all instances immediately."*

---

### 3.7 Event Loop Advanced

**Question:** Predict output and explain microtask vs macrotask priority.

```javascript
console.log('Script start');

async function async1() {
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}

async function async2() {
  console.log('async2');
}

setTimeout(() => {
  console.log('setTimeout');
}, 0);

async1();

new Promise(resolve => {
  console.log('Promise');
  resolve();
}).then(() => {
  console.log('Promise then');
});

console.log('Script end');
```

**Output:**
```
Script start
async1 start
async2
Promise
Script end
async1 end
Promise then
setTimeout
```

**Explanation:**
1. **Sync:** "Script start"
2. **Sync:** Call async1 → "async1 start"
3. **Sync:** Call async2 → "async2"
4. **Async:** await pauses async1, queues continuation as microtask
5. **Sync:** Promise executor runs → "Promise"
6. **.then()** queues microtask
7. **Sync:** "Script end"
8. **Microtasks:** "async1 end", "Promise then"
9. **Macrotask:** "setTimeout"

**Key Points:**
- `async/await` is syntactic sugar over promises
- `await` queues the rest of the function as a microtask
- Promise executor runs synchronously
- Microtasks (promises) run before macrotasks (setTimeout)

**Interview Tip:** *"The async function pauses at await and queues its continuation as a microtask. The Promise executor runs synchronously—only .then() callbacks are async. After synchronous code completes, all microtasks run before the first macrotask, which is why both promise callbacks execute before setTimeout despite 0 delay."*

---

## 4. COMPREHENSIVE MOCK INTERVIEWS

### 4.1 Junior Level Interview (30 minutes)

**Round 1: Fundamentals (10 min)**

**Q1:** Explain the difference between `let`, `const`, and `var`.

**Expected Answer:**
"var is function-scoped and hoisted with undefined initialization. let and const are block-scoped and hoisted but remain in the Temporal Dead Zone until their declaration line. const prevents reassignment but objects can still be mutated. I use const by default, let when reassignment is needed, and avoid var in modern code."

**Q2:** What is a closure? Give an example.

**Expected Answer:**
"A closure is a function that remembers variables from its outer scope even after the outer function has returned. Common use cases include data privacy and function factories."

```javascript
function createCounter() {
  let count = 0;
  return {
    increment: () => ++count,
    getCount: () => count
  };
}

const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
```

**Q3:** Explain event delegation.

**Expected Answer:**
"Event delegation attaches a single event listener to a parent element instead of multiple listeners on children. It uses event bubbling—when a child element triggers an event, it bubbles up to the parent. This is more efficient and handles dynamically added elements."

```javascript
document.getElementById('list').addEventListener('click', (e) => {
  if (e.target.matches('li')) {
    console.log('Clicked:', e.target.textContent);
  }
});
```

**Round 2: Coding (15 min)**

**Q4:** Write a function to debounce an input handler.

```javascript
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// Usage
const handleInput = debounce((value) => {
  console.log('Searching:', value);
}, 300);
```

**Q5:** Implement `Array.prototype.filter`.

```javascript
Array.prototype.myFilter = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};

// Test
[1, 2, 3, 4].myFilter(x => x > 2); // [3, 4]
```

**Round 3: Conceptual (5 min)**

**Q6:** What happens when you type a URL in the browser?

**Expected:** DNS lookup → TCP connection → HTTP request → Server processing → Response → Parsing HTML → Loading resources → Rendering → JavaScript execution

---

### 4.2 Mid-Level Interview (45 minutes)

**Round 1: Deep Concepts (15 min)**

**Q1:** Explain the event loop and task queues.

**Expected Answer:**
"JavaScript has a call stack for synchronous code, a microtask queue for promises, and a macrotask queue for setTimeout/setInterval. The event loop processes:
1. All synchronous code on call stack
2. All microtasks (promises, queueMicrotask)
3. One macrotask (setTimeout, setInterval, I/O)
4. Repeat

Microtasks always run before the next macrotask, which is why promise callbacks execute before setTimeout even with 0 delay."

**Q2:** How does prototypal inheritance work?

**Expected Answer:**
"JavaScript objects inherit from other objects via the [[Prototype]] chain. When accessing a property, JS looks on the object first, then walks up the prototype chain. Functions have a prototype property that becomes the [[Prototype]] of instances created with new. Classes are syntactic sugar over this mechanism."

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  return `${this.name} speaks`;
};

const dog = new Animal('Rex');
dog.speak(); // "Rex speaks"

console.log(dog.__proto__ === Animal.prototype); // true
```

**Q3:** Explain different ways `this` is determined.

**Expected Answer:**
1. Method call: `obj.method()` → this is obj
2. Regular function: `func()` → this is window/undefined
3. Arrow function: Lexical this from enclosing scope
4. Constructor: `new Func()` → this is new instance
5. Explicit: `call/apply/bind` → this is provided argument

**Round 2: Coding Challenges (20 min)**

**Q4:** Implement Promise.all.

```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError('Argument must be an array'));
    }

    if (promises.length === 0) {
      return resolve([]);
    }

    const results = [];
    let completed = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(value => {
          results[index] = value;
          completed++;

          if (completed === promises.length) {
            resolve(results);
          }
        })
        .catch(reject);
    });
  });
}
```

**Q5:** Implement a function to deep clone an object.

```javascript
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (hash.has(obj)) return hash.get(obj);

  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);

  const clone = Array.isArray(obj) ? [] : {};
  hash.set(obj, clone);

  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key], hash);
    }
  }

  return clone;
}
```

**Round 3: System Design (10 min)**

**Q6:** Design an autocomplete component. What are the key considerations?

**Expected Answer:**
- Debouncing to reduce API calls
- Cancel previous requests (AbortController)
- Keyboard navigation (arrow keys, enter, escape)
- Accessibility (ARIA labels)
- Loading states and error handling
- Highlight matching text
- Handle edge cases (empty input, no results)

---

### 4.3 Senior Level Interview (60 minutes)

**Round 1: Architecture & Performance (20 min)**

**Q1:** How would you optimize a slow React application?

**Expected Answer:**
1. Identify bottlenecks with React DevTools Profiler
2. Memoization: React.memo, useMemo, useCallback
3. Code splitting with React.lazy and Suspense
4. Virtualization for long lists (react-window)
5. Optimize re-renders (proper key usage, state structure)
6. Bundle size analysis and tree-shaking
7. Lazy load images and resources
8. Use Web Workers for heavy computations

**Q2:** Explain memory leaks in JavaScript and prevention strategies.

**Expected Answer:**
"Memory leaks occur when objects remain in memory unnecessarily:
1. Forgotten timers/intervals → Always clear them
2. Event listeners on removed elements → Remove listeners
3. Closures capturing large data → Minimize scope
4. Global variables → Avoid or use WeakMap
5. Detached DOM nodes → Clear references

Prevention:
- Use WeakMap/WeakSet for caches
- Implement cleanup in useEffect/componentWillUnmount
- Profile with Chrome DevTools Memory tab
- Use heap snapshots to identify leaks"

**Q3:** Design a state management system. What principles would you follow?

**Expected Answer:**
- Single source of truth
- Immutable updates
- Predictable state transitions
- Separation of concerns (actions, reducers, selectors)
- Middleware for async logic
- DevTools integration
- Time-travel debugging support

**Round 2: Advanced Coding (25 min)**

**Q4:** Implement an LRU Cache with O(1) operations.

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }

  get(key) {
    if (!this.cache.has(key)) return -1;

    const value = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, value);
    return value;
  }

  put(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key);
    }

    this.cache.set(key, value);

    if (this.cache.size > this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
  }
}
```

**Q5:** Implement a function to flatten an object.

```javascript
function flattenObject(obj, prefix = '', result = {}) {
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      const newKey = prefix ? `${prefix}.${key}` : key;

      if (typeof obj[key] === 'object' && obj[key] !== null && !Array.isArray(obj[key])) {
        flattenObject(obj[key], newKey, result);
      } else {
        result[newKey] = obj[key];
      }
    }
  }

  return result;
}

// Test
const nested = {
  a: 1,
  b: { c: 2, d: { e: 3 } },
  f: [4, 5]
};

console.log(flattenObject(nested));
// { 'a': 1, 'b.c': 2, 'b.d.e': 3, 'f': [4, 5] }
```

**Round 3: System Design (15 min)**

**Q6:** Design a real-time collaborative text editor (like Google Docs).

**Expected Answer:**

**Architecture:**
1. WebSocket connection for real-time updates
2. Operational Transformation (OT) or CRDT for conflict resolution
3. Cursor position synchronization
4. Presence indicators
5. Version history with snapshots
6. Offline support with local storage
7. Conflict resolution strategy

**Data Structure:**
- Text represented as array of characters or spans
- Each operation: { type, position, content, userId, timestamp }
- Transform operations to handle concurrent edits

**Scalability:**
- Use Redis for pub/sub across servers
- Persist to database periodically
- Implement room-based connections
- Rate limiting and auth

**Challenges:**
- Race conditions in concurrent edits
- Network latency and disconnections
- Large document performance
- Cursor position accuracy during edits

---

## 5. SYSTEM DESIGN QUESTIONS

### 5.1 Infinite Scroll News Feed

**Question:** Design an infinite scroll news feed like Twitter/Facebook.

**Requirements:**
- Load posts as user scrolls
- Real-time updates for new posts
- Optimistic UI updates
- Handle poor network conditions
- Memory management for long sessions

**Solution Architecture:**

```javascript
class NewsFeedManager {
  constructor(options) {
    this.container = options.container;
    this.fetchPosts = options.fetchPosts;
    this.pageSize = 20;
    this.currentPage = 1;
    this.isLoading = false;
    this.hasMore = true;
    this.posts = [];
    this.wsConnection = null;

    this.init();
  }

  init() {
    // Initial load
    this.loadMore();

    // Infinite scroll
    this.setupInfiniteScroll();

    // Real-time updates
    this.setupWebSocket();

    // Visibility change handling
    document.addEventListener('visibilitychange', () => {
      if (!document.hidden) {
        this.refreshFeed();
      }
    });
  }

  setupInfiniteScroll() {
    const observer = new IntersectionObserver(
      (entries) => {
        if (entries[0].isIntersecting && !this.isLoading && this.hasMore) {
          this.loadMore();
        }
      },
      { rootMargin: '200px' }
    );

    const sentinel = document.createElement('div');
    this.container.appendChild(sentinel);
    observer.observe(sentinel);
  }

  async loadMore() {
    if (this.isLoading || !this.hasMore) return;

    this.isLoading = true;
    this.showLoader();

    try {
      const newPosts = await this.fetchPosts(this.currentPage, this.pageSize);

      if (newPosts.length < this.pageSize) {
        this.hasMore = false;
      }

      this.posts = [...this.posts, ...newPosts];
      this.renderPosts(newPosts);
      this.currentPage++;

      // Memory management: Remove old posts if too many
      if (this.posts.length > 200) {
        this.pruneOldPosts();
      }
    } catch (error) {
      this.handleError(error);
    } finally {
      this.isLoading = false;
      this.hideLoader();
    }
  }

  setupWebSocket() {
    this.wsConnection = new WebSocket('wss://api.example.com/feed');

    this.wsConnection.onmessage = (event) => {
      const newPost = JSON.parse(event.data);
      this.prependPost(newPost);
    };

    this.wsConnection.onerror = () => {
      // Fallback to polling
      this.startPolling();
    };
  }

  prependPost(post) {
    this.posts.unshift(post);
    const element = this.createPostElement(post);
    this.container.insertBefore(element, this.container.firstChild);

    // Animate entrance
    element.classList.add('fade-in');
  }

  pruneOldPosts() {
    // Keep first 100, remove rest from DOM
    const toRemove = this.posts.slice(100);
    toRemove.forEach(post => {
      const element = document.getElementById(`post-${post.id}`);
      if (element) element.remove();
    });

    this.posts = this.posts.slice(0, 100);
  }

  // Optimistic updates for likes
  async toggleLike(postId) {
    const post = this.posts.find(p => p.id === postId);
    if (!post) return;

    // Optimistic update
    post.liked = !post.liked;
    post.likeCount += post.liked ? 1 : -1;
    this.updatePostUI(post);

    try {
      await fetch(`/api/posts/${postId}/like`, {
        method: post.liked ? 'POST' : 'DELETE'
      });
    } catch (error) {
      // Rollback on failure
      post.liked = !post.liked;
      post.likeCount += post.liked ? 1 : -1;
      this.updatePostUI(post);
      this.showError('Failed to update like');
    }
  }

  // Handle offline/online
  setupNetworkHandling() {
    window.addEventListener('online', () => {
      this.refreshFeed();
      this.syncPendingActions();
    });

    window.addEventListener('offline', () => {
      this.showOfflineIndicator();
    });
  }
}
```

**Key Considerations:**
1. **Performance:**
   - Virtual scrolling for large lists
   - Image lazy loading
   - Debounced scroll handling
   - Memory cleanup (prune old posts)

2. **Real-time:**
   - WebSocket for instant updates
   - Polling fallback
   - Optimistic UI updates

3. **Network:**
   - Retry failed requests
   - Cache responses
   - Offline support
   - Progressive loading

4. **UX:**
   - Loading states
   - Error handling
   - Smooth animations
   - Pull-to-refresh

**Interview Tip:** *"For an infinite scroll feed, I'd use Intersection Observer for efficient scroll detection, WebSocket for real-time updates with polling fallback, and implement optimistic UI updates for instant feedback. Memory management is crucial—I'd prune old posts beyond a threshold and use virtual scrolling for large lists. Network resilience with retries and offline support ensures a smooth experience."*

---

## 6. PERFORMANCE OPTIMIZATION

### 6.1 Bundle Size Optimization

**Strategies:**

```javascript
// 1. Code Splitting
// Before: Single bundle (500KB)
import { HeavyComponent } from './HeavyComponent';

// After: Dynamic import (50KB initial)
const HeavyComponent = lazy(() => import('./HeavyComponent'));

// 2. Tree Shaking
// Before: Import entire library
import _ from 'lodash';

// After: Import specific functions
import debounce from 'lodash/debounce';
// Or use lodash-es for better tree shaking

// 3. Remove unused dependencies
// Analyze with webpack-bundle-analyzer
npm run build -- --analyze

// 4. Replace heavy libraries
// Before: moment.js (70KB)
import moment from 'moment';

// After: date-fns (modular, 10KB)
import { format } from 'date-fns';

// 5. Use lighter alternatives
// Before: axios (13KB)
// After: fetch API (built-in, 0KB)

// 6. Lazy load images
<img
  loading="lazy"
  src="image.jpg"
  alt="Description"
/>

// 7. Preload critical resources
<link rel="preload" href="critical.js" as="script" />

// 8. Service Worker caching
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
```

**Measuring Impact:**
```javascript
// Lighthouse audit
npm install -g lighthouse
lighthouse https://yoursite.com --view

// Bundle analysis
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin()
  ]
};

// Performance monitoring
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log(`${entry.name}: ${entry.duration}ms`);
  }
});

observer.observe({ entryTypes: ['measure'] });

performance.mark('start-operation');
// ... operation ...
performance.mark('end-operation');
performance.measure('operation', 'start-operation', 'end-operation');
```

---

### 6.2 Rendering Performance

```javascript
// 1. Virtualization for long lists
import { FixedSizeList } from 'react-window';

function LongList({ items }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {({ index, style }) => (
        <div style={style}>
          {items[index]}
        </div>
      )}
    </FixedSizeList>
  );
}

// 2. Debounce expensive operations
const expensiveSearch = debounce((query) => {
  // Heavy computation
  const results = performSearch(query);
  updateUI(results);
}, 300);

// 3. RequestAnimationFrame for animations
function animate() {
  // Update animation
  updatePosition();

  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);

// 4. Web Workers for heavy computation
// main.js
const worker = new Worker('worker.js');

worker.postMessage({ data: largeDataset });

worker.onmessage = (e) => {
  console.log('Result:', e.data);
};

// worker.js
self.onmessage = (e) => {
  const result = performHeavyComputation(e.data);
  self.postMessage(result);
};

// 5. Intersection Observer for lazy loading
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      imageObserver.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  imageObserver.observe(img);
});
```

---

## COMPLETION CHECKPOINT ✅

Congratulations! You now have a comprehensive JavaScript interview preparation guide covering:

✅ **Frontend Challenges:** Todo app, debounce/throttle, infinite scroll, autocomplete
✅ **Algorithms:** String manipulation, arrays, trees, intervals
✅ **Tricky Questions:** Execution context, this binding, closures, coercion, async patterns
✅ **Mock Interviews:** Junior, Mid, Senior level scripts
✅ **System Design:** News feed, real-time features
✅ **Performance:** Bundle optimization, rendering efficiency

---

## Your Next Steps:

1. **Practice Coding:** Spend 30-60 minutes daily solving problems
2. **Build Projects:** Implement the frontend challenges
3. **Review Gotchas:** Master the tricky questions section
4. **Mock Interviews:** Practice explaining concepts out loud
5. **Deep Dive:** Pick one weak area and study it intensively

**Ready to continue?**

Tell me:
- "Practice [topic]" - Get hands-on exercises
- "Quiz me on [topic]" - Test your knowledge
- "Explain [concept] deeper" - Get more detailed explanation
- "Mock interview [level]" - Simulate interview session

What would you like to focus on? 🚀
