<template>
  <div class="body">
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"
    />
    <article class="content">
      <ask v-for="ask in askList" :key="ask.id" :ask="ask" />
    </article>
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"
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
        currentPage: 1,
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
        const list = this.askStories.data.slice(0, this.perPage)
        this.totalPages = Math.ceil(this.askStories.data.length / this.perPage)
        list.map( async element => {
          const askStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.askList.push(askStory)
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
