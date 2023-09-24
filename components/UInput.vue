<script setup lang="ts">

defineProps({
  modelValue: String,
  myFunction: Function as PropType<(payload: KeyboardEvent) => void>,
});
defineEmits(["update:modelValue"]);
</script>

<template>
  <div class="container mx-auto">
    <div class="flex justify-center my-10">
      <input
        type="text"
        class="outline-0 text-center w-full md:w-1/2 mb-6 bg-gray-100 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5 cursor-pointer dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-gray-400 dark:focus:ring-blue-500 dark:focus:border-blue-500"
        :value="modelValue"
        @input="$emit('update:modelValue', ($event.target as HTMLInputElement).value)"
        @keyup.enter="myFunction"
        placeholder="Enter country or city"
      />
    </div>
  </div>
</template>
