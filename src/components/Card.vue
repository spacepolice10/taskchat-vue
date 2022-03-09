<script setup>
    const props = defineProps(['card'])
    import { ref } from 'vue'
    import { XCircleIcon } from '@heroicons/vue/solid'
    import CardInfo from '@/components/CardInfo.vue'
    let showMore = ref(false)
    let color = 'bg-gray-50'
    switch (props.card.category_id) {
        case 0: color = 'bg-gray-100 text-gray-400'; break
        case 1: color = 'bg-blue-100 text-blue-400'; break
        case 2: color = 'bg-yellow-100 text-yellow-400'; break
        case 3: color = 'bg-green-100 text-green-400'; break
        case 4: color = 'bg-pink-100 text-pink-400'; break
    }
</script>

<template>
    <div @click="showMore = !showMore" draggable="true" :class="showMore ? [color, 'shadow-lg w-96 pb-60 scale-110 duration-150'] : [color, 'shadow-sm w-80 duration-150']" >
        <div class="flex justify-between">
        <p>{{ card.name }}</p>
        <button v-if="showMore">
            <XCircleIcon class="w-4 h-4 text-gray-400 fill-current" />
        </button>
        </div>
        <transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="translate-y-1 opacity-0"
        enter-to-class="translate-y-0 opacity-100"
        leave-active-class="transition duration-150 ease-in"
        leave-from-class="translate-y-0 opacity-100"
        leave-to-class="translate-y-1 opacity-0"
        >
        <div class="absolute flex justify-between w-10/12" v-if="showMore">
            {{card.category_id}}
        </div>
        </transition>
    </div>
</template>