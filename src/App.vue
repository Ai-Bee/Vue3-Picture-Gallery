<template>
  <HomeView></HomeView>
</template>

<script>
import HomeView from "./views/HomeView.vue";
export default {
  components: {
    HomeView,
  },
  setup() {},
};
</script>

<style lang="scss">
#app {
  height: 100%;
  padding-left: 20px;
  padding-right: 20px;
}
</style>
