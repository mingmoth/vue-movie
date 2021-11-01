<template>
  <div class="header bg-light">
    <nav class="navbar navbar-expand-lg navbar-light ">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Movie Lists</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNavAltMarkup"
          aria-controls="navbarNavAltMarkup"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <a class="nav-link active" aria-current="page" href="#" @click.stop.prevent="setVisibility('all')">Home</a>
            <a class="nav-link" href="#" @click.stop.prevent="setVisibility('fav')">Favorites</a>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  methods: {
    setVisibility(visibility) {
      this.$emit('checkFav', visibility)
    }
  }
}
</script>

<style>
.header {
  margin: 0;
  width: 100%;
  display: flex;
  justify-content: center;
}
.navbar {
  width: 80%;
}
</style>