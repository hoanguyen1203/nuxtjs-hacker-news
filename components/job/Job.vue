<template>
  <section class="content__job job">
    <span class="job__scope">{{ job.data.score }}</span>
    <div class="job__title">
      <a class="job__link" :href="job.data.url" target="_blank">{{ job.data.title }}</a>
      <span class="job__host">{{ job.data.url | host }}</span>
    </div>
    <p class="job__time">{{ job.data.time | timeAgo }}</p>
  </section>
</template>

<script>
  import _ from 'lodash'

  export default {
    name: 'Job',
    props: [
      'job'
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
    }
  }
</script>

<style lang="scss" scoped>
  .job {
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
    &__time {
      font-size: 0.85em;
      color: #595959;
    }
  }
</style>
