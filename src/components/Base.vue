<template>
<div class="absolute top-0 left-0 w-full h-full">
  <div class="flex items-center justify-center h-full w-full">
    <div class="container mx-auto">
      <div class="grid grid-col-12">
        <div class="col-span-12">
          <h1 class="font-heading text-5xl text-red-400">{{ msg }}</h1>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Base',
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
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
