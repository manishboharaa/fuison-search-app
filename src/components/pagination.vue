<template>
  <section>
    <ul class="nav-pagination">
      <li @click="setPage(1)" v-if="currentPage !== 1" class="first">First</li>
      <li @click="setPage(currentPage - 1)" v-if="currentPage !== 1">&#60;</li>
      <li v-for="(pageNumber, index) in totalPages" :key="index" v-if="Math.abs(pageNumber - currentPage) < 3 || pageNumber == totalPages - 1 || pageNumber == 0" @click="setPage(pageNumber)" :class="{current: currentPage === pageNumber, last: (pageNumber == totalPages - 1 && Math.abs(pageNumber - currentPage) > 3), first:(pageNumber == 0 && Math.abs(pageNumber - currentPage) > 3)}">
        {{ pageNumber }}
      </li>
      <li v-if="totalPages !== currentPage" @click="setPage(currentPage + 1)">&#62;</li>
      <li v-if="totalPages !== currentPage" @click="setPage(totalPages)" class="last">Last</li>
    </ul>
  </section>
</template>

<script>
export default {
  name: 'pagination',
  props: ['resultCount', 'itemsPerPage', 'startPage'],
  data () {
    return {
      currentPage: ''
    }
  },
  computed: {
    totalPages: function () {
      // get the total number of pages
      return Math.ceil(this.resultCount / this.itemsPerPage)
    }
  },
  watch: {
    '$props': {
      handler: function () {
        this.getCurrentPage()
      },
      deep: true
    }
  },
  mounted () {
    this.getCurrentPage()
  },
  methods: {
    getCurrentPage: function () {
      this.currentPage = this.startPage
    },
    setPage: function (pageNumber) {
      let searchedTerm = ''
      if (this.$route.params.term !== '' && typeof this.$route.params.term !== 'undefined') {
        searchedTerm = this.$route.params.term
      }
      this.currentPage = pageNumber
      this.$router.replace(`/listing/${this.currentPage}/${searchedTerm}`)
    }
  }
}
</script>

<style>
</style>
