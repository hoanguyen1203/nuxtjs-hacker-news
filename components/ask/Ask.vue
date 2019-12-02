<template>
  <section class="content__ask ask">
    <span class="ask__scope">{{ ask.data.score }}</span>
    <div class="ask__title">
      <a class="ask__link" :href="ask.data.url" target="_blank">{{ ask.data.title }}</a>
    </div>
    <div class="ask__info">
        <span class="ask__by">by
          <nuxt-link :to="{ path: '/user/' + ask.data.by }">{{ ask.data.by }}</nuxt-link>
        </span>
      <span class="ask__time">{{ ask.data.time | timeAgo }}</span>
      <span class="ask__dash">|</span>
      <span class="ask__comments">
        <nuxt-link :to="{ path: '/comment/' + ask.data.id }">{{ `${ask.data.descendants} comments` }}</nuxt-link>
      </span>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'Ask',
    props: [
      'ask'
    ],
    filters: {
      timeAgo(time) {
        const arrayTime = [
          ['year', 365 * 24 * 60 * 60],
          ['month', 30 * 24 * 60 * 60],
          ['day', 24 * 60 * 60],
          ['hour', 60 * 60],
          ['minute', 60],
          ['second', 1]
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
  }
</script>

<style lang="scss" scoped>
  .ask {
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
