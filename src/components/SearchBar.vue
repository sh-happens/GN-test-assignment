<template>
  <form
    @submit.prevent="handleSubmit"
    class="flex justify-between py-4 px-6 bg-white rounded-t-2xl"
  >
    <div>
      <SearchInput v-model="searchQuery" placeholder="Поиск" />
      <p v-if="searchError" class="text-red-500">{{ searchError }}</p>
    </div>
    <div class="flex items-center">
      <MultiselectDropdown
        :options="yearOptions"
        v-model:selected="selectedYears"
        title="Год"
        icon="calendar"
      />
      <MultiselectDropdown
        :options="subjectOptions"
        v-model:selected="selectedSubjects"
        title="Тема книги"
        class="ml-3"
        icon="format"
      />

      <button
        type="submit"
        class="bg-primary-blue-500 hover:bg-[#0070E8] text-white font-bold ml-3 flex items-center rounded-lg px-5 py-3 text-sm/[16px] font-inter"
      >
        <MagnifyingGlass class="fill-white stroke-white mr-2" />
        Поиск
      </button>
    </div>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import MultiselectDropdown from "./MultiselectDropdown.vue";
import SearchInput from "./SearchInput.vue";
import MagnifyingGlass from "../assets/magnifyingGlass.svg";

const emits = defineEmits(["search"]);

const searchQuery = ref("");
const subjectOptions = ref([
  "Literature",
  "Authors",
  "Classic Literature",
  "Working class",
  "Young men",
  "American fiction (fictional works by one author)",
  "American literature",
  "Roman",
  "Fiction, general",
  "Authors, fiction",
  "Fiction, biographical",
  "Working class--fiction",
  "Authors--fiction",
]);
const selectedSubjects = ref([]);
const searchError = ref("");

const yearOptions = ref([...Array(130).keys()].map((i) => new Date().getFullYear() - i));
const selectedYears = ref([]);

const handleSubmit = () => {
  if (!searchQuery.value.trim()) {
    searchError.value = "Пожалуйста введите запрос.";
    return;
  }

  searchError.value = "";

  emits("search", {
    query: searchQuery.value,
    subjects: selectedSubjects.value,
    years: selectedYears.value,
  });
};
</script>
<style scoped>
@import "../assets/main.css";

.bg-primary-blue-500 {
  background-color: var(--Primary-Blue-500);
}
</style>
