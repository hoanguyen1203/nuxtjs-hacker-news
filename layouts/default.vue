<template>
  <div id="app">
    <Header />
    <main>
      <nuxt />
    </main>
  </div>
</template>

<script>
    import Header from '~/components/common/Header'

    export default {
        components: {
            Header
        }
    }
</script>

<style>
  html {
    font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
    font-size: 16px;
    box-sizing: border-box;
    line-height: 20px;
  }

  *,
  *:before,
  *:after {
    box-sizing: border-box;
    margin: 0;
  }

  a {
    text-decoration: none;
  }

  #app {
    min-height: 100vh;
    background-color: #f2f3f5;
  }

  main {
    max-width: 800px;
    margin: 0 auto;
  }
</style>
