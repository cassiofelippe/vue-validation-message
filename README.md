# vue-validation-message

## Como usar
1) No componente desejado, importar a lib
```js
import validation from 'vue-validation-message'
```

2) Adicionar na lista de componentes
```js
components: {
  validation
}
```

3) Chamar o componente
```html
<validation :property="$v.model.cpf" message="Deve ser preenchido no formato correto" v-multi-ref:validation />
```
```property``` a propriedate do validador (vuelidate)  
```message``` a mensagem da validação  
```v-multi-ref:validation``` a referência (v-multi-ref), um trigger para ativar a mensagem


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
