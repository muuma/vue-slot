<template>
  <a v-bind:href="url" class="nav-link" >
    <slot></slot>
  </a>
</template>

<script>
export default {
  name: 'NavigationLink',
  props: {
    url: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nav-link {
  color: #42b983;
}
</style>
