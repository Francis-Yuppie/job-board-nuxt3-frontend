<script setup>
import {useUserStore} from '~/stores/user'

const userStore = useUserStore()
const emit = defineEmits(["deleteJob"]);

const props = defineProps({
  my: {
    type: [Boolean],
  },
  job: {
    type: [Object],
  },
});

const deleteJob = async (id) => {
  await $fetch(`http://127.0.0.1:8000/api/v1/jobs/${id}/delete`, {
    method: "DELETE",
    //@ts-ignore
    headers: {
      Authorization: "token " + userStore.user.token,
      "Content-Type": "application/json",
    },
  })
    .then((res) => {
      console.log(res);

      emit("deleteJob", id);
    })
    .catch((err) => {
      console.error(err);
    });
};
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
      <p>Posted {{ job.created_at_formatted }}</p>
    </div>

    <div class="space-x-4">
      <NuxtLink
        :to="`/browse/${job.id}`"
        class="py-4 px-6 bg-teal-600 text-white rounded-xl"
        >Details</NuxtLink
      >
      <NuxtLink
        :to="`/editjob/${job.id}`"
        class="py-4 px-6 bg-cyan-700 text-white rounded-xl"
        v-if="my"
        >Update</NuxtLink
      >
      <a
        @click="deleteJob(job.id)"
        to="/browse"
        class="py-4 px-6 bg-rose-700 text-white rounded-xl"
        v-if="my"
        >Delete</a
      >
    </div>
  </div>
</template>
<style scoped>
</style>