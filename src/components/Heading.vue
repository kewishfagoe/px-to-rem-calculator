<script setup lang="ts">
const props = defineProps<{
    message: string
}>()
</script>

<template>
    <h1>{{ props.message }}</h1>
</template>

<style scoped>

</style>
