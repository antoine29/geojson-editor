<template>
  <i-col span="10" class="sidebar">
    <!-- <top-menu /> -->
    <TabMenu />
    <!-- <code-area /> -->
    <bottom-menu />
  </i-col>
</template>

<script>
// import CodeArea from './Code.vue'
// import TopMenu from './TopMenu.vue'
import BottomMenu from './BottomMenu.vue'
import TabMenu from './TabMenu';

export default {
  name: 'Sidebar',
  components: {
    TabMenu,
    // CodeArea,
    BottomMenu,
    // TopMenu
  }
}
</script>

<style>
.sidebar {
  height: 100%;
}
</style>
