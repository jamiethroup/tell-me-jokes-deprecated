<template>
  <Base />
  <Filter />
</template>

<script>
import Base from './components/Base.vue';
import Filter from './components/Filter.vue';

export default {
  name: 'App',
  components: {
    Base, Filter,
  },
};
</script>

<style lang="stylus">
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50

</style>
