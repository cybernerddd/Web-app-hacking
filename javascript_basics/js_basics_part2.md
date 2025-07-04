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

`Else If statement`
Allows you to add 2 or more outcomes, cause you can add any amount of else if statement aas u want;
let stopLight = 'yellow';

if (stopLight === 'red') {
  console.log('Stop!');
} else if (stopLight === 'yellow') {
  console.log('Slow down.');
} else if (stopLight === 'green') {
  console.log('Go!');
} else {
  console.log('Caution, unknown!');
}


```
## `Comparison Operators`
```javascript
- Theyre used to compare values. some are below; Just like how we've been seeing all the time, nuin different.
Less than: <
Greater than: >
Less than or equal to: <=
Greater than or equal to: >=
Is equal to: ===
Is not equal to: !==
*10 < 12 // Evaluates to true*

## logical operators
the and operator (&&)
the or operator (||)
the not operator, otherwise known as the bang operator (!)

if (stopLight === 'green' && pedestrians === 0) {
  console.log('Go!');
} else {
  console.log('Stop');
}

-------------------------------
The ! not operator reverses, or negates, the value of a boolean:
let sleepy = false;
console.log(!sleepy); // Prints true


```
## `Ternary operators`
```javascript
We can use a ternary operator in place of an if...else statement.
For Instance: An if...else statment below:
let nightAtNight = true;
if (nightAtNight) {
console.log('Go to Bed');
} else {
console.log('Its not bed time yet');
}

So with this, using a ternary operator will be:
nightAtNight ? console.log('Go to bed')
: console.log('Its not bed time yet');

So you give the condition, followed by a question mark ? ,then block of code; which will be run if the
condition given is true, then followed with a a separation colon for the false statement.
 Just like if...else statemenst, the ternary operator ternary operators 
can be used for conditions which evaluate to true or false.
```
