<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{
    rootFontSize: number
}>()

const remValue = ref<number>(1)
const result = ref<number>(0)

const showSuccess = ref<boolean>(false)
const showError = ref<boolean>(false)
function calculatePixelValue() {
    if (remValue.value <= 0 || remValue.value >= 1000000 || isMoreThan3Decimals(remValue.value)) {
        showError.value = true
        setTimeout(() => {
            showError.value = false
        }, 5000)
        return
    }

    result.value = parseFloat((remValue.value * props.rootFontSize).toFixed(2))
    showSuccess.value = true
}

function isMoreThan3Decimals(num: number) {
    const str = num.toString()

    if (str.includes('.')) {
        const decimals = str.split('.')[1]
        return decimals.length > 3
    }

    return false
}

const showCopiedMsg = ref<boolean>(false)
function copyResultToClipboard() {
    navigator.clipboard.writeText(`${result.value}px`)
    showCopiedMsg.value = true

    setTimeout(() => {
        showCopiedMsg.value = false
    }, 3000)
}
</script>

<template>
    <div class="card bg-white min-w-75 max-w-90 shadow-sm m-1">
        <div class="card-body">
            <label class="input join-item">
                <input
                    type="number"
                    min="1"
                    max="999999"
                    placeholder="Enter rem value..."
                    v-model="remValue"
                    @keyup.enter="calculatePixelValue"
                    class="[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none"
                />
                <span class="label">rem</span>
            </label>
            <div class="card-actions">
                <button
                    class="btn bg-brand hover:bg-brand-hover text-primary-content w-full"
                    @click="calculatePixelValue"
                >
                    Calculate
                </button>
            </div>
            <Transition name="fade">
                <div class="mt-4" v-if="showSuccess">
                    <div class="stat bg-background shadow-sm">
                        <div class="stat-title">Result</div>
                        <div class="stat-value text-xl sm:text-3xl">{{ result }} px</div>
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
        </div>
    </div>

    <Transition name="fade">
        <div v-if="showCopiedMsg" class="toast">
            <div class="alert alert-success">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 shrink-0 stroke-current" fill="none" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <span>Result copied to clipboard!</span>
            </div>
        </div>
    </Transition>
    <Transition name="fade">
        <div v-if="showError" class="toast">
            <div class="alert alert-error">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 shrink-0 stroke-current" fill="none" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <span>Please enter a rem value that is a valid number larger than 0 with less than 4 decimal places.</span>
            </div>
        </div>
    </Transition>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
