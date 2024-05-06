<template>
  <div class="relative">
    <button
      type="button"
      @click="toggleDropdown"
      class="bg-gray-50 border border-gray-300 hover:bg-[#E6F2FF] text-gray-900 rounded-lg w-full px-5 py-3 text-sm/[16px] font-inter flex items-center"
    >
      <Calendar v-if="icon === 'calendar'" class="mr-2" />
      <Format v-if="icon === 'format'" class="mr-2" />
      {{ title }}
    </button>
    <div
      v-show="showDropdown"
      class="absolute z-10 bg-white border border-gray-300 mt-1 rounded-lg shadow-lg max-h-60 overflow-auto w-52"
    >
      <SearchInput v-model="filter" placeholder="Поиск" class="m-2" />
      <div class="flex justify-between items-center text-sm/[16px] font-inter px-2">
        <span>Выбрано {{ selectedItems.length }} из {{ options.length }}</span>
      </div>
      <div
        v-for="item in filteredOptions"
        :key="item"
        class="flex items-center p-2 hover:bg-gray-100"
      >
        <input type="checkbox" v-model="selectedItems" :value="item" class="mr-2" />
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, watch, computed } from "vue";
import Calendar from "../assets/calendar.svg";
import Format from "../assets/format.svg";
import SearchInput from "./SearchInput.vue";

const props = defineProps({
  options: Array,
  selected: Array,
  title: String,
  icon: String,
});

const emits = defineEmits(["update:selected"]);

const showDropdown = ref(false);
const filter = ref("");
const selectedItems = ref([...props.selected]);

const toggleDropdown = () => {
  showDropdown.value = !showDropdown.value;
};

const filteredOptions = computed(() => {
  return props.options.filter((item) => {
    const itemAsString = String(item);
    return itemAsString.toLowerCase().includes(filter.value.toLowerCase());
  });
});

watch(selectedItems, (newVal) => {
  emits("update:selected", newVal);
});
</script>
