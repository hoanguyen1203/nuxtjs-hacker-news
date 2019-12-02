<template>
  <div class="body">
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"/>
    <article class="content">
      <story v-for="show in showList" :key="show.id" :story="show" />
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
    name: 'show',
    components: {
      Pagination,
      Story
    },
    data() {
      return {
        showStories: [],
        showList: [],
        currentPage: 1,
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
        this.showStories = await axios
          .get("https://hacker-news.firebaseio.com/v0/showstories.json")
      },
      fetchShowsStory() {
        const list = this.showStories.data.slice(0, this.perPage)
        this.totalPages = Math.ceil(this.showStories.data.length / this.perPage)
        list.map( async element => {
          const showsStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.showList.push(showsStory)
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
