<template>
  <div class="relative flex">
    <input
      :id="id"
      :type="type"
      :placeholder="placeholder"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      class="bg-gray-50 border border-gray-300 text-gray-900 outline-none focus:border-blue-500 hover:bg-[#E6F2FF] text-sm rounded-lg block w-full p-2.5"
    />
    <MagnifyingGlass class="absolute end-2 top-3 fill-[#98A2B3]" />
  </div>
</template>

<script setup>
import { defineEmits } from "vue";
import MagnifyingGlass from "../assets/magnifyingGlass.svg";

const props = defineProps({
  id: {
    type: String,
    default: "searchQuery",
  },
  type: {
    type: String,
    default: "text",
  },
  placeholder: {
    type: String,
    default: "Enter book title...",
  },
  modelValue: {
    type: String,
    default: "",
  },
});

const emits = defineEmits(["update:modelValue"]);
</script>
<style scoped>
@import "../assets/main.css";

.blue {
  background-color: var(--Primary-Blue-100);
}
</style>
