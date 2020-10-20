<template>
  <div id="app">
    <Books/>
  </div>
</template>

<script>
import Books from '@/components/Books.vue'

export default {
  name: 'App',
  components: {
    Books
  }
}
</script>

<style lang="scss">
*,
*::before,
*::after {
  box-sizing: border-box;
}

body,
h1,
h2,
h3,
h4,
p {
  margin: 0;
}

body {
  font-family: "Roboto";
}
</style>
