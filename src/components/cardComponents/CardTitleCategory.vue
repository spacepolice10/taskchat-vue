<script setup>
const props = defineProps(["name", "category_id"]);
let categoryColor = "";
switch (props.category_id) {
  case 0:
    categoryColor = "bg-gray-400";
    break;
  case 1:
    categoryColor = "bg-blue-400";
    break;
  case 2:
    categoryColor = "bg-yellow-400";
    break;
  case 3:
    categoryColor = "bg-green-400";
    break;
  case 4:
    categoryColor = "bg-pink-400";
    break;
}
</script>

<template>
  <div class="flex items-baseline justify-start gap-2">
    <h1 class="mt-2 w-full text-lg font-bold">{{ name }}</h1>
    <!-- Div-wrapper helps to maintain the size of the inner div consistent -->
    <div>
      <div class="h-4 w-4 rounded-full" :class="[categoryColor]"></div>
    </div>
  </div>
</template>
