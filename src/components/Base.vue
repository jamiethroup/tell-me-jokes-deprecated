<template>
<header class="relative z-20 px-4 md:px-0">
  <div class="container mx-auto mb-20">
    <div class="grid grid-cols-12 gap-5">
      <div class="col-span-8 md:col-span-6 py-5">
        <h1 id="logo" class="text-gray-800 uppercase font-heading text-xl font-bold
         text-left">Tell Me Jokes</h1>
      </div>
      <div class="col-span-4 md:col-span-6 py-5 flex items-center justify-end">
        <div class="inline-block pr-3 border-r border-gray-800 border-solid cursor-pointer">
        <template v-if="darkMode === false">
          <img height="20" width="20"
          v-on:click="toggleDarkMode('dark')" src="../assets/icon--moon.svg" alt="">
        </template>
        <template v-else>
          <img class="ml-3" width="20"
          v-on:click="toggleDarkMode('light')" src="../assets/icon--sun.svg"
          alt="">
        </template>
          </div>
        <div class="flex items-center justify-center pl-3">
        <template v-if="darkMode === false">
          <img width="20"
            class="inline-block"
            src="../assets/icon--globe.svg"
            alt="">
        </template>
        <template v-else>
          <img width="20"
            class="inline-block"
            src="../assets/icon--globe-white.svg"
            alt="">
        </template>
         <select name="language"
          class="appearance-none cursor-pointer pl-2 bg-transparent text-sm outline-none"
          id="language"
          @change="changeLanguage"
          v-model="selectedLanguage">
            <option
              class="float-left mr-3" v-for="item in languages" :key="item" :for="item" >
                {{ item }}
            </option>
         </select>
         </div>
      </div>
    </div>
  </div>
</header>
<main class="absolute z-10 top-0 left-0 w-screen h-screen flex items-center justify-center px-10">
  <section class="md:pt-40 md:pb-60">
    <div class="container mx-auto">
      <div v-if="joke.joke">
        <div class="location-box">
          <h2 class="font-heading font-bold text-xl md:text-3xl mb-10">{{ joke.joke }}</h2>
        </div>
      </div>
      <div v-if="joke.setup">
        <div class="location-box">
          <h2 class="font-heading text-lg md:text-2xl mb-5">{{ joke.setup }}</h2>
          <h2 class="font-heading font-bold text-3xl mb-10">{{ joke.delivery }}</h2>
        </div>
      </div>
      <div class="block md:flex items-center justify-center">
        <div class="m-4">
          <button
            id="copy_joke"
            class="w-full bg-blue-300 hover:bg-blue-400 block py-5 px-10 rounded-full font-bold
            text-white font-body transform transition-all duration-300"
            v-on:click="copyJoke">
            <span>Copy Joke</span>
            <img width="14" class="inline-block ml-2 relative top-0.5"
            src="../assets/icon--copy.svg"
            alt=""></button>
        </div>
        <div class="m-4">
          <button
            id="new_joke"
            class="w-full bg-green-400 hover:bg-green-500 block py-5 px-10 rounded-full font-bold
            text-white font-body transform transition-all duration-300"
            v-on:click="fetchJoke">
            <span>New Joke</span>
            <img width="14" class="inline-block ml-2 relative top-0.5"
            src="../assets/icon--plus.svg"
            alt=""></button>
        </div>
      </div>
    </div>
  </section>
</main>
</template>

<script>
export default {
  name: 'JokeApp',
  data() {
    return {
      url_base: 'https://v2.jokeapi.dev/joke/Any',
      query: '',
      darkMode: false,
      availableFilters: ['nsfw', 'religious', 'political', 'racist', 'sexist', 'explicit'],
      languages: ['English', 'German'],
      copyString: '',
      checkedFilters: [],
      selectedLanguage: 'English',
      joke: {
      },
    };
  },
  mounted() {
    window.addEventListener('load', this.onWindowLoad);
  },
  methods: {
    fetchJoke() {
      const url = this.urlBuilder();
      fetch(url)
        .then((res) => res.json())
        .then(this.setResults);
    },
    fetchInitialJoke(link) {
      if (link !== undefined) {
        fetch(link)
          .then((res) => res.json())
          .then(this.setResults);
      } else {
        fetch(`${this.url_base}`)
          .then((res) => res.json())
          .then(this.setResults);
      }
    },
    changeLanguage() {
      if (this.selectedLanguage === 'English') {
        document.getElementById('logo').textContent = 'TELLMEJOKES';
        document.querySelectorAll('#new_joke span')[0].innerHTML = 'New Joke';
        document.querySelectorAll('#copy_joke span')[0].innerHTML = 'Copy Joke';
      } else if (this.selectedLanguage === 'German') {
        document.getElementById('logo').textContent = 'Erzähl mir Witze';
        document.querySelectorAll('#new_joke span')[0].innerHTML = 'Neuer Witz';
        document.querySelectorAll('#copy_joke span')[0].innerHTML = 'Witz kopieren';
      }
    },
    urlBuilder() {
      // Blaclist
      let blacklist = 'blacklistFlags=';
      let language = 'lang=';

      if (this.checkedFilters !== '') {
        this.checkedFilters.forEach((element) => {
          if (this.checkedFilters[this.checkedFilters.length - 1] === element) {
            blacklist = `${blacklist}${element}`;
          } else {
            blacklist = `${blacklist}${element},`;
          }
        });
      }

      // Select Language
      if (this.selectedLanguage === 'English') {
        language = `${language}en`;
      } else if (this.selectedLanguage === 'German') {
        language = `${language}de`;
      }
      return `${this.url_base}?${blacklist}&${language}`;
    },
    onWindowLoad() {
      const url = window.location.search;
      if (url.indexOf('?') === -1) {
        this.fetchJoke();
      } else {
        this.fetchInitialJoke(url);
      }
    },
    setResults(results) {
      this.joke = results;
    },
    copyJoke() {
      if (this.joke.type === 'twopart') {
        this.copyString = `${this.joke.setup}....${this.joke.delivery}`;
        this.copyFunction(this.copyString);
      } else {
        this.copyString = `${this.joke.joke}`;
        this.copyFunction(this.copyString);
      }
    },
    copyFunction(joke) {
      const dummy = document.createElement('TEXTAREA');
      dummy.setAttribute('id', 'text');
      dummy.value = joke;
      document.body.prepend(dummy);
      dummy.select();
      document.execCommand('copy');
      document.body.removeChild(dummy);
    },
    toggleDarkMode(mode) {
      if (mode === 'dark') {
        this.darkMode = true;
        document.body.classList.add('bg-gray-900');
        document.body.classList.remove('bg-gray-300');
      } else {
        this.darkMode = false;
        document.body.classList.remove('bg-gray-900');
        document.body.classList.add('bg-gray-300');
      }
    },
  },
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style lang='stylus'>
h3
  margin 40px 0 0

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983

input[type=checkbox]:checked + label
  font-style: normal;
  background: #f13232;
  color: white;
  border-color: #640808;

body.bg-gray-900 h1, body.bg-gray-900 h2, body.bg-gray-900 select
  color #d1d5db!important

</style>
