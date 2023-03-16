<template>
  <div>
    <button id="start">start</button>
    <button id="reset">reset</button>
  </div>
  <div id="sigma-container"></div>
</template>

<script setup>
import Graph from 'graphology';
import { circlepack } from 'graphology-layout';
import FA2Layout from 'graphology-layout-forceatlas2/worker';
import forceAtlas2 from 'graphology-layout-forceatlas2';
import Sigma from 'sigma';
import getNodeProgramImage from 'sigma/rendering/webgl/programs/node.image';
import dataJson from '../assets/sample_data.json';
import { onMounted, onUnmounted } from 'vue';

const jsonData = {
  name: 'ArcCorp',
  children: [
    {
      name: 'Jump',
      children: [
        { name: 'OM-1' },
        { name: 'OM-2' },
        { name: 'OM-3' },
        { name: 'OM-4' },
        { name: 'OM-5' },
        { name: 'OM-6' },
      ],
    },
    {
      name: 'Space',
      children: [{ name: 'Baijin Point' }, { name: 'Comm Array ST3-90' }],
    },
    {
      name: 'Self',
      children: [
        { name: 'Area18' },
        { name: 'Area04' },
        { name: 'Area06' },
        { name: 'Area11' },
        { name: 'Area17' },
        { name: 'Area20' },
      ],
    },
    {
      name: 'Lyria',
      children: [
        {
          name: 'space',
          children: [{ name: 'Comm Array ST3-18' }],
        },
        {
          name: 'no_qt',
          children: [
            {
              name: 'Paradise Cove',
              children: [{ name: "Teddy's Playhouse" }],
            },
          ],
        },
        { name: 'Shubin Mining Facility SAL-2' },
        { name: 'Shubin Mining Facility SAL-5' },
        { name: 'Loveridge Mineral Reserve' },
        { name: 'Humboldt Mines' },
        { name: 'The Orphanage' },
        { name: 'The Pit' },
        { name: "Wheeler's" },
        { name: 'Security Depot Lyria-1' },
        { name: "Teddy's Playhouse" },
        { name: 'Shubin Processing Facility SPAL-3' },
        { name: 'Shubin Processing Facility SPAL-7' },
        { name: 'Shubin Processing Facility SPAL-9' },
        { name: 'Shubin Processing Facility SPAL-12' },
        { name: 'Shubin Processing Facility SPAL-16' },
        { name: 'Shubin Processing Facility SPAL-21' },
        { name: 'Elsewhere' },
        { name: 'Launch Pad' },
        { name: 'Buckets' },
      ],
    },
    {
      name: 'Wala',
      children: [
        {
          name: 'space',
          children: [{ name: 'Comm Array ST3-35' }],
        },
        {
          name: 'no_qt',
          children: [
            {
              name: 'Good Times Temple',
              children: [{ name: 'Shady Glen Farms' }],
            },
          ],
        },
        { name: 'ArcCorp Mining Area 045' },
        { name: 'ArcCorp Mining Area 048' },
        { name: 'ArcCorp Mining Area 056' },
        { name: 'ArcCorp Mining Area 061' },
        { name: 'ArcCorp Processing Center 115' },
        { name: 'ArcCorp Processing Center 123' },
        { name: 'Lost and Found' },
        { name: 'Samson & Sons Salvage Center' },
        { name: 'Shady Glen Farms' },
      ],
    },
  ],
};

let fa2Layout = null;
onMounted(() => {
  const graph = new Graph({ multi: true });
  const container = document.getElementById('sigma-container');
  //dataJson.data
  const graphData = transformData(jsonData);
  graph.import(graphData);
  console.log(graph);
  graph.nodes().forEach((node, i) => {
    graph.setNodeAttribute(node, 'size', 5);
    // graph.setNodeAttribute(node, 'type', 'image');
    // graph.setNodeAttribute(
    //   node,
    //   'image',
    //   'https://img95.699pic.com/photo/50034/0209.jpg_wh300.jpg'
    // );
  });
  graph.edges().forEach((edge, i) => {
    graph.setEdgeAttribute(edge, 'type', 'arrow');
  });
  circlepack.assign(graph);
  const settings = forceAtlas2.inferSettings(graph);
  fa2Layout = new FA2Layout(graph, { settings });
  console.log(graph._nodes);
  const render = new Sigma(graph, container, {
    allowInvalidContainer: true,
    renderEdgeLabels: true, // 显示线的label
    nodeProgramClasses: {
      image: getNodeProgramImage(),
    },
  });

  const startBtn = document.getElementById('start');
  const resetBtn = document.getElementById('reset');
  startBtn.addEventListener('click', () => {
    fa2Layout.start();
  });
  resetBtn.addEventListener('click', () => {
    if (fa2Layout.isRunning) fa2Layout.stop();
    circlepack.assign(graph);
    render.refresh();
  });
});

onUnmounted(() => {
  if (fa2Layout) {
    fa2Layout.kill();
  }
});

// function transformData(data) {
//   const nodes = [];
//   const edges = [];
//   let count = 0;

//   // Créer les noeuds à partir des données de l'API
//   for (const person of data) {
//     const node = { key: count++, id: person.uid, label: person.name };
//     nodes.push(node);
//   }

//   // Créer les arrêtes (liens) à partir des données de l'API
//   edges.push({ key: count++, source: nodes[0].key, target: nodes[1].key });

//   return { nodes, edges };
// }

function transformData(data) {
  const nodes = [];
  const edges = [];
  let count = 0;

  // Créer les noeuds à partir des données de l'API
  function createNodes(obj, parent) {
    const node = {
      key: count++,
      id: parent ? `${parent}.${obj.name}` : obj.name,
      label: obj.name,
    };
    nodes.push(node);

    if (obj.children) {
      for (const child of obj.children) {
        edges.push({ key: count++, source: node.key, target: count });
        createNodes(child, node.id);
      }
    }
  }

  createNodes(data, null);

  return { nodes, edges };
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#sigma-container {
  position: relative;
  height: 90vh;
  border: 1px solid #e8e8e8;
  border-radius: 8px;
}
button + button {
  margin-left: 10px;
}
</style>
