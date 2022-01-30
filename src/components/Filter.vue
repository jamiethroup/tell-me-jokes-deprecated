<script>
export default {
  data() {
    return {
      availableFilters: ['nsfw', 'religious', 'political', 'racist', 'sexist', 'explicit'],
    };
  },
};
</script>

<template>
<div class="greeting">Hello</div>
          <div class="inline-block mr-3" v-for="item in availableFilters" :key="item" :for="item">
            <input
              :id="item"
              :value="item"
              :name="item"
              class="-left-full fixed"
              type="checkbox">
            <label class="inline-block rounded-full border border-gray-400
            bg-gray-100 border-solid px-4 py-2 my-2 text-xs capitalize
            transition-allduration-300 ease-in-out" :for="item"
              @click="(event) => this.$emit('inputChange', event)">
              {{ item }}
            </label>
          </div>
</template>

<style>
.greeting {
  color: red;
  font-weight: bold;
}
</style>
