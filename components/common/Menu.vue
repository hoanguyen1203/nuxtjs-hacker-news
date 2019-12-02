<template>
  <nav>
    <ul class="menu">
      <li class="menu__item">
        <nuxt-link to="/news" class="menu__link">News</nuxt-link>
      </li>
      <li class="menu__item">
        <nuxt-link to="/newest" class="menu__link">Newest</nuxt-link>
      </li>
      <li class="menu__item">
        <nuxt-link to="/ask" class="menu__link">Ask</nuxt-link>
      </li>
      <li class="menu__item">
        <nuxt-link to="/show" class="menu__link">Show</nuxt-link>
      </li>
      <li class="menu__item">
        <nuxt-link to="/jobs" class="menu__link">Jobs</nuxt-link>
      </li>
    </ul>
  </nav>
</template>

<script>
    export default {
        name: 'Menu'
    }
</script>

<style lang="scss" scoped>
  .menu {
    padding-left: 10px;
    &__item {
      display: inline-block;
      padding-left: 20px;
    }
    &__link {
      color: #ffffff;
      font-size: 15px;
      font-weight: 300;
      letter-spacing: 0.075em;
    }
  }
  .nuxt-link-active {
    font-weight: 600;
  }
</style>
