<script setup>
const emits = defineEmits(["change-category"]);
import { onUpdated, ref } from "vue";
import { RadioGroup, RadioGroupLabel, RadioGroupOption } from "@headlessui/vue";
let taskCategory = ref("0");
onUpdated(() => {
  console.log(taskCategory);
  emits("change-category", taskCategory.value);
});
</script>

<template>
  <RadioGroup
    class="my-2 grid cursor-default grid-cols-2 items-baseline gap-y-5 sm:grid-cols-3"
    v-model="taskCategory"
  >
    <RadioGroupOption class="mt-2" v-slot="{ checked }" value="0">
      <span
        :class="
          checked
            ? 'm-2 rounded-lg bg-gray-400 p-2 text-gray-100 duration-150'
            : 'm-2 rounded-lg bg-gray-100 p-2 duration-150 hover:bg-gray-200'
        "
        >None</span
      >
    </RadioGroupOption>
    <RadioGroupOption v-slot="{ checked }" value="1">
      <span
        :class="
          checked
            ? 'm-2 rounded-lg bg-blue-400 p-2 text-blue-100 duration-150'
            : 'm-2 rounded-lg bg-blue-100 p-2 duration-150 hover:bg-blue-200'
        "
        >💼 Work</span
      >
    </RadioGroupOption>
    <RadioGroupOption v-slot="{ checked }" value="2">
      <span
        :class="
          checked
            ? 'm-2 rounded-lg bg-yellow-400 p-2 text-yellow-100 duration-150'
            : 'm-2 rounded-lg bg-yellow-100 p-2 duration-150 hover:bg-yellow-200'
        "
        >👋 Personal</span
      >
    </RadioGroupOption>
    <RadioGroupOption v-slot="{ checked }" value="3">
      <span
        :class="
          checked
            ? 'm-2 rounded-lg bg-green-400 p-2 text-green-100 duration-150'
            : 'm-2 rounded-lg bg-green-100 p-2 duration-150 hover:bg-green-200'
        "
        >😘 Friends</span
      >
    </RadioGroupOption>
    <RadioGroupOption v-slot="{ checked }" value="4">
      <span
        :class="
          checked
            ? 'm-2 rounded-lg bg-pink-400 p-2 text-pink-100 duration-150'
            : 'm-2 rounded-lg bg-pink-100 p-2 duration-150 hover:bg-pink-200'
        "
        >📚 Education</span
      >
    </RadioGroupOption>
  </RadioGroup>
</template>
