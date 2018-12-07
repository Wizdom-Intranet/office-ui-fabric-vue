<template>
  <div class='ms-TextField' ref='textField' :class='textFieldClass'>
    <label class='ms-Label'>{{ label }}</label>
    <textarea
      v-if="type == 'multiline'"
      :placeholder='placeholder'
      class='ms-TextField-field'
      type='text'
      :value='value'
      v-bind="attrs"
      @input='updateValue'
      @change='changeEvent'
      @blur='blurEvent'
      :disabled='disabled'></textarea>
    <input
      v-else
      :placeholder='placeholder'
      class='ms-TextField-field'
      :type='inputType'
      :value='value'
      v-bind="attrs"
      @input='updateValue'
      @change='changeEvent'
      @blur='blurEvent'
      :disabled='disabled' />
  </div>
</template>
<script>
  import type from '../../mixins/props/type';
  import disabled from '../../mixins/props/disabled';

  export default {
    name: 'ou-text-field',
    inheritAttrs: false,
    mixins: [type('multiline', 'underlined'), disabled],

    props: {
      value: [String, Number],
      label: String,
      placeholder: String,
      inputType: {
        type: String,
        default: 'text',
        validator(value) {
          return ['text', 'password', 'file', 'number'].includes(value);
        }
      }
    },

    computed: {
      attrs(){return this.$attrs},
      textFieldClass() {
        return {
          [`ms-TextField--${this.type}`]: !!this.type,
          'is-disabled': this.disabled
        };
      }
    },

    mounted() {
      new this.$fabric.TextField(this.$refs.textField);
    },

    methods: {
      updateValue(event) {
        this.$emit('input', event.target.value);
      },
      blurEvent(event){
        this.$emit('blur', event.target.value);
      },
      changeEvent(event) {
        this.$emit('change', event.target.value);
      }
    }
  };
</script>
