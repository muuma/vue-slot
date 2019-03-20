<template>
  <button type="submit">
    <!-- Submit 就是后备内容 -->
    <slot>Submit</slot>
  </button>
</template>

<script>
export default {
  name: 'SubmitButton'
}
</script>
