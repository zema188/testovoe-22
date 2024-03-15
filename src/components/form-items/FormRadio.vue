<template>
  <fieldset>
    <legend>{{ label }}</legend>

    <div
        v-for="option in options"
        :key="option.value"
        class="option"
    >
      <input v-bind="$attrs" required type="radio" :id="`${option.value}_${formIndex}`" :value="option.value" :checked="option.selected" />
      <label :for="`${option.value}_${formIndex}`">{{ option.text }}</label>
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "FormRadio",

  props: {
    label: {
      type: String,
      default: ''
    },

    options: {
      type: Object,
      required: true
    },

    formIndex: {
      type: Number,
      required: true
    }
  },
}
</script>

<style scoped>
.option {
  display: flex;
  gap: 5px;
  align-items: center;
}
</style>
