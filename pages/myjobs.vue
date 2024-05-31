<script setup lang="ts">

useSeoMeta({
  title: 'My Jobs',
  ogTitle: 'My Jobs',
  description: 'The jobs description page'
})

//@ts-ignore
import { onMounted } from "vue";
import { useUserStore } from "~/stores/user";

const userStore = useUserStore();
const router = useRouter();
const jobs = ref([]);

onMounted(() => {
  if (!userStore.user.isAuthenticated) {
    router.push("/login");
  } else {
    getJobs();
  }
});

const getJobs = async () => {
  await $fetch("http://127.0.0.1:8000/api/v1/jobs/myjobs", {
    //@ts-ignore
    headers: {
      Authorization: "token " + userStore.user.token,
      "Content-Type": "application/json",
    },
  })
    .then((res) => {
      //@ts-ignore
      jobs.value = res;
    })
    .catch((err) => {
      console.error(err);
    });
};

//@ts-ignore
const deleteJob = (id) => {
  console.log('id', id)

  //@ts-ignore
  jobs.value = jobs.value.filter(job => job.id !== id)

}
</script>

<template>
  <div class="py-10 px-6">
    <h1 class="mb-6 text-2xl text-white">My Jobs</h1>

    <div class="space-y-4">
      <jobsCard
        v-for="job in jobs"
        :key="job.id"
        :job="job"
        :my="true"
        v-on:deleteJob="deleteJob(id)"
      />
    </div>
    <div v-if="jobs.length === 0">
      <p class="mb-6 text-white font-bold">
        You dont have any jobs yet You can create one now
      </p>
    </div>
  </div>
</template>


<style scoped>
</style>