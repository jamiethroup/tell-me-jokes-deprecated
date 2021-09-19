<template>
<main class="bg-gray-100">
  <div class="flex items-center justify-center h-screen w-full flex-col">
    <img width="250" src="../assets/logo.svg" alt="">
    <div class=" max-w-full md:max-w-half w-screen p-4 bg-white rounded-xl shadow-md">
    <label v-for="item in availableFilters" :key="item" :for="item">{{ item }}
        <input class="form-tick appearance-none h-6 w-6 border border-gray-300 rounded-md
      checked:bg-blue-600 checked:border-transparent focus:outline-none"
          :id="item"
          :value="item"
          :name="item"
          type="checkbox"
          v-model="checkedFilters">
    </label>
      <div v-if="joke.joke">
        <div class="location-box">
          Single Joke
          <h2 class="font-base text-3xl">{{ joke.joke }}</h2>
        </div>
      </div>
      <div v-if="joke.setup">
        <div class="location-box">
          Two Part
          <h2 class="font-base text-3xl">{{ joke.setup }}</h2>
          <h2 class="font-base text-xl">{{ joke.delivery }}</h2>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
      </div>
      <div class="search-box relative">
        <i class="fas fa-city absolute top-4 left-4"></i>
        <input
          type="text"
          class="text-gray-400 placeholder-gray-400 py-3 pl-12 pr-5 outline-none border
          rounded-md hover:rounded-md focus:rounded-md w-full"
          placeholder="Enter a City..."
        />
        <button
          class="bg-red-500 block py-5 px-10 rounded-full uppercase font-bold
          text-white hover:bg-red-600 transform transition-all duration-300"
          v-on:click="fetchJoke">Submit</button>
      </div>
    </div>
  </div>
</main>
</template>

<script>
export default {
  name: 'JokeApp',
  data() {
    return {
      url_base: 'https://v2.jokeapi.dev/joke/Any',
      query: '',
      availableFilters: ['nsfw', 'religious', 'political', 'racist', 'sexist', 'explicit'],
      checkedFilters: [],
      joke: {
      },
    };
  },
  methods: {
    fetchJoke() {
      fetch(`${this.url_base}?number=1&blacklistFlags=${this.checkedFilters}`)
        .then((res) => res.json())
        .then(this.setResults);
    },
    updateFilters() {
    },
    setResults(results) {
      this.joke = results;
    },
    dateBuilder() {
      const d = new Date();
      const months = ['January', 'Febuary', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
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
</style>
