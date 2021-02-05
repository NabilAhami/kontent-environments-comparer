<template>
  <Header title="Kontent Environments Comparer" />
  <Comparer class="comparrer"/>
</template>

<script>
import Comparer from "./components/Comparer.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Header,
    Comparer,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

html, body {
  margin: 0;
  padding: 0;
}

.comparrer {
  padding: 0 1em;;
}
</style>
