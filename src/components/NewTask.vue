<script setup>
    import {
        RadioGroup,
        RadioGroupLabel,
        RadioGroupOption,
    } from '@headlessui/vue'
    import { ArrowCircleUpIcon } from '@heroicons/vue/solid'
    import supabase from '../../lib/db'
    import { ref, onMounted } from 'vue'
    const props = defineProps(['id'])
    let newTaskInput = ref('')
    let category = ref('0')
    onMounted(() => {
        document.querySelector('input').focus()
    })
    async function createNewTask () {
    const {data} = await supabase.from('cards').insert(
      {
        name: newTaskInput.value, 
        list_id: props.id,
        category_id: Number(category.value)
      }
    )
  }
</script>

<template>
    <div class="bg-white p-2 flex flex-col gap-2 rounded-lg shadow-lg min-w-max">
        <h1 class="text-2xl font-bold">Task title:</h1>
        <input 
        class="bg-gray-100 rounded-lg border-none mx-2"
        :value="newTaskInput"
        @input="newTaskInput = $event.target.value"
        type="text" />
        <h2 class="text-md font-bold">Category:</h2>
        <RadioGroup class="grid grid-cols-3 gap-y-5 items-baseline" v-model="category">
            <RadioGroupOption class="mt-2" v-slot="{ checked }" value="0">
            <span :class="checked ? 'rounded-lg duration-150 p-2 m-2 text-gray-100 bg-gray-400' : 'rounded-lg duration-150 p-2 m-2 bg-gray-100'">None</span>
            </RadioGroupOption>
            <RadioGroupOption v-slot="{ checked }" value="1">
            <span :class="checked ? 'rounded-lg duration-150 p-2 m-2 text-blue-100 bg-blue-400' : 'rounded-lg duration-150 p-2 m-2 bg-blue-100'">💼  Work</span>
            </RadioGroupOption>
            <RadioGroupOption v-slot="{ checked }" value="2">
            <span :class="checked ? 'rounded-lg duration-150 p-2 m-2 text-yellow-100 bg-yellow-400' : 'rounded-lg duration-150 p-2 m-2 bg-yellow-100'">👋  Personal</span>
            </RadioGroupOption>
            <RadioGroupOption v-slot="{ checked }" value="3">
            <span :class="checked ? 'rounded-lg duration-150 p-2 m-2 text-green-100 bg-green-400' : 'rounded-lg duration-150 p-2 m-2 bg-green-100'">😘  Friends</span>
            </RadioGroupOption>
            <RadioGroupOption v-slot="{ checked }" value="4">
            <span :class="checked ? 'rounded-lg duration-150 p-2 m-2 text-pink-100 bg-pink-400' : 'rounded-lg duration-150 p-2 m-2 bg-pink-100'">📚  Education</span>
            </RadioGroupOption>
        </RadioGroup>
        <button
        @click="createNewTask"
        >
            <ArrowCircleUpIcon 
                class="w-10 h-10 m-2 bg-white rounded-full text-green-400 fill-current duration-150 hover:scale-110"
            />
        </button>
    </div>
</template>