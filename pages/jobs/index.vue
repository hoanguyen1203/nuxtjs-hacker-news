<template>
  <div class="body">
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"/>
    <article class="content">
      <job  v-for="job in jobs" :key="job.id" :job="job" />
    </article>
    <pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :fullPath="fullPath"/>
  </div>
</template>

<script>
  import Pagination from '~/components/pagination/Pagination.vue'
  import Job from '~/components/job/Job.vue'
  import axios from 'axios'

  export default {
    name: 'jobs',
    components: {
      Pagination,
      Job
    },
    data() {
      return {
        jobsStories: [],
        jobs: [],
        currentPage: 1,
        totalPages: 0,
        perPage: 20,
        fullPath: this.$nuxt.$route.path
      }
    },
    async created() {
      await this.fetchJobsStories()
      this.fetchJobsStory()
    },
    methods: {
      async fetchJobsStories() {
        this.jobsStories = await axios
          .get("https://hacker-news.firebaseio.com/v0/jobstories.json")
      },
      fetchJobsStory() {
        const list = this.jobsStories.data.slice(0, this.perPage)
        this.totalPages = Math.ceil(this.jobsStories.data.length / this.perPage)
        list.map( async element => {
          const jobsStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.jobs.push(jobsStory)
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
