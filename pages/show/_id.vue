<template>
  <div class="body">
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"
      @previousPage="changeCurrentPage('previous')"
      @nextPage="changeCurrentPage('next')"
    />
    <article class="content">
      <story v-for="show in showList" :key="show.id" :story="show" />
    </article>
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"
      @previousPage="changeCurrentPage('previous')"
      @nextPage="changeCurrentPage('next')"
    />
  </div>
</template>

<script>
  import Pagination from '~/components/pagination/Pagination.vue'
  import Story from '~/components/story/Story.vue'
  import axios from 'axios'

  export default {
    components: {
      Pagination,
      Story
    },
    data() {
      return {
        showStories: [],
        showList: [],
        currentPage: parseInt(this.$route.params.id),
        totalPages: 0,
        perPage: 20,
        fullPath: this.$nuxt.$route.path
      }
    },
    async created() {
      await this.fetchShowsStories()
      this.fetchShowsStory()
    },
    methods: {
      async fetchShowsStories() {
        this.showStories = await axios.get(
          "https://hacker-news.firebaseio.com/v0/showstories.json"
        )
      },
      fetchShowsStory() {
        this.showList = []
        const list = this.showStories.data.slice((this.currentPage - 1) * this.perPage, this.perPage * this.currentPage)
        this.totalPages = Math.ceil(this.showStories.data.length / this.perPage)
        list.map( async element => {
          const showsStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.showList.push(showsStory)
        })
      },
      async changeCurrentPage(value) {
        if (value === 'previous') {
          this.currentPage -= 1
        } else {
          this.currentPage += 1
        }
        await this.fetchShowsStory()
      }
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    margin: 10px 0;
  }
</style>
