<template>
  <nav class="relative">
      <p class="absolute top-10 left-10 w-3/12 font-bold">Tell us what ingredients you have in the fridge and we will choose dishes that can be made from them!</p>
    <h1 class="text-3xl font-bold">
      FridgeRecipe
    </h1>
  </nav>
  <router-view/>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
  height:100vh;
  width:100vw;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
