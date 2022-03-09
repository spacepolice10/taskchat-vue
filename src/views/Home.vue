<script setup>
  import supabase from '../../lib/db'
  import newTask from '@/components/NewTask.vue'
  import {ref, onMounted} from 'vue'

  let database = ref([])

  async function loadDataFromDB () {
    const {data} = await supabase.from('lists').select('*, cards(*)')
    database.value = data
  }

  onMounted(async () => {
    loadDataFromDB()
  })

</script>

<template>
  <div class="flex justify-center bg-gray-50">
    <div 
    class="flex flex-col gap-6 border m-2"
    :key="list.id"
    v-for="list in database"
    >
    <button>+</button>
      <div
      class="border p-4 m-2"
      :key="card.id"
      v-for="card in list.cards.sort(function(a,b) {return a.position - b.position})"
      >
        {{card.name}}
      </div>
    </div>
  </div>
</template>
  