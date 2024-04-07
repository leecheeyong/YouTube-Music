<script setup>
import { ref } from 'vue'
const audio = new Audio()
const playing = ref(false)
const current = ref(null)
const input = ref('')
const videos = ref([])
const error = ref('')
async function change() {
    const data = await fetch(`https://weeb-api.vercel.app/ytsearch?query=${input.value}`)
    .then(res => res.json())
    if(Array.isArray(data)) {
        videos.value = data
    }
}

async function toggle(url) {
    if (playing.value && current.value === url) {
        audio.pause()
        playing.value = false
        console.log('paused')
    }else if(current.value === url && playing.value === false) {
        audio.play()
        playing.value = true
        console.log('playing')
    }else {
        audio.src = `https://weeb-api.vercel.app/ytaudio?query=${url}`
        try {
        await audio.play()
        playing.value = true
        current.value = url
        console.log('start')
        }catch(e) {
        console.log(e)
        error.value = 'An error occured'
        }
    }
}
</script>
<template>
    <section class="h-screen flex items-center flex-wrap">
        <div class="max-w-screen-xl px-4 mx-auto lg:px-12 mt-4 w-full">
          <div class="relative shadow-md bg-gray-800 sm:rounded-lg">
            <div class="flex flex-col items-center justify-center p-4 space-y-3 md:flex-row md:space-y-0 md:space-x-4">
              <div class="w-full md:w-1/2">
                <div class="flex items-center">
                  <label for="simple-search" class="sr-only">Search</label>
                  <div class="relative w-full">
                    <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                      <svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="currentColor" viewbox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
                      </svg>
                    </div>
                    <input type="text" v-model="input" @change="change" class="block w-full p-2 pl-10 text-sm  border border-gray-300 rounded-lg bg-gray-50 focus:ring-primary-500 focus:border-primary-500 bg-gray-700 border-gray-600 placeholder-gray-400 text-white focus:ring-primary-500 focus:border-primary-500" placeholder="Search" required="">
                </div>
                </div>
              </div>
              <div class="flex flex-col items-stretch justify-end flex-shrink-0 w-full space-y-2 md:w-auto md:flex-row md:space-y-0 md:items-center md:space-x-3">
                <button type="button" @click="change" class="flex items-center justify-center px-4 py-2 text-sm font-medium text-white rounded-lg bg-primary-700 hover:bg-primary-800 focus:ring-4 focus:ring-primary-300 bg-primary-600 hover:bg-primary-700 focus:outline-none focus:ring-primary-800">
                  Search
                </button>
               </div>
               <p class="flex flex-row text-center text-white">{{ error }}</p>
            </div>
          </div>
        </div>
        <div>
        </div>
        <div class="mx-auto max-w-2xl px-4 py-8 sm:px-6 sm:py-8 lg:max-w-7xl lg:px-8 text-white">      
            <div class="grid grid-cols-1 gap-x-6 gap-y-10 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 xl:gap-x-8">
              <a v-for="video in videos" :key="video.id" @click="toggle(video.url)" class="group">
                <div class="aspect-h-1 shadow-lg shadow-slate-600 aspect-w-1 w-full overflow-hidden rounded-lg bg-gray-200 xl:aspect-h-8 xl:aspect-w-7">
                  <img :src="video.thumbnail" :alt="video.image" class="h-full w-full object-cover object-center group-hover:opacity-75" />
                </div>
                <h3 class="mt-4 text-sm text-gray-200">{{ video.title }}</h3>
                <p class="mt-1 text-md font-medium text-gray-400">{{ video.author.name }}</p>
              </a>
            </div>
          </div>
      </section>
</template>