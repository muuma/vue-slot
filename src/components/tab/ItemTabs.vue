<template>
  <div :class="[prefixCls]">
    <div :class="[`${prefixCls}-bar`]">
      <div
        v-for="item in config.tabs"
        :key="item.id"
        :id="item.id"
        :class="[`${prefixCls}-tab`, curIndex === item.id ? `${prefixCls}-tab-active` : '']"
        @click="handleChange(item)"
      >{{ item.name }}</div>
    </div>
    <div
      v-for="item in config.tabs"
      :key="item.id"
      :class="[`${prefixCls}-content`]"
      v-show="curIndex === item.id"
    >
      <slot v-bind:tab="item"></slot>
    </div>
  </div>
</template>

<script>
const prefixCls = "slot-tabs";

export default {
  name: "ItemTabs",
  props: {
    config: {
      type: Object,
      default() {
        return {
          tabs: []
        };
      }
    }
  },
  data() {
    return {
      prefixCls: prefixCls,
      curIndex: "tab_1"
    };
  },
  methods: {
    handleChange(item) {
      this.curIndex = item.id;
    }
  }
};
</script>

<style scoped>
.slot-tabs-bar {
  display: flex;
}
.slot-tabs-tab {
  cursor: pointer;
  transition: color 0.3s ease-in-out;
  padding: 0 15px;
  margin: 15px 0;
}
.slot-tabs-tab.slot-tabs-tab-active {
  color: #19be6b;
  border-color: #19be6b;
}
</style>
