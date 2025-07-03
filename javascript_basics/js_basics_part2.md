# 🧠 JavaScript Basics – Part 2

## Conditional Statements
### `If Statement`
```javascript
The if keyword followed by a set of parentheses () which is followed by a code block,
 or block statement, indicated by a set of curly braces {}
if (true) {
console.log('This is if statement');
}
//prints "Tjis is if statement"

Inside the parentheses (), a condition is provided that evaluates to true or false.
- If the condition evaluates to true, the code inside the curly braces {} runs, or executes.
- If the condition evaluates to false, the block won’t execute.

```

## `If....Else Statments`
```javascript
Not everytime we'll get our condition evaluate to true. It also evaluates to false,
therefore youll need to add an else statement to run a block of code whem
the condition is false

Below:
let sale = true;
sale = false;

if(sale) {
  console.log('Time to buy!');
} else {
  console.log('Time to wait for a sale.');
}
```
