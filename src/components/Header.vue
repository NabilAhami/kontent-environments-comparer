<template>
  <header>
    <nav>
      <img width="50" alt="Kontent logo" src="../assets/logo.svg" />
      <span class="title">{{title}}</span>
    </nav>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: String,
  },
};
</script>

<style scoped>
nav {
  display: flex;
  align-items: center;
}

img {
  padding: 1em;
}

.title {
  font-weight: bold;
}

header {
  box-shadow: 0 -30px 40px 30px black;
  position: sticky;
  margin-bottom: 30px;;
}
</style>