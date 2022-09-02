<template>
<header class="relative z-20 px-4 md:px-0">
  <div class="container mx-auto mb-20">
    <div class="grid grid-cols-12 gap-5">
      <div  class="col-span-8 md:col-span-6 py-5">
        <h1 id="logo" :class="darkMode == true ? 'text-black' : 'text-red'"
        class="text-gray-800 uppercase font-heading text-xl font-bold
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
  <div class="w-20 h-20 bg-blue"></div>
  <div class="w-20 h-20 bg-primary"></div>
  <div class="w-20 h-20 bg-secondary"></div>
  <div class="w-20 h-20 bg-tertiary"></div>
  <div class="w-20 h-20 bg-quaternary"></div>
  <div class="w-20 h-20 bg-quinary"></div>
  <section class="absolute top-0 left-0 w-full h-full flex items-center justify-center -mt-20">
    <div class="container mx-auto">
      <div v-if="joke.error">
          <h2 class="font-heading font-bold text-3xl mb-10"
          :class="darkMode == true ? 'text-white' : 'text-blue'">{{ joke.message }}</h2>
          <h3 class="font-heading text-lg md:text-2xl mb-5"
          :class="darkMode == true ? 'text-white' : 'text-blue'">{{ joke.additionalInfo }}</h3>
      </div>
      <div v-if="joke.joke">
        <div class="location-box">
          <h2 class="font-heading font-bold text-xl
          md:text-3xl mb-10"
          :class="darkMode == true ? 'text-white' : 'text-blue'">{{ joke.joke }}</h2>
        </div>
      </div>
      <div v-if="joke.setup">
        <div class="location-box">
          <h2 class="font-heading text-lg md:text-2xl mb-5
          " :class="darkMode == true ? 'text-white' : 'text-blue'">{{ joke.setup }}</h2>
          <h3 class="font-heading font-bold text-3xl mb-10
          " :class="darkMode == true ? 'text-white' : 'text-blue'" >{{ joke.delivery }}</h3>
        </div>
      </div>
      <div class="block md:flex items-center justify-center">
        <div class="m-4">
          <button
            id="copy_joke"
            class="w-full bg-blue hover:bg-blue-400 block py-3 px-10 rounded-full font-bold
            text-white font-body transform transition-all duration-300"
            v-on:click="copyJoke">
            <span>Copy Joke</span>
            <img width="14" class="inline-block ml-2 relative"
            src="../assets/icon--copy.svg"
            alt=""></button>
        </div>
        <div class="m-4">
          <button
            id="new_joke"
            class="w-full bg-green-400 hover:bg-green-500 block py-3 px-10 rounded-full font-bold
            text-white font-body transform transition-all duration-300"
            v-on:click="this.fetchJoke()">
            <span>New Joke</span>
            <img width="14" class="inline-block ml-2 relative"
            src="../assets/icon--plus.svg"
            alt=""></button>
        </div>
      </div>
    </div>
  </section>
  <div class="absolute bottom-6 right-6 leading-none flex flex-row">
    <button
      class="w-full bg-gray-800  block p-3 rounded-lg font-bold
    text-white font-body transform transition-all duration-300 mr-4
      shadow-md hover:shadow-lg transition-all duration-300 ease-in-out"
      v-on:click="togglePopup('search_popup')">
      <img width="20"
        class="inline-block"
        src="../assets/icon--search.svg"
        alt="">
        <div class="w-4 font-medium h-4 flex items-center justify-center hidden
        bg-red-500 text-white text-xs rounded-2xl absolute -top-1 -right-1 px-1"></div>
    </button>
    <button
      class="w-full bg-gray-800  block p-3 rounded-lg font-bold
      text-white font-body transform transition-all duration-300 mr-4
      shadow-md hover:shadow-lg transition-all duration-300 ease-in-out"
      v-on:click="togglePopup('flag_popup')">
      <img width="20"
        class="inline-block"
        src="../assets/icon--flag.svg"
        alt="">
        <div class="w-4 font-medium h-4 flex items-center justify-center hidden
        bg-red-500 text-white text-xs rounded-2xl absolute -top-1 -right-1 px-1"></div>
    </button>
    <button
      class="w-full bg-gray-800  block p-3 rounded-lg font-bold
      text-white font-body transform transition-all duration-300
      shadow-md hover:shadow-lg transition-all duration-300 ease-in-out"
      v-on:click="togglePopup('filter_popup')">
      <img width="20"
        class="inline-block"
        src="../assets/icon--filter.svg"
        alt="">
        <div class="w-4 font-medium h-4 flex items-center justify-center hidden
        bg-red-500 text-white text-xs rounded-2xl absolute -top-1 -right-1 px-1"></div>
    </button>
  </div>
