<template>
  <span>
    <slot v-bind="user">{{ user.lastName }}</slot>
    <!-- <slot name="other" v-bind="otherUser">{{ otherUser.name }}</slot> -->
  </span>
</template>

<script>
export default {
  name: 'CurrentUser',
  data () {
    return {
      user: {
        firstName: 'current-user-first-name',
        lastName: 'current-user-last-name'
      },
      otherUser: {
        name: 'current-user',
        age: 18
      }
    }
  }
}
</script>
