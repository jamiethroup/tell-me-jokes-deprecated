<template>
<header class="relative z-20 px-4 md:px-0">
  <div class="container mx-auto mb-20">
    <div class="grid grid-cols-12 gap-5">
      <div  class="col-span-8 md:col-span-6 py-5">
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
<main>
  <section class="absolute top-0 left-0 w-full h-full flex items-center justify-center -mt-20">
    <div class="container mx-auto">
      <div v-if="joke.error">
          <h2 class="font-heading font-bold text-3xl mb-10">{{ joke.message }}</h2>
          <h3 class="font-heading text-lg md:text-2xl mb-5">{{ joke.additionalInfo }}</h3>
      </div>
      <div v-if="joke.joke">
        <div class="location-box">
          <h2 class="font-heading text-white font-bold text-xl
          md:text-3xl mb-10">{{ joke.joke }}</h2>
        </div>
      </div>
      <div v-if="joke.setup">
        <div class="location-box">
          <h2 class="font-heading text-lg md:text-2xl mb-5">{{ joke.setup }}</h2>
          <h3 class="font-heading font-bold text-3xl mb-10">{{ joke.delivery }}</h3>
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
  <div class="absolute bottom-6 right-6 leading-none flex flex-row">
    <button
      id="new_joke"
      class="w-full bg-secondary block p-5 rounded-lg font-bold
      text-white font-body transform transition-all duration-300 mr-4
      shadow-md hover:shadow-lg transition-all duration-300 ease-in-out"
      v-on:click="toggleSearch">
      <img width="20"
        class="inline-block"
        src="../assets/icon--search.svg"
        alt="">
    </button>
    <button
      id="new_joke"
      class="w-full bg-secondary block p-5 rounded-lg font-bold
      text-white font-body transform transition-all duration-300 mr-4
      shadow-md hover:shadow-lg transition-all duration-300 ease-in-out"
      v-on:click="toggleFlags">
      <img width="20"
        class="inline-block"
        src="../assets/icon--flag.svg"
        alt="">
    </button>
    <button
      id="new_joke"
      class="w-full bg-secondary block p-5 rounded-lg font-bold
      text-white font-body transform transition-all duration-300
      shadow-md hover:shadow-lg transition-all duration-300 ease-in-out"
      v-on:click="toggleFilter">
      <img width="20"
        class="inline-block"
        src="../assets/icon--filter.svg"
        alt="">
    </button>
  </div>
