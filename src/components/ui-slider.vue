<template>
  <input class="slider" type="range" step="1" :value="modelValue" @input="onInput"  :style="{backgroundSize: backgroundSize}">
</template>

<script>
export default {
  name: "ui-slider",
  props: {
    modelValue: Number,
  },
  data () {
    return {
      backgroundSize: '0% 100%'
    }
  },
  emits:["update:modelValue"],
  methods: {
    onInput(e){
      this.$emit("update:modelValue",e.target.value)
      let clickedElement = e.target,
          min = clickedElement.min,
          max = clickedElement.max,
          val = clickedElement.value;

      this.backgroundSize = (val - min) * 100 / (max - min) + '% 100%';
    }
  }
}
</script>


<style scoped lang="scss">
@import "../scss/global";

.slider[type=range] {
  position: absolute;
  -webkit-appearance: none;
  display: block;
  height: 2px;
  width: 90%;
  background: #E1E1E1;
  background-image: -webkit-gradient(linear, 20% 0%, 20% 100%, color-stop(0%, $primary-color), color-stop(100%, $primary-color));
  background-image: -webkit-linear-gradient(left, $primary-color 0%,$primary-color 100%);
  background-image: -moz-linear-gradient(left, $primary-color 0%, $primary-color 100%);
  background-image: -o-linear-gradient(to right, $primary-color 0%,$primary-color 100%);
  background-image: linear-gradient(to right, $primary-color 0%,$primary-color 100%);
  background-repeat: no-repeat;
  &[type=range]:focus{
    outline: none;
  }
  &::-webkit-slider-thumb{
     height: 20px;
     width: 20px;
     background-color: $primary-color;
     border-radius: 50%;
     cursor: pointer;
     -webkit-appearance: none;
   }
  &[type=range]:focus::-webkit-slider-runnable-track{
    background: transparent;
  }
  &[type=range]::-moz-range-track{
    height: 2px;
    width: 90%;
    cursor: pointer;
    box-shadow: none;
    background: transparent;
    border-radius: 0px;
    border: none;
  }
  &[type=range]::-moz-range-thumb{
    height: 20px;
    width: 20px;
    background-color: $primary-color;
    border-radius: 50%;
    cursor: pointer;
    -webkit-appearance: none;
  }
  &[type=range]::-ms-track{
    height: 2px;
    width: 90%;
    cursor: pointer;
    box-shadow: none;
    background: transparent;
    border-radius: 0px;
    border: none;
  }
  &[type=range]::-ms-fill-lower{
    background: transparent;
    border: none;
    border-radius: 0px;
    box-shadow: none;
  }
  &[type=range]::-ms-thumb{
    box-shadow: none;
    border: 4px solid #ADD8E6;
    width: 16px;
    border-radius: 2px;
    background: #ffffff;
    cursor: pointer;
    height: 4px;
  }


}
</style>