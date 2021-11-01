<template>
  <div id="app">
    <Navbar 
      @checkFav="setVisibility"/>
    <main>
      <router-view 
        :visibility="visibility"/>
    </main>
  </div>
</template>

<script>
import "bootstrap";
import "bootstrap/dist/css/bootstrap.min.css";
import 'bootstrap-vue/dist/bootstrap-vue.css'
import Navbar from "./components/Navbar.vue";



export default {
  name: 'App',
  components: {
    Navbar,
  },
  data() {
    return {
      visibility: "all",
    };
  },
  methods: {
    setVisibility(visibility) {
      return this.visibility = visibility;
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
}
</style>