<!-- START Notification Box -->
  <section id="notification-box"
    class="opacity-0  transition-all ease-linear duration-300
    absolute right-6 p-8 bg-secondary max-w-md rounded-3xl">
    <div class="grid grid-cols-12">
      <div class="col-span-2">
        <div class="bg-blue p-4 rounded-xl inline-block">
          <img width="22"
          src="../assets/icon--copied.svg"
          alt="">
        </div>
      </div>
      <div class="col-span-10 pl-2">
        <h3 data-title class="text-lg text-white font-bold mt-3 text-left">Awesome!</h3>
        <p data-message class="text-base text-tertiary font-semibold text-left">
        You have successfully copied and pasted the joke</p>
      </div>
    </div>
  </section>
  <section
    class="opacity-0 popup-box transition-all ease-linear duration-300
    absolute right-6 p-8 bg-secondary max-w-md rounded-lg shadow-md">
    <div class="grid grid-cols-12 gap-2">
      <div class="col-span-12">
        <img width="40"
          class="inline-block"
          src="../assets/icon--filter.svg"
          alt="">
      </div>
      <div class="col-span-12 pl-2">
        <h3 data-title class="text-lg text-white font-semibold mt-3 text-center">
          Turn on Filters!</h3>
        <p data-message class="text-base text-tertiary font-medium text-center">
        Blacklist jokes from these categories</p>
      </div>
        <div class="col-span-6" v-for="item in availableFilters" :key="item" :for="item">
          <input
            :id="item"
            :value="item"
            :name="item"
            class="-left-full fixed"
            type="checkbox"
            v-model="checkedFilters">
          <label class="block w-full rounded-md
          bg-blue border-solid px-4 py-2 text-xs
          transition-all duration-300 ease-in-out text-white" :for="item">
            {{ item }}
          </label>
        </div>
    </div>
  </section>
  <!-- END Notification Box -->
  <section class="absolute bottom-0 left-0 w-1/2 pb-10 ">
    <div class="container max-w-screen-lg mx-auto ">
      <div class="grid grid-cols-4 gap-5">
        <div>
          <div class="p-4 bg-white rounded-lg shadow-md overflow-auto relative -top-10">
            <h2 class="text-xl text-left font-semibold block mb-3">Filter
              <img src="../assets/icon--filter-eye.svg"
                alt="Filter Eye"
                class="float-right relative top-2 w-6"
              >
            </h2>
          </div>
        </div>
      </div>
      <div class="grid grid-cols-1 gap-5 p-4 bg-white rounded-lg shadow-md
      overflow-auto">
        <div>
          <h2 class="text-xl text-left font-semibold block md:hidden block mb-3">Filter
            <img src="../assets/icon--filter-eye.svg"
              alt="Filter Eye"
              class="float-right relative top-2 w-6"
            >
          </h2>
          <div class="inline-block mr-3" v-for="item in availableFilters" :key="item" :for="item">
            <input
              :id="item"
              :value="item"
              :name="item"
              class="-left-full fixed"
              type="checkbox"
              v-model="checkedFilters">
            <label class="inline-block rounded-full border border-gray-400
            bg-gray-100 border-solid px-4 py-2 my-2 text-xs capitalize
            transition-allduration-300 ease-in-out" :for="item">
              {{ item }}
            </label>
          </div>
          <h2 class="text-xl text-left font-semibold block md:hidden block mb-3">Filter
            <img src="../assets/icon--filter-eye.svg"
              alt="Filter Eye"
              class="float-right relative top-2 w-6"
            >
          </h2>
          <div class="inline-block mr-3"
            v-for="item in availableCategories" :key="item" :for="item">
            <input
              :id="item"
              :value="item"
              :name="item"
              class="-left-full fixed"
              type="checkbox"
              v-model="checkedCategories">
            <label class="inline-block rounded-full border border-gray-400
            bg-gray-100 border-solid px-4 py-2 my-2 text-xs capitalize
            transition-allduration-300 ease-in-out" :for="item">
              {{ item }}
            </label>
          </div>
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
      url_base: 'https://v2.jokeapi.dev/joke',
      query: '',
      darkMode: false,
      availableCategories: ['Programming', 'Miscellaneous', 'Dark', 'Pun', 'Spooky', 'Christmas'],
      availableFilters: ['nsfw', 'religious', 'political', 'racist', 'sexist', 'explicit'],
      languages: ['English', 'German'],
      copyString: '',
      checkedFilters: [],
      checkedCategories: [],
      selectedLanguage: 'English',
      joke: {
      },
    };
  },
  mounted() {
    window.addEventListener('load', this.onWindowLoad);
  },
  methods: {
    handleChange(event) {
      const { value } = event.target;
      this.value = value;
      console.log(this.value);
    },
    fetchJoke() {
      const url = this.urlBuilder();
      console.log(url);
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
      let categories = 'Any';
      let language = 'lang=';

      if (this.checkedFilters.length > 0) {
        this.checkedFilters.forEach((element) => {
          if (this.checkedFilters[this.checkedFilters.length - 1] === element) {
            blacklist = `${blacklist}${element}`;
          } else {
            blacklist = `${blacklist}${element},`;
          }
        });
      }

      if (this.checkedCategories.length > 0) {
        categories = '';
        console.log(this.checkedCategories.length);
        this.checkedCategories.forEach((element) => {
          if (this.checkedCategories[this.checkedCategories.length - 1] === element) {
            categories = `${categories}${element}`;
          } else {
            categories = `${categories}${element},`;
          }
        });
      }

      // Select Language
      if (this.selectedLanguage === 'English') {
        language = `${language}en`;
      } else if (this.selectedLanguage === 'German') {
        language = `${language}de`;
      }
      return `${this.url_base}/${categories}?${blacklist}&${language}`;
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
      this.notificationAlert(
        'Copied!',
        "You've successfully copied the joke. Go share it with your friends!",
        3000,
      );
    },
    notificationAlert(title, message, time) {
      const notificationBox = document.getElementById('notification-box');
      notificationBox.classList.add('open');
      notificationBox.querySelector('[data-title]').innerHTML = title;
      notificationBox.querySelector('[data-message]').innerHTML = message;
      setTimeout(() => notificationBox.classList.remove('open'), time);
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

</style>
