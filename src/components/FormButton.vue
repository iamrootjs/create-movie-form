<script setup>
import FormModal from "./FormModal.vue"
import {ref} from 'vue'

const toggle = ref(false)

let showModal = () =>
{
    toggle.value = !toggle.value
}

</script>
<template>
    <div class="self-end mr-10 mb-10 ">
        <button @click="showModal()" class="w-full h-auto rounded-lg bg-blue-500 p-2 text-white shadow-md border-solid border-white border pointer-events-auto">Add a movie</button>
    </div>
    <FormModal v-if="toggle" @movieAdded="showModal()" />

</template>
