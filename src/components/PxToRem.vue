<script setup lang="ts">
import { ref } from 'vue'
import Toast from './Toast.vue'
import Card from '../layouts/Card.vue'

const props = defineProps<{
    rootFontSize: number
}>()

const pixelValue = ref<number>(1)
const result = ref<number>(0)

const showSuccess = ref<boolean>(false)
const showError = ref<boolean>(false)
function calculateRemValue() {
    if (pixelValue.value <= 0 || pixelValue.value >= 1000000 || !Number.isInteger(pixelValue.value)) {
        showError.value = true
        setTimeout(() => {
            showError.value = false
        }, 3000)
        return
    }

    result.value = parseFloat((pixelValue.value / props.rootFontSize).toFixed(4))
    showSuccess.value = true
}

const showCopiedMsg = ref<boolean>(false)
function copyResultToClipboard() {
    navigator.clipboard.writeText(`${result.value}rem`)
    showCopiedMsg.value = true

    setTimeout(() => {
        showCopiedMsg.value = false
    }, 3000)
}
</script>

<template>
    <Card>
        <template v-slot:body>
            <label class="input join-item">
                <input
                    type="number"
                    min="1"
                    max="999999"
                    placeholder="Enter pixel value..."
                    v-model="pixelValue"
                    @keyup.enter="calculateRemValue"
                    class="[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none"
                />
                <span class="label">px</span>
            </label>
            <div class="card-actions">
                <button
                    class="btn bg-brand hover:bg-brand-hover text-primary-content w-full"
                    @click="calculateRemValue"
                >
                    Calculate
                </button>
            </div>
            <Transition name="fade">
                <div class="mt-4" v-if="showSuccess">
                    <div class="stat bg-background shadow-sm">
                        <div class="stat-title">Result</div>
                        <div class="stat-value text-xl sm:text-3xl">{{ result }} rem</div>
                        <div class="stat-actions flex justify-end">
                            <button
                                class="btn btn-neutral btn-sm"
                                @click="copyResultToClipboard"
                            >
                                <span>Copy</span>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-copy" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M4 2a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2zm2-1a1 1 0 0 0-1 1v8a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1zM2 5a1 1 0 0 0-1 1v8a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1v-1h1v1a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h1v1z"/>
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>
            </Transition>
        </template>
    </Card>

    <Transition name="fade">
        <Toast v-if="showCopiedMsg" type="success" message="Result copied to clipboard!"/>
    </Transition>
    <Transition name="fade">
        <Toast v-if="showError" type="error" message="Please enter a pixel value that is a valid whole number larger than 0."/>
    </Transition>
</template>

<style scoped>

</style>
