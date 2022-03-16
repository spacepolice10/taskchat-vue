<script setup>
import supabase from "../../lib/db";
import { Popover, PopoverButton, PopoverPanel } from "@headlessui/vue";
import NewTask from "@/components/NewTask.vue";
import Card from "@/components/Card.vue";
import { onMounted, ref } from "vue";
const props = defineProps(["list"]);
let cards = ref([]);
let updateListId = ref(props.list.id);

async function loadDataFromDB() {
  const { data } = await supabase
    .from("cards")
    .select("*")
    .eq("list_id", props.list.id);
  cards.value = data;
}

onMounted(async () => {
  loadDataFromDB();
  const cards = supabase
    .from(`cards:list_id=eq.${props.list.id}`)
    .on("INSERT", (payload) => {
      loadDataFromDB();
    })
    .subscribe();
});

async function dropCardToCard(e) {
  const elem = document.querySelector(".dragged");
  elem.remove();
  cards.value.splice(e.dropElem.position, 0, e.dragElem);
  const {} = await supabase.rpc("lowerposition", {
    posi: e.dragElem.position,
    li: e.dropElem.list_id,
  });
  const {} = await supabase
    .from("cards")
    .update({ list_id: e.dropElem.list_id, position: e.dropElem.position })
    .eq("id", e.dragElem.id);
  const {} = await supabase.rpc("higherposition", {
    posi: e.dragElem.position,
    li: e.dropElem.list_id,
  });
}

async function addCardToList(e) {
  if (!e.target.classList.contains("h-screen")) return;
  const dragCard = JSON.parse(e.dataTransfer.getData("card"));
  const elem = document.querySelector(".dragged");
  if (elem) elem.remove();
  cards.value.push(dragCard);
  const positionToSet = cards.value.length
    ? cards.value.at(-1).position + 1
    : 0;
  const {} = await supabase
    .from("cards")
    .update({
      list_id: props.list.id,
      position: positionToSet,
    })
    .eq("id", dragCard.id);
}
</script>

<template>
  <div class="h-screen w-full" @dragover.prevent @drop="addCardToList($event)">
    <Popover class="relative flex items-center p-2">
      {{ list.name }}
      <PopoverButton
        class="font-2xl m-2 flex h-6 w-10 items-center justify-center rounded-md bg-gray-200 p-2 text-gray-400 hover:bg-gray-300"
      >
        +
      </PopoverButton>
      <transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="translate-y-1 opacity-0"
        enter-to-class="translate-y-0 opacity-100"
        leave-active-class="transition duration-150 ease-in"
        leave-from-class="translate-y-0 opacity-100"
        leave-to-class="translate-y-1 opacity-0"
      >
        <PopoverPanel
          v-slot="{ close }"
          class="fixed bottom-0 left-0 z-10 m-0 w-full p-0 sm:bottom-auto sm:left-auto sm:mt-96 md:w-auto"
        >
          <new-task @task-created="close()" :id="list.id" :cards="cards" />
        </PopoverPanel>
      </transition>
    </Popover>
    <card
      :key="card.id"
      v-for="card in cards.sort(function (a, b) {
        return a.position - b.position;
      })"
      :card="card"
      @drop="dropCardToCard"
    />
  </div>
</template>
