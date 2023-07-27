<script setup lang="ts">
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'

let showSpinner:boolean = false

function searchWeather(event: Event) {
  console.log("on searchWeather():", showSpinner)
  showSpinner = true
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
            @keydown="searchWeather($event)"
          />
          <div class="search-icon-wrapper">
            <div id="search-icon" :style="{ 'display': !showSpinner ? 'visible' : 'none'}">
            🔍
          </div>
            <img src="../assets/spinner.gif" :style="{ 'display': showSpinner ? 'visible' : 'none'}" />
          </div>
        </div>
        <div class="search-history">history</div>
      </header>
      <article id="weather" class="weather">
        <div class="icon-container">
          <img id="weather__icon" src="" hidden />
        </div>
        <div>
          <h2 id="weather__main"></h2>
        </div>
        <div>
          <h2 id="weather__city">showSpinner:{{ showSpinner }}</h2>
        </div>
        <div>
          <p id="weather__description"></p>
        </div>
      </article>
      <footer class="footer">
        <div class="footer__color-bar"></div>
        <div class="footer__home-icon" id="home-icon">🏠</div>
      </footer>
    </main>
  </div>
</template>

<style scoped>
.container {
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

.search-icon-wrapper {
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
}

.icon-container {
  position: relative;
}

#weather__icon {
  max-width: 5rem;
  height: auto;
}

.footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 5rem;
  text-align: center;
}

.footer__color-bar {
  position: fixed;
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