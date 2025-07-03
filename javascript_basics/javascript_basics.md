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

## String concatenation 
let name = "Cyber" + "nerddd";  // "Cybernerddd"

## string propertoes and methods
"Cybernerddd".length  // 11
"hack".toUpperCase();  // "HACK"
"   padded   ".trim();  // "padded"


## Math Object
*commom methods*
Math.floor(4.9);    // 4
Math.ceil(4.1);     // 5
Math.random();      // 0.X (random number between 0 and 1)
Math.round(4.6);    // 5

**Random number between 1-10**
Math.floor(Math.random() * 10) + 1;