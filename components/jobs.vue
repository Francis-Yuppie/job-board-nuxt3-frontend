<script setup>
const props = defineProps({
  my: {
    type: [Boolean],
  },
  job: {
    type: [Object],
  },
});
</script>

<template>
  <div class="p-6 flex items-center justify-between rounded-xl bg-gray-700">
    <div>
      <h3 class="mb-2 text-xl font-semibold">{{ job.title }}</h3>
      <p class="text-white">{{ job.company_name }}</p>
    </div>

    <div>
      <p class="mb-2">{{ job.position_location }}</p>
      <p>{{ job.position_salary }}</p>
    </div>
    <div>
      <p>{{ job.created_at_formatted }}</p>
    </div>

    <div class="space-x-4">
      <NuxtLink to="/browse" class="py-4 px-6 bg-teal-600 text-white rounded-xl"
        >Details</NuxtLink
      >
      <NuxtLink
        to="/browse"
        class="py-4 px-6 bg-cyan-700 text-white rounded-xl"
        v-if="my"
        >Edit</NuxtLink
      >
      <NuxtLink
        to="/browse"
        class="py-4 px-6 bg-rose-700 text-white rounded-xl"
        v-if="my"
        >Delete</NuxtLink
      >
    </div>
  </div>
</template>
<style scoped>
</style>