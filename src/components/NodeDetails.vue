
<template>
  <div v-if="node" class="node-details">
    <h2>Node Details</h2>
    <p><strong>Name:</strong> {{ node.node.name }}</p>
    <p><strong>Description:</strong> {{ node.node.description }}</p>
    <button @click="onDeselect">Deselect</button>
  </div>
</template>

<script>
export default {
  name: "NodeDetails",
  props: {
    node: Object
  },
  emits: ["deselect"],
  methods: {
    onDeselect() {
      this.$emit("deselect");
    }
  }
};
</script>
