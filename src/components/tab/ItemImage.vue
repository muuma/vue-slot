<template>
  <div class="image">
    <img :src="config.url" :title="config.title" />
  </div>
</template>

<script>
export default {
  name: "ItemImage",
  props: {
		config: {
			type: Object,
			default() {
				return {
					url: '../../assets/logo.png',
					title: 'default image'
				}
			}
		}
	}
};
</script>
