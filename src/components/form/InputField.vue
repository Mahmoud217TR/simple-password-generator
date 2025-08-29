<template>
  <input v-bind="$attrs"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
    class="border border-neutral-500 px-2 py-1 rounded-md disabled:bg-neutral-300 disabled:cursor-not-allowed"
  />
</template>

<script>
export default {
  name: "InputField",
  props: {
    modelValue: {
      default: ""
    }
  },
  emits: ["update:modelValue"]
}
</script>

<style>

</style>
