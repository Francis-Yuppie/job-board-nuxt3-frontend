<script setup>
import { onMounted } from "vue";
import { useUserStore } from "~/stores/user";

const userStore = useUserStore();
const router = useRouter();
const route = useRoute();

onMounted(() => {
  if (!userStore.user.isAuthenticated) {
    router.push("/login");
  } else {
    getJobs();
  }
});

const { data: job } = await useFetch(
  "http://127.0.0.1:8000/api/v1/jobs/" + route.params.id
);

let category = ref("");
let title = ref(job.value.title);
let description = ref(job.value.description);
let location = ref(job.value.position_location);
let salary = ref(job.value.position_salary);
let company_name = ref(job.value.company_name);
let company_email = ref(job.value.company_email);
let company_location = ref(job.value.company_location);
let errors = ref([]);


const onSubmit = async () => {
  errors.value = [];

  if (title.value == "") {
    errors.value.push("title field is missing");
  }
  if (description.value == "") {
    errors.value.push("description field is missing");
  }
  if (salary.value == "") {
    errors.value.push("salary field is missing");
  }
  if (location.value == "") {
    errors.value.push("location field is missing");
  }
  if (company_name.value == "") {
    errors.value.push("company name field is missing");
  }
  if (company_location.value == "") {
    errors.value.push("company location field is missing");
  }
  if (company_email.value == "") {
    errors.value.push("company email field is missing");
  }


  if (errors.value.length == 0) {
    try {
      await $fetch(`http://127.0.0.1:8000/api/v1/jobs/${route.params.id}/update`, {
        method: "PUT",
        //@ts-ignore
        headers: {
          Authorization: "token " + userStore.user.token,
          "Content-Type": "application/json",
        },
        body: {
          category: job.value.category,
          title: title.value,
          description: description.value,
          position_salary: salary.value,
          position_location: location.value,
          company_name: company_name.value,
          company_location: company_location.value,
          company_email: company_email.value,
        },
      })
        .then((response) => {
          console.log(response);

          useRouter().push("/myjobs");
        })
        .catch((err) => {
          if (err.response) {
            for (const property in err.response._data) {
              //@ts-ignore
              errors.value.push(
                `${property}: ${err.response._data[property]} `
              );
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
  }
};

</script>

<template>
    <div class="py-10 px-6">
      <h1 class="mb-t text-2xl text-white font-extrabold">Update Job: {{ job.id }}</h1>
  
      <form @submit.prevent="onSubmit" class="space-y-4">
        
        <div>
          <label class="text-white font-semibold">Title</label>
          <input
            v-model="title"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 h-10"
          />
        </div>
        <div>
          <label class="text-white font-semibold">Description</label>
          <textarea
            v-model="description"
            rows="8"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 font-normal"
          ></textarea>
        </div>
        <div>
          <label class="text-white font-semibold">Location</label>
          <input
            v-model="location"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 h-10"
          />
        </div>
        <div>
          <label class="text-white font-semibold">Salary</label>
          <input
            v-model="salary"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 h-10"
          />
        </div>
  
        <div>
          <label class="text-white font-semibold">Company Name</label>
          <input
            v-model="company_name"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 h-10"
          />
        </div>
        <div>
          <label class="text-white font-semibold">Company Location</label>
          <input
            v-model="company_location"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 h-10"
          />
        </div>
        <div>
          <label class="text-white font-semibold">Company Email</label>
          <input
            v-model="company_email"
            type="text"
            class="w-full mt-2 p-4 rounded-xl bg-gray-100 h-10"
          />
        </div>
  
        <div
          v-if="errors.length"
          class="mb-6 py-4 bg-rose-400 text-white rounded-xl"
        >
          <p v-for="err in errors" :key="err">
            {{ err }}
          </p>
        </div>
  
        <button
          class="py-4 px-6 bg-teal-700 text-white rounded-xl hover:bg-teal-400"
        >
          Save Changes
        </button>
      </form>
    </div>
  </template>