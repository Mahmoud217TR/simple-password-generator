<template>
  <button v-bind="$attrs" class="cursor-pointer bg-red-600 hover:bg-red-700 text-white">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "RegularButton"
}
</script>

<style>

</style>
