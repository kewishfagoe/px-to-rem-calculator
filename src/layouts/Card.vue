<script setup lang="ts">

</script>

<template>
    <div class="card bg-white min-w-75 max-w-90 shadow-sm m-1">
        <div v-if="$slots.body" class="card-body">
            <slot name="body"></slot>
        </div>
    </div>
</template>

<style scoped>

</style>
