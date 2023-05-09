<template>
  <div id="app" style="display: flex;">
    <div style="width: 50%; align-items: center; justify-content: center;">
      <tree-browser :tree-data="treeData1"></tree-browser>
    </div>
    <div style="width: 50%; align-items: center; justify-content: center;">
      <tree-browser :tree-data="treeData2"></tree-browser>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import TreeBrowser from "./components/TreeBrowser.vue";

export default {
  name: "App",
  components: {
    TreeBrowser,
  },
  data() {
    return {
      treeData1: [],
      treeData2: [],
    };
  },
  async created() {
    try {
      const response1 = await axios.get("data.json");
      this.treeData1 = response1.data;
      const response2 = await axios.get("data2.json");
      this.treeData2 = response2.data;
    } catch (error) {
      console.error("Failed to fetch tree data:", error);
    }
  },
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
