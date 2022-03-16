<script setup>
import supabase from "../../lib/db";
import List from "@/components/List.vue";
import { ref, onMounted } from "vue";

let database = ref([]);

async function loadDataFromDB() {
  const { data } = await supabase.from("lists").select("*");
  database.value = data;
}

onMounted(async () => {
  loadDataFromDB();
});
</script>

<template>
  <div class="flex max-h-screen w-full snap-x overflow-scroll bg-slate-100">
    <div
      class="m-2 flex w-full min-w-fit snap-center scroll-m-0 flex-col gap-6 overflow-y-scroll"
      :key="list.id"
      v-for="list in database"
    >
      <list :list="list" />
    </div>
  </div>
</template>
