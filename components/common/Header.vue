<template>
  <header class="header">
    <div class="header__inner">
      <div class="header__left">
        <div class="header__logo">
          <logo />
        </div>
        <div class="header__menu">
          <Menu />
        </div>
      </div>
      <div class="header__right">
        Built with Nuxt.js
      </div>
    </div>
  </header>
</template>

<script>
  import Logo from '~/components/common/Logo.vue'
  import Menu from '~/components/common/Menu.vue'

  export default {
    name: 'Header',
    components: {
      Menu,
      Logo
    }
  }
</script>

<style lang="scss" scoped>
  .header {
    background-color: #2e495e;
    &__inner {
      max-width: 800px;
      margin: 0 auto;
      padding: 15px 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      color: #ffffff;
    }
    &__left {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    &__logo {
      width: 24px;
    }
  }
</style>
