<script setup>
import { RadioGroup, RadioGroupLabel, RadioGroupOption } from "@headlessui/vue";
import { ArrowCircleUpIcon } from "@heroicons/vue/solid";
import supabase from "../../lib/db";
import { ref, onMounted } from "vue";
const props = defineProps(["id"]);
let taskName = ref("");
let taskDescription = ref("");
let category = ref("0");

onMounted(() => {
  document.querySelector("input").focus();
});
function updateImagePreview() {
  // document.querySelector('img').src = document.querySelector('#test').files[0]
}
async function uploadImage() {
  const image = document.querySelector("#test").files[0];
  const imageName = image.name;
  const { data } = await supabase.storage
    .from("images")
    .upload(`public/${imageName}`, image);
  console.log(data);
}
async function createNewTask() {
  // uploadImage()
  const { data } = await supabase.from("cards").insert({
    name: taskName.value,
    list_id: props.id,
    category_id: Number(category.value),
    description: taskDescription.value,
  });
}
</script>

<template>
  <div class="flex min-w-max flex-col gap-2 rounded-lg bg-white p-2 shadow-lg">
    <h1 class="text-2xl font-bold">Task title:</h1>
    <input
      class="mx-2 h-10 rounded-lg border-none bg-gray-100 p-2 text-gray-500"
      :value="taskName"
      @input="taskName = $event.target.value"
    />
    <h2 class="text-md font-bold">Category:</h2>
    <RadioGroup
      class="my-2 grid cursor-default grid-cols-3 items-baseline gap-y-5"
      v-model="category"
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
    <h2 class="text-md font-bold">Date:</h2>
    <input
      class="mx-2 rounded-lg border-none bg-gray-100 text-gray-500"
      type="date"
      value="2022-01-01"
    />
    <h2 class="text-md font-bold">Description:</h2>
    <textarea
      :value="taskDescription"
      @input="taskDescription = $event.target.value"
      class="mx-2 resize-none rounded-lg border-none bg-gray-100 text-gray-500"
    >
    </textarea>
    <h2 class="text-md font-bold">Image:</h2>
    <input
      @change="updateImagePreview"
      id="test"
      class="text-sm font-light text-gray-400 file:my-4 file:mx-2 file:rounded-lg file:border-none file:bg-gray-100 file:p-2 file:font-light file:text-gray-400 hover:file:bg-gray-50"
      type="file"
    />
    <img />
    <button @click="createNewTask">
      <ArrowCircleUpIcon
        class="m-2 h-10 w-10 rounded-full bg-white fill-current text-green-400 duration-150 hover:scale-110"
      />
    </button>
  </div>
</template>
