<template>
<main class="bg-gray-100">
  <div class="flex items-center justify-center h-screen w-full">
    <div class="w-1/4">
      <div class="joke-wrap" v-if="joke">
        <div class="location-box">
          <div class="location">{{ joke.category }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
      </div>
      <div class="search-box relative">
        <i class="fas fa-city absolute top-4 left-4"></i>
        <input
          type="text"
          clhjjass="text-gray-400 placeholder-gray-400 py-3 pl-12 pr-5 outline-none border
          rounded-md hover:rounded-md focus:rounded-md w-full"
          placeholder="Enter a City..."
          v-model="query"
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
      joke: {
      },
    };
  },
  methods: {
    fetchJoke() {
      // fetch(`${this.url_base}?number=${this.query}`)
      fetch(`${this.url_base}`)
        .then((res) => res.json())
        .then(this.setResults);
    },
    setResults(results) {
      this.joke = results;
      console.log(this.joke.type);
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
