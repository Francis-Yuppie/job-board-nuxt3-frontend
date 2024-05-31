<script setup>

const {data: jobs} = useFetch("http://127.0.0.1:8000/api/v1/jobs/new")
</script>
<template>
  <div>
    <div class="py-20 px-6 text-center bg-teal-700">
      <h1 class="mb-12 text-5xl text-white font-extrabold">
        Find A Job Anywhere
      </h1>

      <NuxtLink
        to="/browse"
        class="inline-block py-4 px-8 bg-teal-900 text-white rounded-xl hover:bg-teal-500"
        >Browse</NuxtLink
      >
    </div>
    <div class="py-10 px-6">
      <h2 class="mb-8 text-2xl text-center text-neutral-50 font-bold">
        New Jobs
      </h2>

      <div class="space-y-4">
        <jobsCard v-for="job in jobs" :key="job.id" :job="job"/>
      </div>
    </div>
  </div>
</template>