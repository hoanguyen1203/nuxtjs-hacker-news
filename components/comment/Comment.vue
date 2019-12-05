<template>
  <div class="comment">
    <div class="comment__by">
      <nuxt-link :to="{ path: '/user/' + kid.data.by }">{{ kid.data.by }}</nuxt-link>
      <span>{{ kid.data.time | timeAgo }}</span>
    </div>
    <div class="comment__text" v-html="kid.data.text"></div>
    <div class="comment__kid" v-if="hasKids === true">
      <button class="comment__toggle" :class="{ 'isOpen': isOpen }" @click="toggleComments">[-]</button>
      <button class="comment__collapse" :class="{ 'isCollapse': isCollapse }" @click="toggleCollapse">{{ kids.length }} replies collapsed</button>
      <comment v-for="kid in kidsData" :key="kid.id" :kid="kid" />
    </div>
  </div>
</template>

<script>
  import CommentChildren from '~/components/comment/CommentChildren'
  import axios from "axios"

  export default {
    name: 'Comment',
    components: {
      CommentChildren
    },
    data() {
      return {
        kids: [],
        kidsData: [],
        isOpen: true,
        isCollapse: false
      }
    },
    props: [
      'kid'
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
    async created() {
      if (this.hasKids  === true) {
        await this.getKids()
        this.fetchCommentChildren()
      }
    },
    computed: {
      hasKids() {
        const hasKids = 'kids' in this.kid.data
        return hasKids
      }
    },
    methods: {
      getKids() {
        this.kids = _.split(this.kid.data.kids, ',')
        return this.kids
      },
      fetchCommentChildren() {
        this.kids.map( async element => {
          const kid =  await axios.get(
            "https://hacker-news.firebaseio.com/v0/item/" + element + ".json"
          )
          this.kidsData.push(kid)
        })
      },
      toggleComments() {
        this.isOpen = !this.isOpen
        this.isCollapse = !this.isCollapse
      },
      toggleCollapse() {
        this.isCollapse = !this.isCollapse
        this.isOpen = !this.isOpen
      }
    }
  }
</script>

<style lang="scss" scoped>
  .comment {
    border-top: 1px solid #eee;
    padding-top: 1em;
    font-size: 15px;
    &__by {
      color: #595959;
      a {
        text-decoration: underline;
        color: #595959;
      }
    }
    &__text {
      color: #2e495e;
      margin: 1em 0;
    }
    &__kid {
      position: relative;
      padding-left: 1.5em;
    }
    &__toggle {
      position: absolute;
      top: -10px;
      left: 0;
      background-color: transparent;
      border: none;
      cursor: pointer;
      display: none;
      &:focus {
        outline: none;
      }
      & ~ div {
        display: none;
      }
      &.isOpen {
        display: block;
        & ~ div {
          display: block;
        }
      }
    }
    &__collapse {
      background-color: transparent;
      border: none;
      cursor: pointer;
      display: none;
      margin-bottom: 1em;
      color: #00c48d;
      padding: 0;
      &.isCollapse {
        display: block;
      }
      &:focus {
        outline: none;
      }
      &::before {
        content: '[+]';
        position: absolute;
        top: 0;
        left: 0;
      }
    }
  }
</style>
