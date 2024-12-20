
<template>
  <div id="tree-graph"></div>
</template>

<script>
import { onMounted, ref } from "vue";
import * as d3 from "d3";
import axios from "axios";

export default {
  name: "TreeGraph",
  emits: ["node-select"],
  setup(props, { emit }) {
    const data = ref(null);

    onMounted(() => {
      axios.get("http://localhost:3001/users").then((response) => {
        const transformedData = transformToHierarchy(response.data);
        data.value = transformedData;
        console.log(data)
        if (data.value) renderGraph(data.value);
      });
    });

    const transformToHierarchy = (data) => {
      const map = {};
      data.forEach((item) => (map[item.name] = { ...item, children: [] }));
      const hierarchy = [];
      data.forEach((item) => {
        if (item.parent) {
          map[item.parent].children.push(map[item.name]);
        } else {
          hierarchy.push(map[item.name]);
        }
      });
      return hierarchy[0]; // Root node
    };

    const renderGraph = (rootData) => {
      const width = 800;
      const height = 400;

      d3.select("#tree-graph").selectAll("*").remove();

      const svg = d3
        .select("#tree-graph")
        .append("svg")
        .attr("width", width)
        .attr("height", height);

      const root = d3.hierarchy(rootData);
      const treeLayout = d3.tree().size([width - 50, height - 50]);
      treeLayout(root);

      svg
        .selectAll(".link")
        .data(root.links())
        .enter()
        .append("line")
        .attr("class", "link")
        .attr("x1", (d) => d.source.x)
        .attr("y1", (d) => d.source.y+10)
        .attr("x2", (d) => d.target.x)
        .attr("y2", (d) => d.target.y+10)
        .attr("stroke", "#ccc");

      svg
        .selectAll(".node")
        .data(root.descendants())
        .enter()
        .append("circle")
        .attr("class", "node")
        .attr("cx", (d) => d.x)
        .attr("cy", (d) => d.y+10)
        .attr("r", 10)
        .attr("fill", "steelblue")
        .on("click", (event, d) =>{ emit("node-select", d.data, event); 
        console.log(event);
        event.target.setAttribute('fill','red');
        });
    };

    onMounted(() => {
      
    });

    return {
      data,
      renderGraph
    };
  }
};
</script>
