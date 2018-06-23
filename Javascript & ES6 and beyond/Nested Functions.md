> # Call a function inside another function

```javascript
function multiply (a) {
  function by (b) {
    return a * b
  }
  return by
}
```

Above,we have a function inside another function.What the above code does,is to multiply 2 numbers.So let's see 2 ways of executing the above code.

**First way:**

```javascript
console.log(multiply(2)(3)) //=> 6
```

**Second way:**

```javascript
let first = multiply(2)
let second = first(10)

console.log(second) //=> 10
```

As we know after calling a function the variables created inside will no longer exists.But as we can see this is a hack to give a longer life to a variable.

Actually there is no magic happened here.Just the `first` variable holds a function since the function `multiply` returns such one.

After,the `second` variable executes the `by` function held in `first ` variable which return the multiply of arguments **a** and **b**.