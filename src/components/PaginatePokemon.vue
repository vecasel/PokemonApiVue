<script setup>
    defineProps(['next', 'previous']);
    const emit = defineEmits(['loadingPage']);
</script>

<template>
    <button 
        @click="emit('loadingPage', previous)"
        :class="{'button--previous': previous != null, 'button--previousD': previous == null}"
        :disabled="previous == null"
    >Previous
    </button>
    <button  
        @click="emit('loadingPage', next)" 
        :class="{'button--next': next != null, 'button--nextD': next == null}"
        :disabled="next == null" 
    >Next
    </button>
</template>