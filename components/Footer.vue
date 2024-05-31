<script setup lang="ts">
const date = new Date().getFullYear();
</script>

<template>
  <footer class="p-6 flex flex-wrap items-center justify-between bg-gray-900">
    <p class="text-gray-300">&copy; Copyright - YJOB {{ date }}</p>
  </footer>
</template>