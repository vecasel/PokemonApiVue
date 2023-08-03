<script setup>
    defineProps({
        name: String,
        url: String
    });

</script>

<template>
    <p><a :href="url">{{name.toUpperCase()}}</a></p>
</template>