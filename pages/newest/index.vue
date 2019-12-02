<template>
  <div class="body">
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"/>
    <article class="content">
      <story v-for="newest in newestList" :key="newest.id" :story="newest" />
    </article>
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"/>
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
        newsStories: [],
        newestList: [],
        currentPage: 1,
        totalPages: 0,
        perPage: 20,
        fullPath: this.$nuxt.$route.path
      }
    },
    async created() {
      await this.fetchNewsStories()
      this.fetchNewsStory()
    },
    methods: {
      async fetchNewsStories() {
        this.newsStories = await axios
          .get("https://hacker-news.firebaseio.com/v0/newstories.json")
      },
      fetchNewsStory() {
        const list = this.newsStories.data.slice(0, this.perPage)
        this.totalPages = Math.ceil(this.newsStories.data.length / this.perPage)
        list.map(async element => {
          const newsStory = await axios
            .get("https://hacker-news.firebaseio.com/v0/item/" + element + ".json")
          this.newestList.push(newsStory)
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    margin: 10px 0;
  }
</style>
