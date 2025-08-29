<template>
  <label class="flex items-center gap-2 cursor-pointer select-none">
    <input
      type="checkbox"
      :checked="modelValue"
      @change="$emit('update:modelValue', $event.target.checked)"
      v-bind="$attrs"
      class="peer hidden"
    />
    <span class="h-5 w-5 flex items-center justify-center rounded border border-neutral-500 bg-white peer-checked:bg-red-600 peer-checked:border-red-600 transition-colors duration-300">
      <svg v-if="modelValue" xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white"
        fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="3">
        <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7"/>
      </svg>
    </span>
    <span class="">
      <slot></slot>
    </span>
  </label>
</template>

<script>
export default {
  name: "CheckboxField",
  props: {
    modelValue: {
      type: Boolean,
      default: false
    }
  },
  emits: ["update:modelValue"]
}
</script>
