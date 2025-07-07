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
# `Else If statement`
```javascript
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
--------------------
So with this, using a ternary operator will be:
nightAtNight ? console.log('Go to bed')
: console.log('Its not bed time yet');

So you give the condition, followed by a question mark ? ,then block of code; which will be run if the
condition given is true, then followed with a a separation colon for the false statement.
 Just like if...else statemenst, the ternary operator ternary operators 
can be used for conditions which evaluate to true or false.
```
## `switch`
```javascript
We use this when we have to check multiple values and handle each of them differently. We can use the else if statement but 
Imagine if we needed to check 100 different values! Having to write that many else if statements sounds like a pain and shit lol.
Sow we use the switch. Example of a switch statement:

let groceryItem = 'papaya';

switch (groceryItem) {
  case 'tomato':
    console.log('Tomatoes are $0.49');
    break;
  case 'lime':
    console.log('Limes are $1.49');
    break;
  case 'papaya':
    console.log('Papayas are $1.29');
    break;
  default:
    console.log('Invalid item');
    break;
}

// Prints 'Papayas are $1.29'
```

## ARRAYS

```js
let payloads = ["<script>alert(1)</script>", "' OR 1=1 --", "../../../etc/passwd"];
console.log(payloads[0]);           // Access first item
payloads.push("newPayload");        // Add item
payloads[1] = "' OR 'a'='a";        // Modify item
```
## `LOOPS (for / while)`
```js
for (let i = 0; i < payloads.length; i++) {
  console.log("Trying: " + payloads[i]);
}

## While Loop:
let count = 0;
while (count < 3) {
  console.log("Try: " + count);
  count++;
}
```
## ITERATORS (forEach, map)
```js
forEach:
payloads.forEach(function(p) {
  console.log("Inject: " + p);
});

map = modify/transform array:
let encoded = payloads.map(function(p) {
  return encodeURIComponent(p);
});
console.log(encoded);

```
## OBJECTS
```js
let user = {
  username: "admin",
  password: "123456",
  isAdmin: true
};

## Access / Modify / Add / Delete:
console.log(user.username);     // Access
user.password = "root123";      // Modify
user.email = "admin@site.com";  // Add
delete user.isAdmin;            // Delete

## Loop through object:
for (let key in user) {
  console.log(key + ": " + user[key]);
}
## OBJECTS INSIDE ARRAYS:
let creds = [
  { username: "admin", password: "123456" },
  { username: "guest", password: "guest123" }
];

creds.forEach(cred => {
  console.log("Trying: " + cred.username + " / " + cred.password);
});
```

## Nested Loops Comparing Arrays
```js
Find mutual items between 2 arrays:
let bobsFollowers = ["Sam", "John", "Alex"];
let tinasFollowers = ["Alex", "Zoe", "Sam"];
let mutualFollowers = [];

for (let i = 0; i < bobsFollowers.length; i++) {
  for (let j = 0; j < tinasFollowers.length; j++) {
    if (bobsFollowers[i] === tinasFollowers[j]) {
      mutualFollowers.push(bobsFollowers[i]);
    }
  }
}

console.log(mutualFollowers); // ["Sam", "Alex"]

```


