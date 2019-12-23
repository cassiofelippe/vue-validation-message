# vue-validation-message

## How to use
1) In the desired component, import the lib  
```js
import validation from 'vue-validation-message'
```

2) Add to the component's list  
```js
components: {
  validation
}
```

3) Show the validation message  
```html
<validation :property="$v.model.name" message="Must be filled in the correct format" v-multi-ref:validation />
```

Being:  
```property``` the vuelidate property  
```message``` the validation message    
```v-multi-ref:validation``` the reference (v-multi-ref), a trigger to activate de message

## Advanced usage
If you have multiple validations to a single property, e.g:  
```js
validations: {  
  model: {  
    name: {  
      required,  
      validFormat: function() {  
        return x === y  
      }  
    }  
  }  
}  
```
You can pass the message parameter as an object with the properties and messages:  
```html
<validation :property="$v.model.name" :message="{required: 'Please fill out this field', validFormat: 'Must be filled in the correct format'}" v-multi-ref:validation />
```
##  

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Run your tests
```
yarn run test
```

### Lints and fixes files
```
yarn run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
