<template>
  <nav class="d-flex justify-content-center" aria-label="Page navigation example">
  <ul class="pagination">

    <!-- <li class="page-item">
      <a class="page-link" href="#" aria-label="Previous">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li> -->
    <li v-for="page in totalPage" :key="page" class="page-item"><a @click.stop.prevent="setCurrentPage(page)" class="page-link" href="#">{{page}}</a></li>

    <!-- <li class="page-item">
      <a class="page-link" href="#" aria-label="Next">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li> -->

  </ul>
</nav>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      default: 1
    },
    totalPage: {
      type: Array,
      required: true
    }
  },
  methods: {
    setCurrentPage(page) {
      this.$emit('set-current-page', page)
    }
  }
}
</script>