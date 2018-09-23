> ## Learn about `call, apply, bind` in Javascript

The three methods accepting as first parameter a value that inside the function will be accessible by using `this` context. For example using `call`:



```javascript
let context = {
	name: 'Landi'    
}

function change_name (new_name) {
    this.name = new_name //=> this reffers to context object
}

change_name.call(context, 'name changed')

console.log(context.name); //outputs: "name changed"
```

**Note:**  the function `change_name` didn't accept as argument the `this` context. This is done automatically by JS and giving us the ability to access the context passed using `this` keyword.



> ### Differences between 3 methods:



- **Call** invokes the function immediately and allows you to pass arguments **one by one**.
- **Apply** is similar to `call` but  allows you to pass an array of arguments.
- **Bind** does NOT invokes immediately the function. Instead it returns the function, allowing you to save that function into a variable and call it later passing arguments one by one. It is useful when using event listeners.



> ####  Example using the `apply` method



```javascript
let context = {
	name: 'Landi',
    last_name: 'Doda'
}

function change_name (new_name, new_last_name) {
    this.name = new_name 
    this.last_name = new_last_name
}

change_name.apply(context, ['name changed', 'last name changed'])

console.log(context);

/*
 outputs:
 
 {
  last_name:"last name changed"
  name:"name changed"
 }
 
*/
```



> #### Example using the `bind` method



```javascript
let context = {
	name: 'Landi',
    last_name: 'Doda'
}

function change_name (new_name, new_last_name) {
    this.name = new_name 
    this.last_name = new_last_name
}

let bound = change_name.bind(context)

bound('name changed', 'last name changed')

console.log(context);

/*
 outputs:
 
 {
  last_name:"last name changed"
  name:"name changed"
 }
 
*/
```

