<script setup>
const props = defineProps(["card"]);
import supabase from "../../lib/db";
import { onMounted, ref } from "vue";
import { XCircleIcon, CalendarIcon } from "@heroicons/vue/solid";
import CardComments from "@/components/cardComponents/CardComments.vue";
let showMore = ref(false);
let taskPicture = ref(false);
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

onMounted(async () => {
  loadImages().then((x) => {
    if (x.split("/").at(-1) != "null") {
      taskPicture.value.src = x;
    } else {
      taskPicture.value.style.display = "none";
    }
  });
});

async function loadImages() {
  const { publicURL, error } = supabase.storage
    .from("images")
    .getPublicUrl(props.card.picture_link);
  return publicURL;
}
</script>

<template>
  <div
    @click="showMore = !showMore"
    class="m-2 w-80 rounded-xl p-4 duration-300"
    :class="showMore ? [color, 'pb-60 shadow-lg'] : [color, 'shadow-sm']"
  >
    <div class="flex flex-col justify-between">
      <img class="w-full rounded-2xl object-cover" ref="taskPicture" />
      <h1 class="mt-2 text-2xl font-bold">{{ card.name }}</h1>
    </div>
    <p class="break-words font-extralight">{{ card.description }}</p>
    <div class="mt-2 flex">
      <CalendarIcon class="h-6 w-6" />
      {{ card.date }}
    </div>
    <transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="translate-y-1 opacity-0"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="translate-y-1 opacity-0"
    >
    </transition>
  </div>
</template>
