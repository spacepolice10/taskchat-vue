<script setup>
  import supabase from '../../lib/db'
  import newTask from '@/components/NewTask.vue'
  import {ref, onMounted} from 'vue'

  let database = ref([])
  
  //id of the list that will be used as a target for adding function (adding new elements to list manually)
  let addingListId = ref(0)
  //v-model for input that makes possible dynamically change the name of the task sent to list
  let newTaskInput = ref('')

  async function loadDataFromDB () {
    const {data} = await supabase.from('lists').select('*, cards(*)')
    database.value = data
  }
  //manually create new task in supabase
  async function createNewTask () {
    const {data} = await supabase.from('cards').insert(
      {
        name: newTaskInput.value, 
        list_id: addingListId.value
      }
    )
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
    <button @click="addingListId = list.id">+</button>
    <new-task 
    v-if="list.id == addingListId"
    v-model="newTaskInput"
    @createNewTask="createNewTask"
    >
    </new-task>
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
  