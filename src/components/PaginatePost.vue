<script setup>
    const emit = defineEmits(['previous', 'next'])
    defineProps(['start', 'end', 'maxLength'])
</script>


<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button 
            type="button" 
            class="btn btn-outline-primary" 
            @click="emit('previous')"
            :disabled="start === 0 ? true : false"
        >
            Previous
        </button>
        <button 
            type="button" 
            class="btn btn-outline-primary" 
            @click="emit('next')"
            :disabled="end >= maxLength ? true : false"
        >
            Next
        </button>
    </div>
</template>
