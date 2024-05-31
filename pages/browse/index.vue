<script setup lang="ts">

let queryRef = ref('')
let query = ''

const Search = () => {
  queryRef.value = query
}

const { data: categories } = await useFetch(
  "http://127.0.0.1:8000/api/v1/jobs/categories"
);

let selectedCategoriesRef = ref("");
//@ts-ignore
let selectedCategories = <string[]>[];

const toggleCategory = (id: string) => {
  const index = selectedCategories.indexOf(id);

  if (index === -1) {
    selectedCategories.push(id);
  } else {
    selectedCategories.splice(index, 1);
  }

  selectedCategoriesRef.value = selectedCategories.join(",");
};

const { data: jobs } = await useFetch("http://127.0.0.1:8000/api/v1/jobs", {
  query: { 
    query: queryRef,
    categories: selectedCategoriesRef
   },
});
</script>
<template>
  <div class="grid md:grid-cols-4 gap-3 py-10 px-6">
    <div class="md:col-span-1 bg-teal-700 px-6 py-6 rounded-xl">
      <div class="flex space-x-4">
        <input
          v-model="query"
          type="search"
          placeholder="search job..."
          class="w-full px-6 py-4 rounded-full h-10"
        />

        <button 
          @click="Search"
          class="py-4 px-6 bg-teal-900 text-white rounded-xl"
        >
          <IconsSearch class="font-extrabold" />
        </button>
      </div>

      <hr class="my-6 opacity-30" />

      <h3 class="mt-6 text-xl text-white font-semibold">Categories</h3>

      <div class="mt-6 space-y-4">
        <p
          v-for="cat in categories"
          :key="cat.id"
          @click="toggleCategory(cat.id)"
          class="py-1 px-6 text-white rounded-xl hover:cursor-pointer"
          :class="{ 'bg-teal-900': selectedCategoriesRef.includes(cat.id) }"
        >
          {{ cat.title }}
        </p>
      </div>
    </div>
    <div class="md:col-span-3">
      <div class="space-y-4">
        <jobsCard v-for="job in jobs" :key="job.id" :job="job" />
      </div>
    </div>
  </div>
</template>
