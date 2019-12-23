<template>
  <b-col class="validation-messages">
    <div v-for="(v) in validations" :key="v.key">
      <b-form-invalid-feedback :state="v.state">
        {{messages(v)}}
      </b-form-invalid-feedback>
    </div>
  </b-col>
</template>

<script>
  const parseMessage = function(message, key) {
    if (!message || typeof message === 'string') {
      return message;
    } else {
      return message[key];
    }
  };

  export default {
    name: 'validation-message',
    
    props: ['property', 'message'],

    data() {
      return {
        validations: [],
        messages(validation) {
          // objeto que contém as mensagens de validação
          // caso não esteja especificado, retorna mensagem parametrizada ou padrão required
          return {
            required: 'Deve ser preenchido',
            minLength: `Deve ter no mínimo ${validation.min} caracteres`,
            maxLength: `Deve ser menor ou igual a ${validation.max}`,
            email: 'Deve ser um email válido',
            minValue: `Deve ser maior ou igual a ${validation.min}`,
          }[validation.key] || parseMessage(this.message, validation.key) || 'Deve ser preenchido';
        }
      }
    },

    methods: {
      // Monta o objeto com as propriedades usadas na mensagem.
      validate() {
        this.validations = [];
        let prop = this.property || {};
        for (let key in prop['$params']) {
          this.validations.push({
            key: key,
            state: prop[key],
            min: prop && prop['$params'] && prop['$params'][key] ? prop['$params'][key]['min'] : null,
            max: prop && prop['$params'] && prop['$params'][key] ? prop['$params'][key]['max'] : null
          });
        }
      }
    },

    watch: {
      // Ativada quando há alteração nos campos validados.
      'property.$model'() {
        this.validate();
      }
    }
  };

  export function cnpjValidator(cnpj) {
    if(!cnpj) { return false; }
    const cnpjsConhecidos = ['00000000000000', '11111111111111', '22222222222222', '33333333333333', '44444444444444', '55555555555555', '66666666666666', '77777777777777', '88888888888888', '99999999999999'];
    cnpj = cnpj.replace(/[^\d]+/g,'');
    if(!cnpj) { return false }
    if (cnpj.length != 14) { return false; }
    // Elimina CNPJs inválidos conhecidos
    if (cnpjsConhecidos.includes(cnpj)) { return false; }
    // Valida DVs
    let tamanho = cnpj.length - 2
    let numeros = cnpj.substring(0,tamanho);
    let digitos = cnpj.substring(tamanho);
    let soma = 0;
    let pos = tamanho - 7;
    for (let i = tamanho; i >= 1; i--) {
      soma += numeros.charAt(tamanho - i) * pos--;
      if (pos < 2) { pos = 9; }
    }
    let resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
    if (resultado != digitos.charAt(0)) { return false; }
    tamanho = tamanho + 1;
    numeros = cnpj.substring(0,tamanho);
    soma = 0;
    pos = tamanho - 7;
    for (let i = tamanho; i >= 1; i--) {
      soma += numeros.charAt(tamanho - i) * pos--;
      if (pos < 2) { pos = 9; }
    }
    resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
    if (resultado != digitos.charAt(1)) { return false; }
    return true;
  }

  export function cpfValidator(cpf) {
    if (!cpf) { return false; }
    let numeros, digitos, soma, i, resultado, digitos_iguais = 1;
    if (cpf.length < 11) { return false; }
    for (i = 0; i < cpf.length - 1; i++)
      if (cpf.charAt(i) != cpf.charAt(i + 1)) {
        digitos_iguais = 0;
        break;
      }
    if (!digitos_iguais) {
      numeros = cpf.substring(0,9);
      digitos = cpf.substring(9);
      soma = 0;
      for (i = 10; i > 1; i--) { soma += numeros.charAt(10 - i) * i; }
      resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
      if (resultado != digitos.charAt(0)) { return false; }
      numeros = cpf.substring(0,10);
      soma = 0;
      for (i = 11; i > 1; i--) { soma += numeros.charAt(11 - i) * i; }
      resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
      if (resultado != digitos.charAt(1)) { return false; }
      return true;
    } else {
      return false;
    }
  }
</script>

<!-- validation-message Component
  Componente responsável pelas mensagens de validação mostradas abaixo do campo que está sendo validado.
  Deverá ser chamado logo abaixo do campo.
  Recebe como parâmetro a propriedade do validador (vuelidate) que está sendo verificada.
 -->