<script setup>
  import PasswordGenerator from './components/PasswordGenerator.vue';
  import DisplayCard from './components/DisplayCard.vue';
  import Toast from './components/Toast.vue';
</script>

<template>
  <header class="h-20">

  </header>

  <main class="w-full container mx-auto my-8">
    <DisplayCard class="max-w-2xl mx-auto text-gray-800">
      <template #header>
        <h1 class="text-center text-3xl font-bold">
          Simple Passowrd Generator
        </h1>
        <hr class="mt-4 mb-8 border-neutral-500">
      </template>
      <PasswordGenerator class="p-4" />
      <template #footer>
        <hr class="mt-8 mb-4 border-neutral-500">
        <p class="text-center text-lg font-semibold">
          Copyright © 2025 All Rights Reserved -
          <a href="https://github.com/Mahmoud217TR" target="__blank"
            class="hover:text-red-600 transition-colors duration-300">
            MahmoudTR
          </a>
        </p>
      </template>
    </DisplayCard>
  </main>

  <Toast/>
  <footer>

  </footer>
</template>

<style scoped>

</style>
