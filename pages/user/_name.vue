<template>
  <article class="content">
    <section class="content__user user">
      <h1 class="user__name">User: {{ user.id }}</h1>
      <p class="user__created">Created: {{ user.created | timeAgo }}</p>
      <p class="user__karma">Karma: {{ user.karma }}</p>
      <div class="user__about" v-html="user.about"></div>
      <div class="user__links">
        <a :href="`https://news.ycombinator.com/submitted?id=${user.id}`" target="_blank" class="user__link">submissions</a>
        |
        <a :href="`https://news.ycombinator.com/threads?id=${user.id}`" target="_blank" class="user__link">comments</a>
      </div>
    </section>
  </article>
</template>

<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        name: this.$route.params.name,
        user: {}
      }
    },
    filters: {
      timeAgo(time) {
        const arrayTime = [
          ['year', 365 * 24 * 60 * 60],
          ['month', 30 * 24 * 60 * 60],
          ['day', 24 * 60 * 60],
          ['hour', 60 * 60],
          ['minute', 60],
        ]
        let timeAgo = {}
        for (const [name, number] of arrayTime) {
          const seconds = Math.floor((new Date().getTime() / 1000) - time)
          const value = Math.floor(seconds/ number)
          if (value >= 1) {
            timeAgo = {
              name: name,
              number: value
            }
            break
          }
        }
        const suffix = timeAgo.number !== 1 ? 's' : ''
        return `${timeAgo.number} ${timeAgo.name}${suffix} ago`
      }
      // timeAgo(time) {
      //   const seconds = Math.floor((new Date().getTime() / 1000) - time)
      //   const year =  Math.floor(seconds / (365 * 24 * 60 * 60))
      //   const month = Math.floor(seconds / (30 * 24 * 60 * 60))
      //   const day = Math.floor(seconds / (24 * 60 * 60))
      //   const hour = Math.floor(seconds / (60 * 60))
      //   const minute = Math.floor(seconds / (60))
      //   let timeAgo
      //   if (year >= 1) {
      //     timeAgo = year !== 1 ? `${year} years ago` : `${year} year ago`
      //   } else if (month >= 1) {
      //     timeAgo = month !== 1 ? `${month} months ago` : `${month} month ago`
      //   } else if (day >= 1) {
      //     timeAgo = day !== 1 ? `${day} days ago` : `${day} day ago`
      //   } else if (hour >= 1) {
      //     timeAgo = hour !== 1 ? `${hour} hours ago` : `${hour} hour ago`
      //   } else if (minute >= 1) {
      //     timeAgo = minute !== 1 ? `${minute} minutes ago` : `${minute} minute ago`
      //   } else {
      //     timeAgo = 'just now'
      //   }
      //   return timeAgo
      // }
    },
    created() {
      this.fetchUser()
    },
    methods: {
      async fetchUser() {
        const user = await axios
          .get("https://hacker-news.firebaseio.com/v0/user/" + this.name + ".json")
        this.user = user.data
      }
    }
  }
</script>

<style lang="scss" scoped>
  .user {
    background-color: #ffffff;
    box-sizing: border-box;
    padding: 2em 3em;
    color: #2e495e;
    &__name {
      font-size: 1.5em;
      margin-bottom: 1em;
    }
    &__about {
      margin: 1em 0;
    }
    &__link {
      color: #2e495e;
      text-decoration: underline;
    }
  }
</style>
