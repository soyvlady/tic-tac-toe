<script setup lang="ts">
defineProps({
    player: String,
})
</script>
<template>
    <button class="p-5 w-24 h-20 bg-gray-800 text-4xl text-rose-700 flex justify-center items-center cursor-pointer">
        {{player}}
    </button>
</template>
<style scoped></style>
