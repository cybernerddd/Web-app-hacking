# 🧠 JavaScript Basics – Part 1

This note covers my foundational understanding of JavaScript for hacking, automation, and browser manipulation.

---

## 📝 Comments

```javascript
// This is a single-line comment
/* This is
   a multi-line comment */

## Output to screen
console.log("Hello, Cybernerddd");


let sum = 5 + 3;      // Addition
let diff = 10 - 2;    // Subtraction
let prod = 4 * 2;     // Multiplication
let div = 20 / 5;     // Division
let mod = 10 % 3;     // Modulo (remainder)
```

## `String concatenation`
```javascript
let name = "Cyber" + "nerddd";  // "Cybernerddd"

## string propertoes and methods
"Cybernerddd".length  // 11
"hack".toUpperCase();  // "HACK"
"   padded   ".trim();  // "padded"

```

## `Math Object`
```javascript
*commom methods*
Math.floor(4.9);    // 4
Math.ceil(4.1);     // 5
Math.random();      // 0.X (random number between 0 and 1)
Math.round(4.6);    // 5

**Random number between 1-10**
Math.floor(Math.random() * 10) + 1;
```

# 🔢 JavaScript Variables

## 📦 Declaring Variables


### `Declaring Variables`
```javascript
var hacker = "Cybernerddd";

**let**
let target = "example.com"; //Modern, block-scoped. Can be updated but not redeclared

**const**
const tool = "Burp Suite";
```

## Mathematical Assignment Operators
```javascript
- +=
- -=
- *=
- /=

## `Increment and Decrement Operators`
- We use the ++ to increment a variable by 1 //example
let a = 20;
a++;
console.log(a) ///This will give you 21 as output
- And -- to decrease a var by 1 //Example
let b = 10;
b--;
console.log(b); ///This will present the output 9

```

## `String Interpolation`
```javascript
We can insert, or interpolate, variables into strings using template literals.
//Example case
*const myName = 'cybernerddd';*
///Now i was to add this var to a sentence for instance;
'Pleasure to meet you cybernerddd.'
**Your log**
console.log(`Pleasure to meet you ${myName}.`);
```

```javascript
## `Typeof operator`
The typeof operator checks the value to its right and returns, or passes back, 
a string of the data type.

const month1 = 'january';
console.log(typeof month1); // Output: string

const month3 = true; 
console.log(typeof month3); // Output: boolean
