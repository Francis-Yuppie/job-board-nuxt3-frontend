<script setup>
import { useUserStore } from "~/stores/user";

const userStore = useUserStore();

const logOut = () => {
  userStore.removeToken();

  useRouter().push("/");
};
</script>

<template>
  <nav class="p-6 flex items-center justify-between bg-teal-800">
    <NuxtLink to="/" class="text-xl text-white">YJOB</NuxtLink>
    <div class="flex items-center space-x-4">
      <NuxtLink to="/" class="text-white hover:text-teal-400">Home</NuxtLink>
      <NuxtLink to="/browse" class="text-white hover:text-teal-400"
        >Browse</NuxtLink
      >
      <template v-if="userStore.user.isAuthenticated">
        <NuxtLink
          to="/myjobs"
          class="py-4 px-6 bg-slate-900 hover:bg-teal-700 text-white rounded-xl"
          >My Jobs</NuxtLink
        >
        <NuxtLink
          to="/createJob"
          class="py-4 px-6 bg-slate-800 hover:bg-teal-700 text-white rounded-xl"
          >Create Job</NuxtLink
        >
        <a
          @click="logOut"
          class="py-4 px-6 bg-rose-700 hover:bg-teal-700 text-white rounded-xl hover:cursor-pointer"
          >Log out</a
        >
      </template>

      <template v-else>
        <NuxtLink
          to="/login"
          class="py-4 px-6 bg-slate-900 hover:bg-teal-700 text-white rounded-xl"
          >Login</NuxtLink
        >
        <NuxtLink
          to="/signup"
          class="py-4 px-6 bg-slate-800 hover:bg-teal-700 text-white rounded-xl"
          >Sign up</NuxtLink
        >
      </template>
    </div>
  </nav>
</template>

<style scoped></style>