<script setup>
import { ref } from 'vue'
import { items } from "../movies.json";

const movies = ref(items)

const name = ref("")
const description = ref("")
const image = ref("")
const genre = ref([])
const inTheater = ref(false)
let  addMovie = async() => {
    const size = movies.length;
    await movies.push({
        "id": size + 1,
        "name": name.value,
        "description": description.value,
        "image": image.value,
        "rating": null,
        "genres": genre.value,
        "inTheaters": inTheater.value
    })

    
}

/**
 * 
 * @param {text genre} item 
 */
let selectItem = (item) =>
{
    if(genre.value.includes(item))
    {
        const index = genre.value.indexOf(item)
        genre.value.splice(index,1)
    }
    else{
        genre.value.push(item);
    }
}
</script>

<template>
    <transition name="modal" class="absolute z-[500] p-6 rounded-lg shadow-lg">
        <form class="z-100 flex flex-col bg-gray-200 dark:bg-gray-900 absolute h-[600px] w-[400px] justify-between p4"
            @submit.prevent="addMovie(), $emit('movieAdded',movies.value)">
            <div class="flex flex-col">
                <label class="text-white">Name:</label>
                <input type="text" placeholder="Enter your name" class="bg-gray-700 text-white" v-model.trim="name">
            </div>
            <div class="flex flex-col">
                <label class="text-white">Description:</label>
                <textarea type="text" placeholder="Enter description" rows="4" class="bg-gray-700 text-white"
                    v-model.trim="description"></textarea>
            </div>
            <div class="flex flex-col">
                <label class="text-white">Image:</label>
                <input type="text" placeholder="Enter your image url" class="bg-gray-700 text-white" v-model.trim="image">
            </div>
            <div class="flex flex-col">
                <label class="text-white">Genre:</label>
                <ul class="p-2 appearance-none w-full h-full bg-gray-700 text-white">
                    <li    class="w-full appearance-none px-2" @click="selectItem('Comedy')" :class="{'bg-gray-800' : genre.includes('Comedy')}">Comedy</li>
                    <li    class="w-full px-2" @click="selectItem('Drama')" :class="{'bg-gray-800' : genre.includes('Drama')}">Drama</li>
                    <li   class="w-full px-2" @click="selectItem('Action')" :class="{'bg-gray-800' : genre.includes('Action')}">Action</li>
                    <li    class="w-full px-2" @click="selectItem('Sci-fi')" :class="{'bg-gray-800' : genre.includes('Sci-fi')}">Sci-Fi</li>
                </ul>
                <p class="text-white">{{ genre }}</p>
            </div>
            <div class="flex items-center">
                <input type="checkbox" id="inTheaters" v-model="inTheater">
                <label class="ml-4 text-white">In Theaters</label>
            </div>
            <input type="submit" value="Add"
                class="text-white border-2 border-solid border-white hover:text-gray-900 hover:border-gray-900 hover:bg-white cursor-pointer">
        </form>
    </transition>
</template>