<!-- START Notification Box -->
  <section id="notification-box"
    class="settings-box opacity-0  transition-all ease-linear duration-300
    absolute right-6 p-5 bg-gray-800  max-w-md rounded-3xl">
    <div class="grid grid-cols-12">
      <div class="col-span-2">
        <div class="bg-blue p-4 rounded-xl inline-block">
          <img width="22"
          src="../assets/icon--copied.svg"
          alt="">
        </div>
      </div>
      <div class="col-span-10 pl-2">
        <h3 data-title class="text-base text-white font-bold text-left">Awesome!</h3>
        <p data-message class="text-sm text-tertiary font-semibold text-left">
        You have successfully copied and pasted the joke</p>
      </div>
    </div>
  </section>
  <section
    id="filter_popup" class="settings-box opacity-0 overflow-hidden popup-box transition-all
    ease-linear duration-300 fixed md:absolute md:right-6 py-8 px-4 bg-gray-800  max-w-md
    rounded-lg shadow-md">
    <div class="grid grid-cols-12 gap-2">
      <div class="col-span-12">
        <img width="40"
          class="inline-block"
          src="../assets/icon--filter.svg"
          alt="">
      </div>
      <div class="col-span-12 pl-2">
        <h3 data-title class="text-lg text-white font-semibold mt-0 mb-2 text-center">
          Turn on Filters!</h3>
        <p data-message class="text-sm text-tertiary font-medium text-center mb-6">
        Blacklist jokes from these categories</p>
      </div>
    </div>
    <div class="block">
        <div class="inline-block mb-4 mr-2"
        v-for="item in availableFilters" :key="item" :for="item">
          <input
            :id="item"
            :value="item"
            :name="item"
            class="-left-full fixed"
            type="checkbox"
            v-model="checkedFilters">
          <label class="block w-full rounded-3xl flex items-center justify-center
          bg-primary border-solid px-4 py-2 text-sm capitalize font-medium
          transition-all duration-300 ease-in-out text-tertiary" :for="item">
            <span>{{ item }}</span>
            <span class="pl-2">
              <svg viewBox="0 0 34 34" xmlns="http://www.w3.org/2000/svg" xml:space="preserve"
               fill-rule="evenodd" clip-rule="evenodd" stroke-linejoin="round"
               stroke-miterlimit="2" class="w-4">
               <path d="M16.987 33.973C7.668 33.973 0 26.305 0 16.987 0 7.668 7.668 0
               16.987 0c9.318 0 16.986 7.668 16.986 16.987 0 9.318-7.668 16.986-16.986
                16.986Zm0-3.397c7.454 0 13.589-6.135 13.589-13.589 0-7.455-6.135-13.59
                -13.589-13.59-7.455 0-13.59 6.135-13.59 13.59 0 7.454 6.135 13.589 13.59
                13.589Zm1.698-15.288h3.398c.931 0 1.698.767 1.698 1.699 0 .931-.767
                1.698-1.698 1.698h-3.398v3.398c0 .931-.767 1.698-1.698 1.698a1.707
                1.707 0 0 1-1.699-1.698v-3.398h-3.397a1.707 1.707 0 0
                1-1.699-1.698c0-.932.767-1.699 1.699-1.699h3.397v-3.397c0-.932.767-1.699
                1.699-1.699.931 0 1.698.767 1.698 1.699v3.397Z" fill="#2d325a"
                fill-rule="nonzero"/></svg>
            </span>
          </label>
        </div>
    </div>
  </section>
  <section
    id="flag_popup" class="settings-box opacity-0 overflow-hidden popup-box transition-all
    ease-linear duration-300 absolute right-6 py-8 px-4 bg-secondary max-w-md
    rounded-lg shadow-md">
    <div class="grid grid-cols-12 gap-2">
      <div class="col-span-12">
        <img width="40"
          class="inline-block"
          src="../assets/icon--flag.svg"
          alt="">
      </div>
      <div class="col-span-12 pl-2">
        <h3 data-title class="text-lg text-white font-semibold mt-0 mb-2 text-center">
          Choose a category!</h3>
        <p data-message class="text-sm text-tertiary font-medium text-center mb-6">
        Please select categories below for jokes to appear from.</p>
      </div>
    </div>
    <div class="block">
        <div class="inline-block mb-4 mr-2 cursor-pointer"
        v-for="item in availableCategories" :key="item" :for="item">
          <input
            :id="item"
            :value="item"
            :name="item"
            class="-left-full fixed"
            type="checkbox"
            v-model="checkedCategories">
          <label class="block w-full rounded-3xl flex items-center justify-center
          bg-primary border-solid px-4 py-2 text-sm capitalize font-medium
          transition-all duration-300 ease-in-out text-tertiary" :for="item">
            <span>{{ item }}</span>
            <span class="pl-2">
              <svg viewBox="0 0 34 34" xmlns="http://www.w3.org/2000/svg" xml:space="preserve"
               fill-rule="evenodd" clip-rule="evenodd" stroke-linejoin="round"
               stroke-miterlimit="2" class="w-4">
               <path d="M16.987 33.973C7.668 33.973 0 26.305 0 16.987 0 7.668 7.668 0
               16.987 0c9.318 0 16.986 7.668 16.986 16.987 0 9.318-7.668 16.986-16.986
                16.986Zm0-3.397c7.454 0 13.589-6.135 13.589-13.589 0-7.455-6.135-13.59
                -13.589-13.59-7.455 0-13.59 6.135-13.59 13.59 0 7.454 6.135 13.589 13.59
                13.589Zm1.698-15.288h3.398c.931 0 1.698.767 1.698 1.699 0 .931-.767
                1.698-1.698 1.698h-3.398v3.398c0 .931-.767 1.698-1.698 1.698a1.707
                1.707 0 0 1-1.699-1.698v-3.398h-3.397a1.707 1.707 0 0
                1-1.699-1.698c0-.932.767-1.699 1.699-1.699h3.397v-3.397c0-.932.767-1.699
                1.699-1.699.931 0 1.698.767 1.698 1.699v3.397Z" fill="#2d325a"
                fill-rule="nonzero"/></svg>
            </span>
          </label>
        </div>
    </div>
  </section>
  <section
    id="search_popup" class="settings-box opacity-0 overflow-hidden popup-box transition-all
    ease-linear duration-300 absolute right-6 p-4 bg-secondary max-w-md
    rounded-lg shadow-md">
    <div class="flex items-center justify-center">
      <div class="flex-grow">
        <input type="text"
        placeholder="Enter your keyword.." class="w-full p-2 bg-transparent
        border-2 border-primary focus:outline-none text-sm text-gray-300 font-heading
        placeholder-gray-300"
         name="search" id="search">
      </div>
      <div class="flex-grow py-2 px-2 bg-primary h-full border-2 border-solid border-primary
      text-white font-semibold cursor-pointer text-sm font-heading"
      v-on:click="fetchJoke()">
        Search Jokes
      </div>
    </div>
  </section>
  <!-- END Notification Box -->
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
      availableFilters: ['explicit', 'nsfw', 'political', 'racist', 'religious', 'sexist'],
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
    },
    togglePopup(id) {
      const settingBoxes = document.getElementsByClassName('settings-box');
      Array.from(settingBoxes).forEach((box) => {
        box.classList.remove('active');
      });
      document.getElementById(`${id}`).classList.toggle('active');
    },
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
      let categories = 'Any';
      let language = 'lang=';
      let searchQuery = document.getElementById('search').value;

      if (this.checkedFilters.length > 0) {
        this.checkedFilters.forEach((element) => {
          if (this.checkedFilters[this.checkedFilters.length - 1] === element) {
            blacklist = `${blacklist}${element}`;
          } else {
            blacklist = `${blacklist}${element},`;
          }
        });
      }
      if (searchQuery.length > 0) {
        searchQuery = `contains=${searchQuery}`;
      }

      if (this.checkedCategories.length > 0) {
        categories = '';
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
      return `${this.url_base}/${categories}?${searchQuery}&${blacklist}&${language}`;
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
      const settingBoxes = document.getElementsByClassName('settings-box');
      if (this.joke.type === 'twopart') {
        this.copyString = `${this.joke.setup}....${this.joke.delivery}`;
        this.copyFunction(this.copyString);
      } else {
        this.copyString = `${this.joke.joke}`;
        this.copyFunction(this.copyString);
      }
      Array.from(settingBoxes).forEach((box) => {
        box.classList.remove('active');
      });
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
