<script setup lang="ts">
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
import { ref, computed, watchEffect } from 'vue'

let inputValue = ref<string>('')
let showSpinner = ref<boolean>(false)

interface jsonResponse {
  name: string
  main?: {
    temp: number
    feels_like: number
    temp_min: number
    temp_max: number
    pressure: number
    humidity: number
  }
  weather?: Array<{
    main: string
    description: string
    icon: string
  }>
}

const API_URL =
  'https://api.openweathermap.org/data/2.5/weather?q={location}&units=metric&appid=042559fc8f13bd0e86e557aa02965a24'

let responseApi: jsonResponse = { name: '' }

const location = computed(() => inputValue.value)

function searchWeather(e: Event) {
  showSpinner.value = false
  inputValue.value.length > 0 ? fetchWeather(inputValue.value) : null
}

function fetchWeather(value: string) {
  showSpinner.value = true

  watchEffect(async () => {
    const url = API_URL.replace('{location}', location.value)
    responseApi = await (await fetch(url)).json()
    showSpinner.value = false
  })
}

function resetForm() {
  inputValue.value = ''
  showSpinner.value = false
  responseApi = { name: '' }
}
</script>

<template>
  <div class="container">
    <main class="app">
      <header class="app__search">
        <div class="search-imput-wrapper">
          <input
            class="search-input"
            type="text"
            placeholder="Insert a city"
            autocomplete="off"
            v-model="inputValue"
            @keyup="searchWeather($event)"
          />
          <div class="search-icon-wrapper">
            <div id="search-icon" :style="{ display: !showSpinner ? '' : 'none' }">🔍</div>
            <img
              src="../assets/spinner.gif"
              class="spinner-icon"
              :style="{ display: showSpinner ? '' : 'none' }"
            />
          </div>
        </div>
        <div class="search-history">history</div>
      </header>
      <article id="weather" class="weather">
        <div class="icon-container" v-if="responseApi.weather">
          <img
            id="weather__icon"
            :src="`http://openweathermap.org/img/wn/${responseApi.weather[0].icon}@4x.png`"
          />
        </div>
        <div v-if="responseApi.weather && responseApi.main">
          <h2 id="weather__main">
            {{ responseApi.weather[0].main + ', ' + responseApi.main.temp + '°' }}
          </h2>
        </div>
        <div>
          <h2 id="weather__city">{{ responseApi.name }}</h2>
        </div>
        <div v-if="responseApi.weather">
          <p id="weather__description">{{ responseApi.weather[0].description }}</p>
        </div>
      </article>
    </main>
    <footer class="footer">
      <div class="footer__color-bar"></div>
      <div class="footer__home-icon" id="home-icon" @click="resetForm">🏠</div>
    </footer>
  </div>
</template>

<style scoped>
.container {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  background-color: var(--beige);
}

.app {
  min-width: 20rem;
  margin-bottom: 5rem;
}

.app__search {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 1rem;
  margin-bottom: 3rem;
  flex-direction: column;
}

.search-imput-wrapper {
  position: relative;
  width: 90%;
}

.search-input {
  box-sizing: border-box;
  width: 100%;
  padding: 0.5em 0.5em;
  border: 2px solid #ccc;
  border-radius: 0.5em;
  outline: 0;
  font-size: 2em;
  color: #333;
  border: var(--green) solid 0.2rem;
  box-shadow: var(--pink) 0 0.2rem;
}

.search-icon-wrapper,
.spinner-icon {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 1rem;
  transform: translateY(-50%);
  font-size: 2.5rem;
}

.search-history {
  background-color: var(--pink);
  color: white;
  font-size: 1.5rem;
  width: 80%;
  padding: 0.5rem;
  border-radius: 0.5rem;
  display: none;
}
.weather {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  margin-bottom: 5rem;
  color: var(--vt-c-black);
}

.icon-container {
  position: relative;
}

#weather__icon {
  max-width: 5rem;
  height: auto;
}

.footer {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 5rem;
  text-align: center;
}

.footer__color-bar {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 2.5rem;
  background-color: var(--green);
}

.footer__home-icon {
  position: absolute;
  background-color: white;
  padding: 0.1rem;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  font-size: 2.5rem;
  border-radius: 1rem;
  border: var(--green) solid 0.2rem;
  box-shadow: var(--pink) 0 0.2rem;
  cursor: pointer;
}

@media only screen and (min-width: 768px) {
  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 80%;
  }
  .weather {
    flex-direction: row;
    gap: 2rem;
  }
}
</style>