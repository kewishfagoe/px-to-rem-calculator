<script setup lang="ts">

const props = defineProps<{
    result: number
    unit: string
}>()

const emit = defineEmits<{
    copyResult: []
}>()

function copyResult() {
    emit("copyResult")
}

</script>

<template>
    <div class="mt-4">
        <div class="stat bg-background shadow-sm">
            <div class="stat-title">Result</div>
            <div class="stat-value text-xl sm:text-3xl">{{ props.result }} {{ props.unit }}</div>
            <div class="stat-actions flex justify-end">
                <button
                    class="btn btn-neutral btn-sm"
                    @click="copyResult"
                >
                    <span>Copy</span>
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-copy" viewBox="0 0 16 16">
                        <path fill-rule="evenodd" d="M4 2a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2zm2-1a1 1 0 0 0-1 1v8a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1zM2 5a1 1 0 0 0-1 1v8a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1v-1h1v1a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h1v1z"/>
                    </svg>
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>

</style>
