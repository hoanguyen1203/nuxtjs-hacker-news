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
      <job  v-for="job in jobs" :key="job.id" :job="job" />
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
        currentPage: parseInt(this.$route.params.id),
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
        const list = this.jobsStories.data.slice((this.currentPage - 1) * this.perPage, this.perPage * this.currentPage)
        this.totalPages = Math.ceil(this.jobsStories.data.length / this.perPage)
        list.map( async element => {
          const jobsStory =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.jobs.push(jobsStory)
        })
      },
      async changeCurrentPage(value) {
        if (value === 'previous') {
          this.currentPage -= 1
        } else {
          this.currentPage += 1
        }
        await this.fetchJobsStory()
      }
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    margin: 10px 0;
  }
</style>
