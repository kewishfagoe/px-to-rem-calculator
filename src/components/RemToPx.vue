<script setup lang="ts">
import { ref } from 'vue'
import Toast from './Toast.vue'
import Card from '../layouts/Card.vue'
import Result from './Result.vue'

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
    <Card>
        <template v-slot:body>
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
                <Result v-if="showSuccess" :result="result" unit="px" @copy-result="copyResultToClipboard" />
            </Transition>
        </template>
    </Card>

    <Transition name="fade">
        <Toast v-if="showCopiedMsg" type="success" message="Result copied to clipboard!"/>
    </Transition>
    <Transition name="fade">
        <Toast v-if="showError" type="error" message="Please enter a rem value that is a valid number larger than 0 with less than 4 decimal places."/>
    </Transition>
</template>

<style scoped>

</style>
