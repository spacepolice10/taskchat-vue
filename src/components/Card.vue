<script setup>
const props = defineProps(["card"]);
const emits = defineEmits(["drop", "cardDeleted"]);
import supabase from "../../lib/db";
import { onMounted, ref } from "vue";

import { XCircleIcon, CalendarIcon } from "@heroicons/vue/outline";
import CardComments from "@/components/cardComponents/CardComments.vue";
import CardEditMenu from "@/components/cardComponents/CardEditMenu.vue";
import CardTitleCategory from "@/components/cardComponents/CardTitleCategory.vue";
let taskPicture = ref(false);
let cardEl = ref(null);
let dropped = ref(null);
let somethingIsDragged = ref(false);

onMounted(async () => {
  loadImages().then((x) => {
    if (x.split("/").at(-1) != "null") {
      taskPicture.value.src = x;
    } else {
      taskPicture.value.style.display = "none";
    }
  });
});

function deleteCard() {
  cardEl.value.remove();
}

async function loadImages() {
  const { publicURL, error } = supabase.storage
    .from("images")
    .getPublicUrl(props.card.picture_link);
  return publicURL;
}

function onDrag(e, card) {
  e.target.classList.add("dragged");
  e.dataTransfer.setData("card", JSON.stringify(card));
  cardEl.value.style.opacity = "0%";
  cardEl.value.style.margin = `0px 0px -${
    cardEl.value.offsetHeight + "px"
  } 0px`;
}
function onDragEnter(e, card) {}
function onDragLeave(e, card) {}
function onDragEnd(e, card) {
  cardEl.value.style.opacity = "100%";
  cardEl.value.style.margin = "8px";
}
function onDrop(e, card) {
  const dragged = JSON.parse(e.dataTransfer.getData("card"));
  dropped.value = card;
  emits("drop", {
    dragElem: dragged,
    dropElem: dropped.value,
  });
}
</script>

<template>
  <div
    ref="cardEl"
    draggable="true"
    @mouseover="somethingIsDragged = true"
    @mouseleave="somethingIsDragged = false"
    @dragstart="onDrag($event, card)"
    @dragover.prevent
    @dragenter="onDragEnter($event, card)"
    @dragleave="onDragLeave($event, card)"
    @dragend="onDragEnd($event, card)"
    @drop="onDrop($event, card)"
    @contextmenu="test($event)"
    class="placeholder m-2 w-80 rounded-3xl bg-white p-4 text-slate-700 shadow-md duration-300"
    :class="somethingIsDragged ? 'droppable' : ''"
  >
    <div @dragenter.prevent>
      <div class="flex flex-col justify-between">
        <img
          draggable="false"
          class="placeholder pointer-events-none w-full rounded-3xl object-cover"
          ref="taskPicture"
        />
        <div class="flex items-baseline justify-between gap-2">
          <card-title-category
            :name="card.name"
            :category_id="card.category_id"
          />
          <card-edit-menu @cardDeleted="deleteCard" :id="card.id" />
        </div>
      </div>
      <article
        class="prose pointer-events-none break-words font-extralight prose-a:underline"
      >
        {{ card.description }}
      </article>
      <div class="pointer-events-none mt-2 flex gap-4 font-thin italic">
        <CalendarIcon class="h-6 w-6" />
        {{ card.date }}
      </div>
    </div>
  </div>
</template>

<style>
.droppable {
  /* transform: translate(0, 0); */
}
.placeholder {
  animation: appear 0.2s;
}
@keyframes appear {
  from {
    transform: scale(0.9);
    opacity: 0;
  }
  to {
  }
}
</style>
