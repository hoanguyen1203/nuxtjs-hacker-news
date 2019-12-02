<template>
  <section class="content__story story">
    <span class="story__scope">{{ story.data.score }}</span>
    <div class="story__title">
      <a class="story__link" :href="story.data.url" target="_blank">{{ story.data.title }}</a>
      <span class="story__host">{{ story.data.url | host }}</span>
    </div>
    <div class="story__info">
      <span class="story__by">by
        <nuxt-link :to="{ path: '/user/' + story.data.by }">{{ story.data.by }}</nuxt-link>
      </span>
      <span class="story__time">{{ story.data.time | timeAgo }}</span>
      <span class="story__dash">|</span>
      <span class="story__comments">
        <nuxt-link :to="{ path: '/comment/' + story.data.id }">{{ `${story.data.descendants} comments` }}</nuxt-link>
      </span>
    </div>
  </section>
</template>

<script>
  import _ from 'lodash'

  export default {
    name: 'Story',
    props: [
      'story'
    ],
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
    }
  }
</script>

<style lang="scss" scoped>
  .story {
    background-color: #ffffff;
    padding: 20px 30px 20px 80px;
    border-bottom: 1px solid #eee;
    position: relative;
    &__scope {
      color: #2e495e;
      font-size: 1.1em;
      font-weight: 700;
      position: absolute;
      top: 50%;
      left: 0;
      width: 80px;
      text-align: center;
      margin-top: -10px;
    }
    &__link {
      color: #2e495e;
    }
    &__host,
    &__by,
    &__time,
    &__dash,
    &__comments {
      font-size: 0.85em;
      color: #595959;
    }
    &__by,
    &__comments {
      a {
        color: #595959;
        text-decoration: underline;
        &:hover {
          color: #00c48d;
        }
      }
    }
  }
</style>
