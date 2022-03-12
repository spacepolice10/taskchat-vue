<script setup>
import supabase from "../../lib/db";
import newTask from "@/components/NewTask.vue";
import Card from "@/components/Card.vue";
import { ref, onMounted } from "vue";
import { Popover, PopoverButton, PopoverPanel } from "@headlessui/vue";

let database = ref([]);
//id of the list that will be used as a target for adding function (adding new elements to list manually)
let addingListId = ref(0);

async function loadDataFromDB() {
  const { data } = await supabase.from("lists").select("*, cards(*)");
  database.value = data;
}

onMounted(async () => {
  loadDataFromDB();
});
</script>

<template>
  <div class="flex max-h-screen w-full snap-x overflow-scroll bg-gray-50">
    <div
      class="max-w m-2 flex min-w-fit snap-center scroll-m-0 flex-col gap-6 overflow-y-scroll"
      :key="list.id"
      v-for="list in database"
    >
      <Popover class="relative flex items-center p-2">
        {{ list.name }}
        <PopoverButton
          class="font-2xl m-2 flex h-6 w-10 items-center justify-center rounded-md bg-gray-200 p-2 text-gray-400 hover:bg-gray-300"
          @click="addingListId = list.id"
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
          <PopoverPanel class="fixed z-10 mt-96">
            <new-task :id="addingListId" />
          </PopoverPanel>
        </transition>
      </Popover>
      <card
        :key="card.id"
        v-for="card in list.cards.sort(function (a, b) {
          return a.position - b.position;
        })"
        :card="card"
      />
    </div>
  </div>
</template>
