<template>
  <div class="flex flex-col bg-white border border-neutral-500 p-4 rounded-md shadow-md shadow-neutral-400 backdrop-blur-md">
    <header>
      <slot name="header"></slot>
    </header>
    <div class="">
      <slot></slot>
    </div>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<script>
export default {
  name: "DisplayCard"
}
</script>

<style>

</style>
