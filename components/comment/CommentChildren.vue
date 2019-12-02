<template>
  <div class="comments__children">
    <comment v-for="kid in kidsData" :key="kid.id" :kid="kid" />
  </div>
</template>

<script>
    import Comment from '~/components/comment/Comment'
    export default {
      name: 'CommentChildren',
      components: {
        Comment
      },
      props: [
        'kidsData'
      ]
    }
</script>

<style lang="scss" scoped>

</style>
