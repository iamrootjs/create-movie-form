<script setup>
import { ref } from "vue";
import { StarIcon } from "@heroicons/vue/24/solid";
import { items } from "./movies.json";
import FormButton from "./components/FormButton.vue";

const movies = ref(items);
const name = ref("")
const description = ref("")
const image = ref("")
const genre = ref([])
const inTheater = ref(false)
let addMovie = async () => {
  try{


  const size = movies.value.length;
  await movies.value.push({
    "id": size + 1,
    "name": name.value,
    "description": description.value,
    "image": image.value,
    "rating": null,
    "genres": genre.value,
    "inTheaters": inTheater.value
  })
  

    name.value = '',
    description.value = '',
    image.value = '',
    genre.value = [],
    inTheater.value = false
    

      showModal()
   
  }
  catch(error)
  {
    console.log('Error adding the movie',error)
  }
}
const toggle = ref(false)

let showModal = () => {
  toggle.value = !toggle.value
}

/**
 * 
 * @param {text genre} item 
 */
let selectItem = (item) => {
  if (genre.value.includes(item)) {
    const index = genre.value.indexOf(item)
    genre.value.splice(index, 1)
  }
  else {
    genre.value.push(item);
  }
}


function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}
</script>

<template>
  <div class="app flex flex-col ">
    <div class="self-end mr-10 mb-10 ">
      <button @click="showModal()"
        class="w-full h-auto rounded-lg bg-blue-500 p-2 text-white shadow-md border-solid border-white border pointer-events-auto">Add
        a movie</button>
    </div>
    <div class="movie-list">
      <div class="movie-item" v-for="(movie, movieIndex) in movies" :key="movie.id">
        <div class="movie-item-image-wrapper">
          <div class="movie-item-star-wrapper">
            <StarIcon id="rating" class="movie-item-star-rating-icon"
              :class="[movie.rating ? 'text-yellow-500' : 'text-gray-500']" />
            <div class="movie-item-star-content-wrapper">
              <span v-if="movie.rating" id="rating-stars" class="movie-item-star-content-rating-rated">
                {{ movie.rating }}
              </span>
              <span v-else class="movie-item-star-content-rating-not-rated">
                -
              </span>
            </div>
          </div>
          <img :src="movie.image" class="movie-item-image" alt="" />
        </div>

        <div class="movie-item-content-wrapper">
          <div class="movie-item-title-wrapper">
            <h3 class="movie-item-title">{{ movie.name }}</h3>
            <div class="movie-item-genres-wrapper">
              <span v-for="genre in movie.genres" :key="`${movie.id}-${genre}`" class="movie-item-genre-tag">{{ genre
              }}</span>
            </div>
          </div>
          <div class="movie-item-description-wrapper">
            <p class="movie-item-description">{{ movie.description }}</p>
          </div>
          <div class="movie-item-rating-wrapper">
            <span class="movie-item-rating-text">
              Rating: ({{ movie.rating }}/5)
            </span>

            <div class="movie-item-star-icon-wrapper">
              <button v-for="star in 5" :key="star" class="movie-item-star-icon-button" :class="[
                star <= movie.rating ? 'text-yellow-500' : 'text-gray-500',
              ]" :disabled="star === movie.rating" @click="updateRating(movieIndex, star)">
                <StarIcon class="movie-item-star-icon" />
              </button>
            </div>
          </div>
        </div>
      </div>

    </div>
    <transition name="modal" class="absolute z-[500] p-6 rounded-lg shadow-lg" v-if="toggle" @movieAdded="showModal()">
      <form class="z-100 flex flex-col bg-gray-200 dark:bg-gray-900 absolute h-[600px] w-[400px] justify-between p4"
        @submit.prevent="addMovie()">
        <div class="flex flex-col">
          <label class="text-white">Name:</label>
          <input type="text" required placeholder="Enter your name" class="bg-gray-700 text-white" v-model.trim="name">
        </div>
        <div class="flex flex-col">
          <label class="text-white">Description:</label>
          <textarea type="text" required placeholder="Enter description" rows="4" class="bg-gray-700 text-white"
            v-model.trim="description"></textarea>
        </div>
        <div class="flex flex-col">
          <label class="text-white">Image:</label>
          <input type="text" required placeholder="Enter your image url" class="bg-gray-700 text-white" v-model.trim="image">
        </div>
        <div class="flex flex-col">
          <label class="text-white">Genre:</label>
          <ul class="p-2 appearance-none w-full h-full bg-gray-700 text-white">
            <li class="w-full appearance-none px-2" @click="selectItem('Comedy')"
              :class="{ 'bg-gray-800': genre.includes('Comedy') }">Comedy</li>
            <li class="w-full px-2" @click="selectItem('Drama')" :class="{ 'bg-gray-800': genre.includes('Drama') }">Drama
            </li>
            <li class="w-full px-2" @click="selectItem('Action')" :class="{ 'bg-gray-800': genre.includes('Action') }">
              Action</li>
            <li class="w-full px-2" @click="selectItem('Sci-fi')" :class="{ 'bg-gray-800': genre.includes('Sci-fi') }">
              Sci-Fi</li>
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
  </div></template>
