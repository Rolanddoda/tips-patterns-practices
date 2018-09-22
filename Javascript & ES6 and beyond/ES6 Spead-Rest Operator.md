> Spread - Rest Operator



`...` Spread or Rest operator introduced by ES6 can be used in front of any iterable object or array and it acts to "spread" it out into its individual values. Or the opposite. Gather individual values into an array.

```javascript
let array = [1, 2, 3, 4]
console.log(array) // [1, 2, 3, 4]

//using spread operator
console.log(...array)
// 1
// 2
// 3
// 4
```

Another example using rest operator

```javascript
function example (val1, val2, val3) {
  console.log(val1, val2, val3);
}

example(1, 2, 3) // run the function example
//outputs: 
// 1
// 2
// 3

//Using rest operator

function example (...rest) { //=> we can use whatever name (eg ...val)
  console.log(rest); // [1, 2, 3]
}


example(1, 2, 3) 
```

