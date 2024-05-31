<script setup lang="ts">
const route = useRoute();
console.log(route.params);

//const {data: job} = await useFetch(`http://127.0.0.1:8000/api/v1/jobs/${route.params.id}`)

const { data: job } = await useFetch(
  "http://127.0.0.1:8000/api/v1/jobs/" + route.params.id
);

</script>

<template>
  <div class="py-10 px-6 grid md:grid-cols-4 gap-3">
    <div class="md:col-span-3">
      <h3 class="mb-6 text-3xl text-cyan-500 font-bold">
        TITLE: <br>{{ job.title }}
      </h3>
      <p class="text-white">DESCRIPTION: <br> {{ job.description }}</p>
      <a
        :href="`mailto: ${job.company_email}`"
        class="mt-6 inline-block px-6 py-4 bg-teal-700 text-white rounded-xl"
        >Apply Now</a
      >
    </div>

    <div class="md:col-span-1 p-6 text-white rounded-xl bg-teal-700">
      <h3 class="mb-6">Company</h3>
      <p class="mb-2">{{ job.company_name }}</p>
      <p class="text-white">Company Location: {{ job.company_location }}</p>

      <hr class="my-6 opacity-30" />

      <h3 class="mb-4">Position</h3>
      <p class="mb-2">{{ job.position_location }}</p>
      <p class="text-white">Salary: KSH {{ job.position_salary }}</p>

      <hr class="my-6 opacity-30" />
      <p class="">Posted on: {{ job.created_at_formatted }}</p>
    </div>
  </div>
</template>