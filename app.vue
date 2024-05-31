<script setup lang="ts">
import { useUserStore } from "~/stores/user";

const userStore = useUserStore();

onMounted(() => {
  userStore.initStore();
});
</script>
<template>
  <div class="">
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
