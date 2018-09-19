<template>
  <div id="app">
    <nav>
      <h1>The Store</h1>
    </nav>
    <Store/>
  </div>
</template>

<script>
import Store from './components/Store.vue';

export default {
  name: 'app',
  components: {
    Store,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Quicksand:400,700");
@import url("https://fonts.googleapis.com/css?family=Raleway:300i");

body,
html {
  height: 100%;
  margin: 0;
}

body {
  font-family: Quicksand, sans-serif;
}
nav h1 {
  margin: 0;
}

nav {
  text-align: center;
  font-size: 1.5em;
  background: linear-gradient(whitesmoke, snow);
}
</style>
