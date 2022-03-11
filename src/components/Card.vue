<script setup>
const props = defineProps(["card"]);
import { ref } from "vue";
import { XCircleIcon } from "@heroicons/vue/solid";
import CardComments from "@/components/cardComponents/CardComments.vue";
let showMore = ref(false);
let color = "bg-gray-50";
switch (props.card.category_id) {
  case 0:
    color = "bg-gray-100 text-gray-400";
    break;
  case 1:
    color = "bg-blue-100 text-blue-400";
    break;
  case 2:
    color = "bg-yellow-100 text-yellow-400";
    break;
  case 3:
    color = "bg-green-100 text-green-400";
    break;
  case 4:
    color = "bg-pink-100 text-pink-400";
    break;
}
</script>

<template>
  <div
    @click="showMore = !showMore"
    class="m-2 w-80 rounded-xl p-4 duration-300"
    :class="showMore ? [color, 'pb-60 shadow-lg'] : [color, 'shadow-sm']"
  >
    <div class="flex justify-between">
      <p class="font-bold" v-if="!showMore">{{ card.name }}</p>
      <h1 class="text-4xl font-bold" v-if="showMore">{{ card.name }}</h1>
      <button v-if="showMore">
        <XCircleIcon
          class="h-6 w-6 fill-current text-gray-400 hover:text-gray-500"
        />
      </button>
    </div>
    <p>{{ card.description }}</p>
    <transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="translate-y-1 opacity-0"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="translate-y-1 opacity-0"
    >
      <div
        class="absolute flex w-10/12 flex-col justify-between"
        v-if="showMore"
      >
        <p>Category: {{ card.category_id }}</p>
        <p>Description: {{ card.description }}</p>
      </div>
    </transition>
  </div>
</template>
