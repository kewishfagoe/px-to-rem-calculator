<script setup lang="ts">
import { ref, onMounted } from 'vue'

const props = defineProps<{
    rootFontSize: number
}>()

const emits = defineEmits<{
    updateRootFontSize: [newRootFontSize: number]
}>()

const newRootFontSize = ref<number>(props.rootFontSize)
const showError = ref<boolean>(false)

const modalRef = ref(null)

function showModal() {
    if (modalRef.value) {
        modalRef.value.showModal()
    }
}

function closeModal() {
    if (modalRef.value) {
        modalRef.value.close()
        showError.value = false
    }
}

function setNewRootFontSize() {
    // check if 0, negative, or has decimals
    if (newRootFontSize.value <= 0 || !Number.isInteger(newRootFontSize.value)) {
        showError.value = true
        return
    }

    closeModal()
    emits("updateRootFontSize", newRootFontSize.value)
}

defineExpose({
    showModal
})

const newRootFontSizeInput = ref()
onMounted(() => {
    // small delay to ensure the modal is fully rendered
    setTimeout(() => {
        newRootFontSizeInput.value!.focus()
    }, 10)
})
</script>

<template>
    <dialog ref="modalRef" class="modal modal-bottom sm:modal-middle">
        <div class="modal-box not-prose">
            <h3 class="text-lg font-bold">Change Root Font Size</h3>
            <label class="input w-full mt-6">
                <input
                    type="number"
                    placeholder="Enter a new root font size value..."
                    id="root-font-size-input"
                    name="newRootFontSize"
                    v-model="newRootFontSize"
                    ref="newRootFontSizeInput"
                    @keyup.enter="setNewRootFontSize"
                />
                <span class="label">px</span>
            </label>
            <div v-if="showError" class="text-red-600 text-xs mt-2">The root font size needs to be a valid whole number larger than 0.</div>
            <div class="modal-action">
                <button class="btn" @click="closeModal">Close</button>
                <button class="btn btn-primary bg-brand hover:bg-brand-hover" @click="setNewRootFontSize">Save</button>
            </div>
        </div>
    </dialog>
</template>

<style scoped>

</style>
