<script setup lang="ts">
useSeoMeta({
  title: 'Sign Up',
  ogTitle: 'Sign Up',
  description: 'The Sign Up description page'
})


let email = ref("");
let password1 = ref("");
let password2 = ref("");
let errors = ref([]);

const onSubmit = async () => {
  errors.value = [];

  try {
    await $fetch("http://127.0.0.1:8000/api/v1users/", {
      method: "POST",
      body: {
        username: email.value,
        password: password1.value,
      },
    })
      .then((response) => {
        console.log(response);

        useRouter().push("/login");
      })
      .catch((err) => {
        if (err.response) {
          for (const property in err.response._data) {
            //@ts-ignore
            errors.value.push(`${property}: ${err.response._data[property]} `);
          }
          console.log(JSON.stringify(err.response));
        } else if (err.message) {
          //@ts-ignore
          errors.value.push("Something went wrong");
          console.log(JSON.stringify(err));
        }
      });
  } catch (err) {
    console.error(err);
  }
};
</script>
<template>
  <div class="py-10 px-6">
    <div class="max-w-sm mx-auto py-10 px-6 bg-gray-100 rounded-xl">
      <h1 class="mb-6 text-2xl">Sign In</h1>

      <form @submit.prevent="onSubmit">
        <input
          v-model="email"
          type="email"
          placeholder="example@gmail.com"
          class="w-full mb-4 px-6 rounded-xl h-12"
        />
        <input
          v-model="password1"
          type="password"
          placeholder="Password"
          class="w-full mb-4 px-6 rounded-xl h-12"
        />
        <input
          v-model="password2"
          type="password"
          placeholder="Password Repeat"
          class="w-full mb-4 px-6 rounded-xl h-12"
        />

        <div
          v-if="errors.length"
          class="mb-6 py-4 bg-rose-400 text-white rounded-xl"
        >
          <p v-for="err in errors" :key="err">
            {{ err }}
          </p>
        </div>

        <button class="py-4 px-6 bg-teal-700 text-white rounded-xl inline">
          Register
        </button>
      </form>
    </div>
  </div>
</template>