> # ES6 Getters and Setters

We know that we can create objects holding methods like:

```javascript
let new_obj = {
  name: 'Roland',
  last_name: 'Doda',
  get_full_name: function () {
    return this.name + ' ' + this.last_name
  }
}

console.log(new_obj.get_full_name()) //=> "Roland Doda"
```

As you can see, calling the method `get_full_name()` will give us the result of this method.The downside of this is that we have to use <u>**parenthesis**</u>.Fortunately, `javascript` offers **getters**:

```javascript
let new_obj = {
  name: 'Roland',
  last_name: 'Doda',
  get get_full_name () {
    return this.name + ' ' + this.last_name
  },
}

console.log(new_obj.get_full_name) // => "Roland Doda"
```

Now the `get_full_name` property of `new_obj` which is actually a **method** returns the concatenation of `name` and `last_name`.

**NOTE:** this is reactive,so if you change the `name` property and then call the `get_full_name` the last one will return the updated `name`.

But wouldn't be great to use another method to change the `name` and `last_name`?Javascript offers **setters**:

```javascript
function create_person (name, last_name) {
  return {
    name: name,
    last_name: last_name,
    get full_name () { //=> getter
      return `${this.name} ${this.last_name}`
    },
    set full_name (new_name) { //=>setter
      this.name = new_name.split(' ')[0]
      this.last_name = new_name.split(' ')[1]      
    }
  }
}

let person = new create_person('Roland', 'Doda') //=> creating new object
console.log(person.full_name); //=> "Roland Doda"

person.full_name = 'Mariglen Njeshi' //=> changing full name

console.log(person.full_name); //"Mariglen Njeshi"
```



Please take a look to [this](https://jsfiddle.net/Roland1993/0xc371tp/"Getters Setters example")  example.