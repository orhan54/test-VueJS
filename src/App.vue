<template>

  <p>Compteur : {{ count }}</p>

  <p :style="{ color: 'red' }" v-if="count > 5">
    vous avez cliqué plus de 5 fois
  </p>
  <p :style="{ color: 'green' }" v-else>
    vous avez cliqué moins de 5 fois
  </p>

  <button :disabled="count > 9" @click="incre">incrementer</button>
  <button :disabled="count < 1" @click="decre">decrementer</button>

  <hr>

  <form @submit.prevent="addMovie">
    <input type="text" placeholder="Nouveau film" v-model="movieName">
    <button>Ajouter</button>
  </form>

  <ul>
    <li v-for="movie in movies" :key="movie">
      {{ movie }}
      <button @click="deleteMovie(movie)">Supprimer</button>
    </li>
  </ul>

  <FlashCards :items="cards" @swipe-left="handleSwipeLeft" @swipe-right="handleSwipeRight">
    <template #default="{ item }">
      <div class="card">
        <h3>{{ item.text }}</h3>
      </div>
    </template>
  </FlashCards>

  <!-- CAROUSEL PHOTOS -->
  <Carousel v-bind="carouselConfig">
    <Slide v-for="(image, index) in slides" :key="index">
      <div class="carousel__item">
        <img :src="image" alt="photo" />
      </div>
    </Slide>

    <template #addons>
      <Navigation />
      <Pagination />
    </template>
  </Carousel>


  <div class="container mt-4">
    <div class="row">

      <div class="col-sm-6 col-md-4 col-lg-3 mb-4" v-for="(slide, index) in slides" :key="index">
        <div class="card h-100 shadow-sm">
          <img :src="slide" class="card-img-top" alt="Image">
          <div class="card-body text-center">
            <p class="card-text">
              Image numéro {{ index + 1 }}
            </p>
          </div>
        </div>
      </div>

    </div>
  </div>





</template>


<script setup>
import { ref } from 'vue'
import { FlashCards } from 'vue3-flashcards'
import 'vue3-carousel/carousel.css'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'

/* CONFIG CAROUSEL */
const carouselConfig = {
  itemsToShow: 2.5,
  wrapAround: true
}

/* TABLEAU DES IMAGES */
const slides = [
  new URL('./assets/image1.avif', import.meta.url).href,
  new URL('./assets/image2.avif', import.meta.url).href,
  new URL('./assets/image3.webp', import.meta.url).href,
  new URL('./assets/image4.webp', import.meta.url).href,
  new URL('./assets/image5.avif', import.meta.url).href,
  new URL('./assets/image6.avif', import.meta.url).href,
  new URL('./assets/image7.jpeg', import.meta.url).href,
  new URL('./assets/image8.avif', import.meta.url).href,
  new URL('./assets/image9.avif', import.meta.url).href,
  new URL('./assets/image10.webp', import.meta.url).href,
]

/* FLASH CARDS */
const cards = ref([
  { id: 1, text: 'First Card' },
  { id: 2, text: 'Second Card' },
  { id: 3, text: 'Third Card' },
])

function handleSwipeLeft(item) {
  console.log('Swiped left:', item)
}

function handleSwipeRight(item) {
  console.log('Swiped right:', item)
}

/* COMPTEUR */
const count = ref(0)
const incre = () => count.value++
const decre = () => count.value--

/* MOVIES */
const movieName = ref('')
const movies = ref([
  'Matrix',
  'Lilo & Stitch',
  'Titanic'
])

const addMovie = () => {
  if (movieName.value.trim() !== '') {
    movies.value.push(movieName.value)
    movieName.value = ''
  }
}

const deleteMovie = (movie) => {
  movies.value = movies.value.filter(m => m !== movie)
}
</script>


<style>
.flash-card__transform {
  display: flex;
  justify-content: center;
}

.card {
  border: 1px solid black;
  background-color: antiquewhite;
  width: 250px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.carousel {
  margin-top: 80px;
}

/* STYLE SLIDE */
.carousel__item {
  display: flex;
  justify-content: center;
  align-items: center;
}

.carousel__item img {
  width: 100%;
  height: 300px;
  object-fit: cover;
  border-radius: 10px;
}

.carousel__icon {
  color: white;
}

.carousel__pagination-item button {
  color: white;
}

.card-img-top {
  min-height: 200px;
}

.card:hover {
  cursor: pointer;
  transform: scale(1.08);
  transition:transform .8s;
}
</style>