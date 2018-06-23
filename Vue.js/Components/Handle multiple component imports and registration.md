> ## Handle multiple component imports and registration

Sometimes may you will have a component which has a lot of children components.And it look like this:

```javascript
import component1 from '...'
import component2 from '...'
import component3 from '...'
//...many others component imports

export default {
  components: {
    component1,
    component2,
    component3
    //...
  }
}
```

Then you think "wait,it must be a shorter and cleaner way to do this".Well,there are some ways.

**I do prefer the 3rd way though**

 **1st First way**

​	Creating a "service" in my case I name it `dynamic_imports.js`,with the following content:

```javascript
export default function (config) {
    let registered_components = {}
    for (let component of config.components) {
        registered_components[component.name] = () =>                   				System.import(`../${config.path}/${component.file_name}.vue`)
    }
    return registered_components
}
```

​	Then in the components you have many component imports and registrations, you use it like:

```javascript
import dynamic_import from '@/services/dynamic_imports' //importing the above file
let components = dynamic_import({
    path: 'components/servers', //path we are looking for
    components: [
        { name: 'server-one', file_name: 'serverOne' },
        { name: 'server-two', file_name: 'serverTwo' },
    ]
})

export default {
//...other code
    components: components
}
```

**2nd way**

```javascript
function import_component(cmp_name){
    return System.import(`@/components/${cmp_name}.vue`); 
}

export default{
    components: {
        'component1': () => import_component('componentOne'),
        'component2': () => import_component('componentTwo'),
        'component3': () => import_component('componentThree')
    }
}
```

**3rd way**

`dynamic_imports.js`

```javascript
export default function ({path, file_names, component_names}) {
    let registered_components = {}
    for (let [index, file_name] of file_names.entries()) {
        registered_components[component_names[index]] = () => System.import(`../${path}/${file_name}.vue`)
    }
    return registered_components
}
```

`In your component`

```javascript
import dynamic_import from '@/services/dynamic_imports'

let components = dynamic_import({
    path: 'components/servers',
    file_names: ['serverOne', 'serverTwo'],
    component_names: ['server-one', 'server-two']
})

export default {
    components: components
}
```

