<template>
<header>
  <div class="container mx-auto mb-20">
    <div class="grid grid-cols-2 gap-5">
      <div class="py-5">
        <h1 class="text-gray-800 uppercase font-heading text-xl font-bold
         text-left">Tell Me Jokes</h1>
      </div>
      <div class="py-5 flex items-center justify-end">
        <div class="inline-block">
        <template v-if="darkMode === false">
          <img height="20" width="20"
          v-on:click="toggleDarkMode('dark')" src="../assets/icon--moon.svg" alt="">
        </template>
        <template v-else>
          <img width="20"
          v-on:click="toggleDarkMode('light')" src="../assets/icon--sun.svg"
          alt="">
        </template>
          </div>
        <div class="inline-block"><img width="20"
         src="../assets/icon--globe.svg" alt=""></div>
      </div>
    </div>
  </div>
</header>
<main>
  <section class="pt-40 pb-60">
    <div class="container mx-auto">
      <div v-if="joke.joke">
        <div class="location-box">
          <h2 class="font-heading font-bold text-3xl mb-10">{{ joke.joke }}</h2>
        </div>
      </div>
      <div v-if="joke.setup">
        <div class="location-box">
          <h2 class="font-base text-2xl mb-5">{{ joke.setup }}</h2>
          <h2 class="font-heading font-bold text-3xl mb-10">{{ joke.delivery }}</h2>
        </div>
      </div>
      <div class="flex items-center justify-center">
        <div class="m-4">
          <button
            class="bg-blue-300 hover:bg-blue-400 block py-5 px-10 rounded-full font-bold
            text-white font-heading transform transition-all duration-300"
            v-on:click="copyJoke">Copy
            <img width="14" class="inline-block ml-2 relative top-0.5"
            src="../assets/icon--copy.svg"
            alt=""></button>
        </div>
        <div class="m-4">
          <button
            class="bg-green-400 hover:bg-green-500 block py-5 px-10 rounded-full font-bold
            text-white font-heading transform transition-all duration-300"
            v-on:click="fetchJoke">New Joke
            <img width="14" class="inline-block ml-2 relative top-0.5"
            src="../assets/icon--plus.svg"
            alt=""></button>
        </div>
      </div>
    </div>
  </section>
  <section class="bg-gray-800">
    <div class="container mx-auto">
      <div class="w-full p-4 bg-white rounded-lg shadow-md relative -top-20">
        <div class="inline-block m-1" v-for="item in availableFilters" :key="item" :for="item">
          <input
            :id="item"
            :value="item"
            :name="item"
            class="-left-full fixed"
            type="checkbox"
            v-model="checkedFilters">
          <label class="inline-block rounded-full border border-gray-400
          bg-gray-100 border-solid px-4 py-3 capitalize" :for="item">
            {{ item }}
          </label>
        </div>
        <div class="search-box relative">
          <i class="fas fa-city absolute top-4 left-4"></i>
          <input
            type="text"
            class="hidden text-gray-400 placeholder-gray-400 py-3 pl-12 pr-5 outline-none border
            rounded-md hover:rounded-md focus:rounded-md w-full"
            placeholder="Enter a City..."
          />
        </div>
      </div>
    </div>
  </section>
</main>
<button class="absolute bottom-5 right-5 bg-white p-3 block rounded-full shadow-md
hover:scale-110 transition-all duration-300"
  v-on:click="fetchFilters">
    <img width="25" src="../assets/icon--filter.svg" alt="">
</button>
<aside class="absolute top-0 left-0 transform translate-y-3/4 w-full h-3/4 bg-red-200
 hidden"></aside>
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
      copyString: '',
      checkedFilters: [],
      joke: {
      },
    };
  },
  mounted() {
    window.addEventListener('load', this.onWindowLoad);
  },
  methods: {
    fetchJoke(link) {
      if (link !== null) {
        const url = this.urlBuilder(link);
        console.log(url);
        fetch(url)
          .then((res) => res.json())
          .then(this.setResults);
      } else {
        console.log('SECOND');
        fetch(`${this.url_base}`)
          .then((res) => res.json())
          .then(this.setResults);
      }
    },
    urlBuilder(link) {
      const newLink = '';
      const urlParams = new URLSearchParams(link);
      const range = urlParams.get('idRange');
      const blacklistFlags = urlParams.get('blacklistFlags');
      console.log(range, blacklistFlags);
      if (range !== null) {
        this.newLink = `${newLink}?idRange=${range}`;
      }
      if (blacklistFlags !== null) {
        this.newLink = `${newLink}&blacklistFlags=${blacklistFlags}`;
      }
      return `${this.url_base}${this.newLink}`;
    },
    onWindowLoad() {
      const url = window.location.search;
      if (url.indexOf('?') === -1) {
        this.fetchJoke();
      } else {
        this.fetchJoke(url);
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
        document.body.classList.add('dark');
      } else {
        this.darkMode = false;
        document.body.classList.remove('dark');
      }
    },
  },
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped lang='stylus'>
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
  color #f00
  font-style normal

</style>
