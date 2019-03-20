<template>
  <div class="item-wrap" v-if="list.length > 0">
    <div
      v-for="(item, index) in list"
      :key="index"
    >
      <component
        :is="item.name"
        :config="item.config"
      >
        <template v-if="item.name === 'ItemTabs'" v-slot="{ tab }">
          <Item :list="tab.content"></Item>
        </template>
      </component>
    </div>
  </div>
</template>

<script>
import ItemImage from './ItemImage'
import ItemText from './ItemText'
import ItemTabs from './ItemTabs'

export default {
	name: "Item",
	components: {
		ItemImage,
		ItemText,
		ItemTabs
	},
  props: {
		list: {
			type: Array,
			default() {
				return []
			}
		}
	},
};
</script>
