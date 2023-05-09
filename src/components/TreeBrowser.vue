<template>
    <div>
        <ul class="tree">
            <tree-node v-for="(node, index) in treeData" :key="index" :node="node" :depth="0"></tree-node>
        </ul>
    </div>
</template>

<script>
import axios from "axios";
import TreeNode from "./TreeNode.vue";

export default {
  components: {
    TreeNode,
  },
  props: {
    treeData: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
.tree {
  list-style-type: none;
  padding-left: 0;
  width: 300px;
  margin: 0 auto;
}

.label-wrapper {
    padding-bottom: 10px;
    margin-bottom: 10px;
    border-bottom: 1px solid #ccc;
}

.tree-node {
  margin-bottom: 10px;
}
</style>
