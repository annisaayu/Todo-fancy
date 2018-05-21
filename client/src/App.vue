<template>
  <div id="app" class="background-image">
    <div class="title">
      <h3 class="center-align logo">
        <span class="pink-text text-lighten-1">UI Todo</span>
      </h3>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
html {
  font-family: 'Quicksand', sans-serif;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.logo {
  font-family: 'Merienda One', cursive;
}
h1, h2, h3, h4, h5, h6 {
  font-family: 'Tillana', cursive;
  color: #0277bd;
}
.background-image {
  background:url("./assets/pattern.png");
  background-repeat: repeat;
  display: block;
  left: 0;
  position: fixed;
  right: 0;
  z-index: 1;
  height: 100%;
}
.card-title {
  font-weight: bolder;
}
.title h3 {
  margin: 0;
  padding: 40px 0 20px 0;
  position: relative;
  z-index: 2;
}
.card {
  top: 40px;
  position: relative;
  z-index: 2;
}
</style>
