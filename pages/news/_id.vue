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
      <story v-for="news in newsList" :key="news.id" :story="news" />
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
        topStories: [],
        newsList: [],
        currentPage: parseInt(this.$route.params.id),
        totalPages: 0,
        perPage: 20,
        fullPath: this.$nuxt.$route.path
      }
    },
    async created() {
      await this.fetchTopStories()
      this.fetchTopStory()
    },
    methods: {
      async fetchTopStories() {
        this.topStories = await axios
          .get("https://hacker-news.firebaseio.com/v0/topstories.json")
      },
      fetchTopStory() {
        this.newsList = []
        let list = this.topStories.data.slice((this.currentPage - 1) * this.perPage, this.perPage * this.currentPage)
        this.totalPages = Math.ceil(this.topStories.data.length / this.perPage)
        list.map( async element => {
          const topStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.newsList.push(topStory)
        })
      },
      async changeCurrentPage(value) {
        if (value === 'previous') {
          this.currentPage -= 1
        } else {
          this.currentPage += 1
        }
        await this.fetchTopStory()
      }
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    margin: 10px 0;
  }
</style>
