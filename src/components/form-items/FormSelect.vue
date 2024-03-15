<template>
  <div>
    <p class="label">{{ label }}</p>
    <select v-bind="$attrs">
      <option
        v-for="option in options"
        :key="option.value"
        :selected="option.selected"
      >{{ option.text }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "FormSelect",

  props: {
    label: {
      type: String,
      default: ''
    },

    options: {
      type: Object,
      required: true
    }
  },
}
</script>

<style scoped>

</style>
