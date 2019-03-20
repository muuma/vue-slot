<template>
  <div id="app">
    <div class="slot-item">
      插槽内容：
      <NavigationLink url="/profile">Your profile</NavigationLink>
    </div>

    <div class="slot-item">
      默认内容：
      <SubmitButton></SubmitButton>
      <SubmitButton>Save</SubmitButton>
    </div>

    <div class="slot-item">
      具名插槽：
      <BaseLayout>
        <template v-slot:header>
          <h1>Here might be a page title</h1>
        </template>

        <p>A paragraph for the main content.</p>
        <p>And another one.</p>

        <template v-slot:footer>
          <p>Here's some contact info</p>
        </template>
      </BaseLayout>
    </div>

    <div class="slot-item">
      编译作用域：
      <NavigationLink url="/profile">Logged in as {{ user.name }}</NavigationLink>,
      <NavigationLink url="/profile">
        Clicking here will send you to: {{ url }}
        <!-- 这里的 `url` 会是 undefined，
            因为"/profile"是传递给<NavigationLink>的，只有在<NavigationLink>内部可以访问 -->
      </NavigationLink>
    </div>

    <div class="slot-item">
      作用域插槽：
      <CurrentUser>{{user.firstName }}</CurrentUser>
      <!-- 注意 vue 版本 -->
      <CurrentUser>
        <template v-slot:default="slotProps">
          {{ slotProps.firstName }}, 
        </template>
      </CurrentUser>
      <CurrentUser v-slot:default="slotProps">{{ slotProps.firstName }}, </CurrentUser>
      <CurrentUser v-slot="slotProps">{{ slotProps.firstName }}, </CurrentUser>

      <!-- 多个插槽 -->
      <!-- <CurrentUser>
        <template v-slot:default="slotProps">
          {{ slotProps.firstName }}
        </template>
        <template v-slot:other="otherSlotProps">
          {{ otherSlotProps.age }}
        </template>
      </CurrentUser> -->
    </div>

    <div class="slot-item">
      示例（todo-list）：
      <TodoList :todos="todos">
        <template v-slot="{ todo }">
          <span v-if="todo.isComplete">✓</span>
          {{ todo.id }} {{ todo.text }}
        </template>
      </TodoList>
    </div>

    <div class="slot-item">
      这里再举一个特别的例子，动态渲染组件。其中 Tabs 组件用到了 vue-slot：
      <Item :list="configList"></Item>
    </div>
  </div>
</template>

<script>
import NavigationLink from './components/NavigationLink.vue'
import SubmitButton from './components/SubmitButton.vue'
import BaseLayout from './components/BaseLayout.vue'
import CurrentUser from './components/CurrentUser.vue'
import TodoList from './components/TodoList.vue'
import Item from './components/tab/Item.vue'
import Logo from './assets/logo.png'

export default {
  name: 'app',
  components: {
    NavigationLink,
    SubmitButton,
    BaseLayout,
    CurrentUser,
    TodoList,
    Item
  },
  data() {
    return {
      user: {
        name: 'muuma'
      },
      todos: [
        {
          id: 1,
          text: 'Hi',
          isComplete: false
        },
        {
          id: 2,
          text: 'Slot',
          isComplete: true
        },
        {
          id: 3,
          text: 'Vue',
          isComplete: false
        }
      ],
      configList: [
        {
          name: 'ItemImage',
          config: {
            url: Logo,
            title: 'Item image'
          }
        },
        {
          name: 'ItemText',
          config: {
            text: 'Hi, I\'m vue-slot.'
          }
        },
        {
          name: 'ItemTabs',
          config: {
            tabs: [
              {
                id: 'tab_1',
                name: 'tab1',
                content: [
                  {
                    name: 'ItemImage',
                    config: {
                      url: Logo,
                      title: 'Tabs image'
                    }
                  }
                ]
              },
              {
                id: 'tab_2',
                name: 'tab2',
                content: [
                  {
                    name: 'ItemText',
                    config: {
                      text: 'Hi, I\'m Tabs vue-slot.'
                    }
                  }
                ]
              }
            ]
          }
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 360px;
}
.slot-item {
  margin-bottom: 10px;
}
</style>
