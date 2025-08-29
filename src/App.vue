<script setup>
  import PasswordGenerator from './components/PasswordGenerator.vue'
  import DisplayCard from './components/DisplayCard.vue'
</script>

<template>
  <header class="h-20">

  </header>

  <main class="w-full container mx-auto my-8">
    <DisplayCard class="max-w-2xl mx-auto">
      <template #header>
        <h1 class="text-center text-3xl font-bold">
          Passowrd Generator
        </h1>
        <hr class="mt-4 mb-8 border-neutral-500">
      </template>
      <PasswordGenerator class="p-4" />
    </DisplayCard>
  </main>

  <footer>

  </footer>
</template>

<style scoped>

</style>
