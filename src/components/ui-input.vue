<template>
  <input :id="id" class="ui-input" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" v-bind="$attrs" :type="type">
</template>

<script>
export default {
  name: "ui-input",
  props: {
    id: String,
    type: Number,
    modelValue: {
      type: Number,
      default: ''
    }
  },
}
</script>

<style lang="scss" scoped>
@import "../scss/global";

.ui-input{
  font-family: 'Nekst-Black', sans-serif;
  font-style: normal;
  font-weight: 900;
  font-size: 30px;
  color: #575757;
  height: 68px;
  width: 100%;
  background-color: #F3F3F4;
  border: 2px;
  border-radius: 16px;
  padding-left: 24px;
  border: transparent;
  &[type=number] {
    -moz-appearance:textfield;
  }
  &:active{
    background-color: $white;
    border: 2px solid #F3F3F4;
  }
  &:focus {
    background-color: $white;
    border: 2px solid #F3F3F4;
    outline: none;
  }
  &::-webkit-outer-spin-button,
  &::-webkit-inner-spin-button{
    -webkit-appearance: none;
    margin: 0;
  }
}
</style>