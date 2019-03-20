<template>
  <div class="text">
		<p>{{ config.text }}</p>
  </div>
</template>

<script>
export default {
  name: "ItemText",
  props: {
    config: {
			type: Object,
			default() {
				return {
					text: 'default text'
				}
			}
		}
  }
};
</script>
