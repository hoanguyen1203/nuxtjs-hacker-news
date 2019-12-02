<template>
  <nav>
    <ul class="pagination">
      <li class="pagination__item">
        <nuxt-link
          :to="`/${path}/${prev}`"
          class="pagination__link"
          :disabled="disabledPrev"
          @click="previousPage"
        >
          < prev
        </nuxt-link>
      <li class="pagination__item">
        <span>{{ `${currentPage}/${totalPages}` }}</span>
      </li>
      <li class="pagination__item">
        <nuxt-link
          :to="`/${path}/${next}`"
          class="pagination__link"
          :disabled="disabledNext"
          @click="nextPage"
        >
          next >
        </nuxt-link>
      </li>
    </ul>
  </nav>
</template>

<script>
  import _ from 'lodash'

  export default {
    name: 'Pagination',
    props: [
      'currentPage',
      'totalPages',
      'fullPath'
    ],
    computed: {
      disabledPrev() {
        return this.currentPage === 1
      },
      disabledNext() {
        return this.currentPage === this.totalPages
      },
      prev() {
        return this.currentPage - 1
      },
      next() {
        return this.currentPage + 1
      },
      path() {
        const path = _.split(this.fullPath, '/')[1]
        return path
      }
    },
    methods: {
      previousPage() {
        this.$emit('previousPage')
      },
      nextPage() {
        this.$emit('nextPage')
      }
    }
  }
</script>

<style lang="scss" scoped>
  .pagination {
    padding: 15px 30px;
    text-align: center;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    background-color: #ffffff;
    border-radius: 2px;
    font-size: 15px;
    color: #2e495e;
    &__item {
      display: inline-block;
    }
    &__link {
      color: #2e495e;
      margin: 0 1em;
      &[disabled] {
        opacity: 0.8;
        pointer-events: none;
      }
    }
  }
</style>
