<script setup lang="ts">
import { ref, onMounted } from 'vue'

import Heading from './components/Heading.vue';
import Footer from './components/Footer.vue';
import SetRootFontSizeModal from './components/SetRootFontSizeModal.vue';

const rootFontSize = ref<number>(16)

const loadRootFontSize = () => {
    try {
        const storedValue = localStorage.getItem('rootFontSize')
        if (storedValue) {
            rootFontSize.value = parseInt(storedValue)
        }
    } catch (error) {
        console.warn('localStorage access failed:', error)
    }
}

const updateRootFontSize = (newValue: number) => {
    rootFontSize.value = newValue
    try {
        if (newValue === 16) {
          localStorage.removeItem('rootFontSize')
        } else {
          localStorage.setItem('rootFontSize', newValue.toString())
        }
    } catch (error) {
        console.warn('localStorage write failed: ', error)
    }
}


const modalRef = ref(null)
const showModal = () => {
    if (modalRef.value) {
        modalRef.value.showModal()
    }
}

onMounted(() => {
    loadRootFontSize()
})
</script>

<template>
    <main class="prose">
        <Heading message="PX to REM Calculator" />
        <p class="flex items-center">
            The calculations are based on a root <code>font-size</code> of {{ rootFontSize }} pixel.
            <span @click="showModal">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil ml-2 hover:cursor-pointer" viewBox="0 0 16 16">
                    <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325"/>
                </svg>
            </span>
        </p>
        <Footer />
        <SetRootFontSizeModal ref="modalRef" :root-font-size="rootFontSize" @update-root-font-size="updateRootFontSize" />
    </main>
</template>

<style scoped>

</style>
