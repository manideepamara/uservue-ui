
<template>
  <div class="app">
    <h1>Graph Visualization</h1>
    <TreeGraph @node-select="onNodeSelect" />
    <NodeDetails :node="selectedNode" @deselect="onDeselect" />
  </div>
</template>

<script>
import { ref } from "vue";
import TreeGraph from "./components/TreeGraph.vue";
import NodeDetails from "./components/NodeDetails.vue";

export default {
  components: {
    TreeGraph,
    NodeDetails
  },
  setup() {
    const selectedNode = ref(null);

    const onNodeSelect = (node,event) => {
      selectedNode?.value?.event?.target?.setAttribute('fill','steelblue');
      selectedNode.value = {node,event};
    };

    const onDeselect = () => {
      selectedNode.value.event.target.setAttribute('fill','steelblue');
      selectedNode.value = null;

    };

    return {
      selectedNode,
      onNodeSelect,
      onDeselect
    };
  }
};
</script>

<style scoped>
.app {
  display: flex;
  justify-content: space-between;
}

#tree-graph {
  flex: 2;
  border: 1px solid #ddd;
}

.node-details {
  flex: 1;
  padding: 10px;
  border-left: 1px solid #ddd;
}
</style>
