<!-- <template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Welcome to Your Vue.js App" />
</template> -->
<template>
  <div s="aaasd">
    <div class="navbar navbar-expand-lg navbar-light bg-light">
      <search-bar class="my-2 my-lg-0"></search-bar>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
      <search-bar></search-bar>
      </div>
      <div class="panel-body">
        <p></p>
      </div>
    </div>
  </div>
</template>

<script>
//import HelloWorld from "./components/HelloWorld.vue";
import SearchBar from "./components/SearchBar.vue";

export default {
  name: "App",
  components: {
    //HelloWorld,
    SearchBar,
  },
};
</script>

<style scoped>
.panel {
   width: 480px;
   height: 480px;
   margin: 40px auto;
 }

 .panel > .panel-heading {
   position:relative;
 }

.navbar .search-bar {
  width:300px;
  position: relative;
  left: calc(100% - 300px);
}
</style>
