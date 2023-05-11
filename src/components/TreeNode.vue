<template>
  <li>
    <div class="node" @click="toggleExpanded">
      <i v-if="node.children && node.children.length" :class="expanded ? 'fa fa-minus-square' : 'fa fa-plus-square'"></i>
      <i :class="getIconClass(depth)"></i>
      <div v-if="!isEditing" @click.stop>
        <span class="node-name" v-if="!isEditing" @click.stop>
          {{ node.name }}
        </span>
      </div>
      <input v-else type="text" v-model="node.name" @blur="endEdit" @keydown.enter="endEdit">
      <div class="button-group">
        <i class="fa fa-file-o" @click.stop="addNode" title="Add"></i>
        <i class="fa fa-trash-o" @click.stop="removeNode" title="Remove"></i>
        <i class="fa fa-edit" @click.stop="beginEdit" title="Edit"></i>
      </div>
    </div>
    <ul v-if="node.children && node.children.length && expanded" class="children">
      <tree-node v-for="(child, index) in node.children" :key="index" :node="child" :depth="depth + 1"
        @remove-node="handleRemoveNode"></tree-node>
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
      isEditing: false,
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
    addNode() {
      const newNode = { name: "New Node", children: [] };
      this.node.children.push(newNode);

      // If the node is not already expanded, expand it
      if (!this.expanded) {
        this.toggleExpanded();
      }
    },
    removeNode() {
      this.$emit("remove-node", this.node);
    },
    handleRemoveNode(nodeToRemove) {
      this.node.children = this.node.children.filter(child => child !== nodeToRemove);
    },
    beginEdit() {
      this.isEditing = true;
    },
    endEdit() {
      this.isEditing = false;
    }
  },
};
</script>
<style scoped>
.node {
  display: flex;
  position: relative;
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

.button-group {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.node:hover .button-group {
  opacity: 1;
}

.button-group button {
  background-color: #2c3e50;
  color: #fff;
  border: none;
  border-radius: 3px;
  padding: 5px 10px;
  margin-left: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.button-group button:hover {
  background-color: #34495e;
}

.button-group i {
  margin: 0 2px;
}

.node-name {
  margin-left: 10px;
}
</style>
