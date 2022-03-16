<script setup>
import { ArrowCircleUpIcon, XCircleIcon } from "@heroicons/vue/solid";
import supabase from "../../lib/db";
import { ref, onMounted } from "vue";
import CategoryPicker from "@/components/newTaskComponents/CategoryPicker.vue";
const props = defineProps(["cards", "id"]);
const emits = defineEmits(["task-created"]);
const file = ref(null);
const title = ref(null);
let taskName = ref("");
let taskDescription = ref("");
let taskDate = ref("");
let taskCategory = ref("0");
let taskPictureLink = ref("null");

onMounted(() => {
  title.value.focus();
});
// function updateImagePreview() {
//   document.querySelector('img').src = document.querySelector('#test').files[0]
// }
async function uploadImage() {
  const image = file.value.files[0];
  if (!image) return;
  const imageName = image.name;
  taskPictureLink = { value: `public/${imageName}` };
  const { data } = await supabase.storage
    .from("images")
    .upload(`public/${imageName}`, image);
}
async function addCardToDatabase() {
  const lastCard = props.cards.at(-1);
  const positionToSet = lastCard ? lastCard.position + 1 : 0;
  const { data } = await supabase.from("cards").insert({
    name: taskName.value,
    list_id: props.id,
    category_id: Number(taskCategory.value),
    description: taskDescription.value,
    date: taskDate.value,
    picture_link: taskPictureLink.value,
    position: positionToSet,
  });
}
async function createNewTask() {
  uploadImage()
    .then(() => addCardToDatabase())
    .then(() => emits("task-created"));
}
</script>

<template>
  <div
    class="it flex min-w-fit flex-col gap-2 rounded-lg bg-white p-2 shadow-lg md:min-w-max"
  >
    <div class="flex w-full justify-between">
      <h1 class="text-2xl font-bold">Task title:</h1>
    </div>
    <input
      class="mx-2 h-10 rounded-lg border-none bg-gray-100 p-2 text-gray-500"
      ref="title"
      v-model="taskName"
    />
    <h2 class="text-md font-bold">Category:</h2>
    <category-picker @change-category="(x) => (taskCategory = x)" />
    <h2 class="text-md font-bold">Date:</h2>
    <input
      class="mx-2 rounded-lg border-none bg-gray-100 text-gray-500"
      type="date"
      v-model="taskDate"
    />
    <h2 class="text-md font-bold">Description:</h2>
    <textarea
      v-model="taskDescription"
      class="mx-2 resize-none rounded-lg border-none bg-gray-100 text-gray-500"
    >
    </textarea>
    <h2 class="text-md font-bold">Image:</h2>
    <input
      @change="updateImagePreview"
      ref="file"
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
