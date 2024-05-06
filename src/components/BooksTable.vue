<template>
  <div v-if="isLoading" class="text-center p-4">Выпоняется запрос...</div>
  <div v-if="formattedBooks && formattedBooks.length > 0 && !isLoading" class="overflow-x-auto">
    <table class="min-w-full leading-normal">
      <thead>
        <tr>
          <th class="px-5 py-3 text-left font-inter text-sm/[16px] text-gray-600">Название</th>
          <th class="px-5 py-3 text-left font-inter text-sm/[16px] text-gray-600">Автор</th>
          <th class="px-5 py-3 text-left font-inter text-sm/[16px] text-gray-600 min-w-36">
            Год Издания
          </th>
          <th class="px-5 py-3 text-left font-inter text-sm/[16px] text-gray-600">Формат книги</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in formattedBooks" :key="book.key" class="hover:bg-gray-100">
          <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
            <div class="flex items-center">
              <p class="text-gray-900 whitespace-no-wrap">{{ book.title }}</p>
            </div>
          </td>
          <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
            <p class="text-gray-900 whitespace-no-wrap">{{ book.author_name.join(", ") }}</p>
          </td>
          <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm text-right">
            <p class="text-gray-900 whitespace-no-wrap">{{ book.first_publish_year }}</p>
          </td>
          <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
            <div class="flex flex-wrap gap-1">
              <span
                v-for="format in book.displayFormats.visibleFormats"
                :key="format"
                class="bg-primary-blue-100 rounded-lg px-2 py-1 text-xs font-inter h-6"
              >
                {{ format }}
              </span>
              <span v-if="book.displayFormats.more" class="text-xs text-gray-500">{{
                book.displayFormats.more
              }}</span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <div v-if="formattedBooks && formattedBooks.length == 0 && !isLoading" class="text-center p-4">
    По вашему запросу ничего не найдено
  </div>
</template>

<style scoped>
@import "../assets/main.css";

.bg-primary-blue-100 {
  background-color: var(--Primary-Blue-100);
}
</style>

<script setup>
import { defineProps, computed } from "vue";

const props = defineProps({
  books: Array,
  isLoading: Boolean,
});

const formattedBooks = computed(() => {
  return props.books.map((book) => ({
    ...book,
    displayFormats: formatDisplay(book.format),
  }));
});

function formatDisplay(formats) {
  if (formats.length > 3) {
    const visibleFormats = formats.slice(0, 3);
    const moreCount = formats.length - 3;
    return { visibleFormats, more: `еще+${moreCount}` };
  } else {
    return { visibleFormats: formats, more: "" };
  }
}
</script>
