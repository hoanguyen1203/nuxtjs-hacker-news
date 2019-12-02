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
      <ask v-for="ask in askList" :key="ask.id" :ask="ask" />
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
  import Ask from '~/components/ask/Ask.vue'
  import axios from 'axios'

  export default {
    components: {
      Pagination,
      Ask
    },
    data() {
      return {
        askStories: [],
        askList: [],
        currentPage: parseInt(this.$route.params.id),
        totalPages: 0,
        perPage: 20,
        fullPath: this.$nuxt.$route.path
      }
    },
    async created() {
      await this.fetchAskStories()
      this.fetchAskStory()
    },
    methods: {
      async fetchAskStories() {
        this.askStories = await axios
          .get("https://hacker-news.firebaseio.com/v0/askstories.json")
      },
      fetchAskStory() {
        this.askList = []
        const list = this.askStories.data.slice((this.currentPage - 1) * this.perPage, this.perPage * this.currentPage)
        this.totalPages = Math.ceil(this.askStories.data.length / this.perPage)
        list.map( async element => {
          const askStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.askList.push(askStory)
        })
      },
      async changeCurrentPage(value) {
        if (value === 'previous') {
          this.currentPage -= 1
        } else {
          this.currentPage += 1
        }
        await this.fetchAskStory()
      }
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    margin: 10px 0;
  }
</style>
