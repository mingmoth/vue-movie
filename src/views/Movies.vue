<template>
  <div class="main">
    <!--search bar-->
    <div class="d-flex mt-5 justify-content-between" id="search-bar">
      <form id="search" class="form-inline d-flex">
        <input
          v-model="keyword"
          type="text"
          class="form-control mb-2 mr-sm-2"
          id="search-input"
          placeholder="search name ..."
          style="width: 300px"
        />
        <button
          id="search"
          type="submit"
          class="btn btn-primary mb-2"
          @click.stop.prevent="searchMovie(keyword)"
        >
          Search
        </button>
        <button
          id="clear"
          type="submit"
          class="btn btn-secondary mb-2"
          @click.stop.prevent="clearSearch"
        >
          Clear
        </button>
      </form>
      <div class="d-flex align-items-center" v-if="searchInput">
        Keyword: " {{ searchInput }} " shows {{ filterMovies.length }} results
      </div>
    </div>

    <!-- data-panel -->
    <div class="container mt-5">
      <div class="row" id="data-panel">
        <!-- print movie list here -->
        <div v-for="movie in MovieByPage" :key="movie.id" class="col-sm-3">
          <div class="card mb-2">
            <img :src="movie.image" class="card-img-top" alt="Card image cap" />
            <div class="card-body movie-item-body">
              <h5 class="card-title">{{ movie.title }}</h5>
            </div>
            <!-- "More" button -->
            <div class="card-footer d-flex justify-content-end">
              <b-button
                v-b-modal.show-movie-modal
                @click.stop.prevent="moreInfo(movie.id)"
                id="more"
                variant="primary"
                data-toggle="modal"
                data-target="#show-movie-modal"
              >
                More
              </b-button>
              <button
                v-if="!movie.liked"
                type="button"
                class="btn btn-danger"
                @click="addLiked(movie.id)"
              >
                Like
              </button>
              <button
                v-else
                type="button"
                class="btn btn-outline-danger"
                @click="removeLiked(movie.id)"
              >
                DisLike
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <Pagination
      :total-page="totalPage"
      :current-page="currentPage"
      @set-current-page="setCurrentPage"
    />

    <!-- Modal -->

    <b-modal id="show-movie-modal" size="lg" ok-only>
      <template #modal-header="">
        <h5>{{ movieInfo.title }}</h5>
      </template>
      <template>
        <div class="modal-body" id="show-movie-body">
          <div class="row">
            <img
              :src="movieInfo.image"
              class="col-sm-8"
              id="show-movie-image"
            />
            <div class="col-sm-4">
              <p>
                <em id="show-movie-date"
                  >release at : {{ movieInfo.release_date }}</em
                >
              </p>
              <p id="show-movie-description">{{ movieInfo.description }}</p>
            </div>
          </div>
        </div>
      </template>
    </b-modal>
  </div>
</template>

<script>
const BASE_URL = "https://movie-list.alphacamp.io";
const INDEX_URL = BASE_URL + "/api/v1/movies/";
const POSTER_URL = BASE_URL + "/posters/";

const filters = {
  all: (movies) => movies,
  fav: (movies) => movies.filter((movie) => movie.liked),
};

import axios from "axios";
import Pagination from "../components/Pagination.vue";
export default {
  name: "Movies",
  components: {
    Pagination,
  },
  props: {
    visibility: {
      type: String,
      required: true,
    },
  },
  watch: {
    visibility() {
      this.keyword = "";
      this.searchInput = "";
    },
  },
  data() {
    return {
      movies: [],
      movieInfo: {},
      keyword: "",
      searchInput: "",
      currentPage: 1,
      moviePerPage: 8,
    };
  },
  created() {
    this.fetchMovie();
  },
  computed: {
    filterMovies() {
      return filters[this.visibility](this.movies).filter((movie) =>
        movie.title.toLowerCase().includes(this.searchInput)
      );
    },
    totalPage() {
      const maxPage = Math.ceil(this.filterMovies.length / this.moviePerPage);
      return Array.from(Array(maxPage).keys(), (page) => page + 1);
    },
    MovieByPage() {
      return this.filterMovies.slice(
        (this.currentPage - 1) * this.moviePerPage,
        this.currentPage * this.moviePerPage
      );
    },
  },
  methods: {
    fetchMovie() {
      axios.get(INDEX_URL).then((response) => {
        this.movies = response.data.results.map((movie) => ({
          ...movie,
          image: POSTER_URL + movie.image,
          liked: false,
        }));
      });
    },
    moreInfo(id) {
      this.movieInfo = this.filterMovies.find((movie) => movie.id === id);
    },
    searchMovie(keyword) {
      this.searchInput = keyword.trim().toLowerCase();
      this.currentPage = 1;
    },
    clearSearch() {
      this.keyword = "";
      this.searchInput = "";
      this.currentPage = 1;
    },
    addLiked(id) {
      this.filterMovies.map((movie) => {
        if (movie.id === id) {
          movie.liked = true;
        }
      });
    },
    removeLiked(id) {
      this.filterMovies.map((movie) => {
        if (movie.id === id) {
          movie.liked = false;
        }
      });
    },
    setCurrentPage(page) {
      this.currentPage = page;
    },
  },
};
</script>

<style >
.main {
  margin: 0 auto;
  width: 80%;
}
#search,
#clear {
  margin-left: 8px;
}
#more {
  margin-right: 6px;
}
</style>