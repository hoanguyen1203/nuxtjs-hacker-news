<template>
  <div class="comments">
    <div class="comments__header">
      <div class="comments__title">
        <a class="comments__link" :href="comment.url" target="_blank">{{ comment.title }}</a>
        <span class="comments__host">{{ comment.url | host }}</span>
      </div>
      <div class="comments__info">
        <span class="comments__scope">{{ comment.score }} points</span>
        <span class="comments__dash">|</span>
        <span class="comments__by">by
          <nuxt-link :to="{ path: '/user/' + comment.by }">{{ comment.by }}</nuxt-link>
        </span>
        <span class="comments__time">{{ comment.time | timeAgo }}</span>
      </div>
    </div>
    <div class="comments__body">
      <p class="comments__number">{{ numberComments }} comments</p>
      <comment-children :kidsData="kidsData" />
    </div>
  </div>
</template>

<script>
  import CommentChildren from '~/components/comment/CommentChildren'
  import axios from 'axios'
  import _ from 'lodash'

  export default {
    components: {
      CommentChildren
    },
    data() {
      return {
        id: this.$route.params.id,
        comment: {},
        kids: [],
        kidsData: []
      }
    },
    filters: {
      host(url) {
        let host = _.split(_.split(url, '//')[1], '/')[0]
        if (_.includes(host, 'www.')) {
          host = _.split(host, 'www.')[1]
        }
        if (host !== '') {
          host = `(${host})`
        }
        return host
      },
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
    },
    async created() {
      await this.fetchComment()
      await this.getKids()
      this.fetchCommentChildren()
    },
    computed: {
      numberComments() {
        return this.kids.length
      }
    },
    methods: {
      getKids() {
        this.kids = _.split(this.comment.kids, ',')
        return this.kids
      },
      async fetchComment() {
        const comment = await axios
          .get("https://hacker-news.firebaseio.com/v0/item/" + this.id + ".json")
        this.comment = comment.data
      },
      fetchCommentChildren() {
        this.kids.map( async element => {
          const kid =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.kidsData.push(kid)
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .comments {
    &__header {
      background-color: #ffffff;
      padding: 1.8em 2em 1em;
      box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    }
    &__link {
      font-size: 1.5em;
      font-weight: 700;
      color: #2e495e;
    }
    &__host {
      font-size: 15px;
      color: #595959;
    }
    &__info {
      font-size: 15px;
      color: #595959;
      margin: 1em 0;
    }
    &__by {
      a {
        color: #595959;
        text-decoration: underline;
        &:hover {
          color: #00c48d;
        }
      }
    }
    &__body {
      background-color: #ffffff;
      margin-top: 10px;
      padding: 0 2em 0.5em;
    }
    &__number {
      font-size: 1.1em;
      color: #2e495e;
      padding: 1em 0;
    }
  }
</style>
