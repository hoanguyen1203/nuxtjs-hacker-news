<template>
  <div class="body">
    <article class="content">
      <h1 class="content__title">Welcome to Hacker New API</h1>
      <img src="https://dulichbui.org/wp-content/uploads/2015/11/saigon.jpg" class="content__image">
    </article>
  </div>
</template>

<script>
  export default {
  }
</script>

<style lang="scss" scoped>
  .content {
    margin: 10px 0;
    background-color: #ffffff;
    padding: 30px 50px;
    border-bottom: 1px solid #eee;
    &__title {
      color: #2e495e;
      margin-bottom: 20px;
    }
    &__image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
</style>
