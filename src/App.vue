<template>
  <div class="container mx-auto px-4">
    <div class="p-8 min-h-screen">
      <div class="flex w-full mb-2">
        <p class="font-inter text-lg/[24px] w-full bg-white py-4 px-6 rounded-2xl">
          Тестовое задание
        </p>
      </div>
      <SearchBar @search="handleSearch" />
      <BooksTable :books="books" :isLoading="isLoading" />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import BooksTable from "./components/BooksTable.vue";
import SearchBar from "./components/SearchBar.vue";

const books = ref([]);
const isLoading = ref(false);

const handleSearch = async ({ query, subjects, years }) => {
  isLoading.value = true;
  try {
    const searchParams = new URLSearchParams({
      q: query,
      has_fulltext: true,
    });

    if (subjects.length) {
      searchParams.append("subject", subjects.join(","));
    }
    if (years.length) {
      console.log("years length: ", years.length);
      searchParams.append("first_publish_year", years.join(","));
    }

    const response = await fetch(`https://openlibrary.org/search.json?${searchParams.toString()}`);
    const data = await response.json();
    books.value = data.docs.map((doc) => ({
      title: doc.title,
      first_publish_year: doc.first_publish_year,
      format: doc.format || [],
      author_name: doc.author_name || [],
    }));
  } catch (error) {
    console.error("Failed to fetch books:", error);
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  handleSearch({ query: "the lord of the rings", subjects: [], years: [] });
});
</script>
