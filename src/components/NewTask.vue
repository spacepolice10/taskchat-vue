<script setup>
    defineProps(['modelValue'])
    defineEmits(['update:modelValue'])
</script>

<template>
    <div class="bg-white">
        <input 
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        type="text" />
        <button
        @click="$emit('createNewTask')"
        >
            Send
        </button>
    </div>
</template>