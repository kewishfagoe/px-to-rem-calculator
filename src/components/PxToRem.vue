<script setup lang="ts">
import { ref } from 'vue'
import Toast from './Toast.vue'
import Card from '../layouts/Card.vue'
import Result from './Result.vue'

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
                <Result v-if="showSuccess" :result="result" unit="rem" @copy-result="copyResultToClipboard" />
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
