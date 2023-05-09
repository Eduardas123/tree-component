<template>
    <li>
      <div class="node" @click="toggleExpanded">
        <i v-if="node.children && node.children.length" :class="expanded ? 'fa fa-minus-square' : 'fa fa-plus-square'"></i>
        <i :class="getIconClass(depth)"></i>
        {{ node.name }}
      </div>
      <ul v-if="node.children && node.children.length && expanded" class="children">
        <tree-node v-for="(child, index) in node.children" :key="index" :node="child" :depth="depth + 1"></tree-node>
      </ul>
    </li>
</template>

<script>
export default {
  name: "TreeNode",
  props: {
    node: {
      type: Object,
      required: true,
    },
    depth: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      expanded: false,
    };
  },
  methods: {
    toggleExpanded() {
      if (this.node.children && this.node.children.length) {
        this.expanded = !this.expanded;
      }
    },
    getIconClass(depth) {
      const iconMap = {
        0: "fa fa-folder",
        1: "fa fa-file",
        2: "fa fa-picture-o",
        3: "fa fa-file-text-o",
        4: "fa fa-file-pdf-o",
        5: "fa fa-file-word-o",
        6: "fa fa-file-excel-o",
        7: "fa fa-file-powerpoint-o",
        8: "fa fa-file-archive-o",
      };

      return iconMap[depth] || "fa fa-icon-default";
    },
  },
};
</script>

<style scoped>
.node {
  cursor: pointer;
  font-family: "Open Sans", sans-serif;
  font-size: 18px;
  font-weight: 300;
  line-height: 1em;
  color: #2c3e50;
  text-align: left;
  padding: 6px 0;
}

.node:hover {
  background-color: rgba(164, 172, 178, 0.1);
  transition: background-color 0.3s ease;
}

.children {
  list-style-type: none;
  padding-left: 20px;
  position: relative;
}

.children:before {
  content: '';
  position: absolute;
  top: 0;
  left: 8px;
  bottom: 0;
  width: 1.5px;
  background-color: #ccc;
}

.tree-menu .label-wrapper {
  padding-bottom: 10px;
  margin-bottom: 10px;
  border-bottom: 1px solid #ccc;
}

.fa-plus-square,
.fa-minus-square {
  font-size: 0.8em;
  color: #2c3e50;
  margin-right: 5px;
}
</style>
