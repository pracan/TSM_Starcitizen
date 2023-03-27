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
  type: 'Planet',
  children: [
    {
      name: 'OM',
      children: [
        { name: 'OM-1', no_qt: 'OM-1' },
        { name: 'OM-2', no_qt: 'OM-2' },
        { name: 'OM-3', no_qt: 'OM-4' },
        { name: 'OM-4', no_qt: 'OM-3' },
        { name: 'OM-5', no_qt: 'OM-6' },
        { name: 'OM-6', no_qt: 'OM-5' },
      ],
    },
    {
      name: 'space',
      children: [
        {
          name: 'Baijin Point',
          qt_point: [{ name: 'OM-2' }, { name: 'OM-4' }, { name: 'OM-6' }],
        },
        {
          name: 'Comm Array ST3-90',
          qt_point: [{ name: 'OM-4' }, { name: 'OM-5' }],
        },
      ],
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
        { name: 'The Sky Scrapper' },
      ],
    },
    {
      name: 'Lyria',
      type: 'Moon',
      children: [
        {
          name: 'OM',
          children: [
            { name: 'OM-1', no_qt: 'OM-1' },
            { name: 'OM-2', no_qt: 'OM-2' },
            { name: 'OM-3', no_qt: 'OM-4' },
            { name: 'OM-4', no_qt: 'OM-3' },
            { name: 'OM-5', no_qt: 'OM-6' },
            { name: 'OM-6', no_qt: 'OM-5' },
          ],
        },
        {
          name: 'space',
          children: [
            {
              name: 'Comm Array ST3-18',
              qt_point: [
                { name: 'OM-1' },
                { name: 'OM-2' },
                { name: 'OM-3' },
                { name: 'OM-4' },
                { name: 'OM-6' },
              ],
            },
          ],
        },
        {
          name: 'no_qt',
          children: [
            {
              name: 'Paradise Cove',
              near: [{ name: "Teddy's Playhouse" }],
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
      type: 'Moon',
      children: [
        {
          name: 'OM',
          children: [
            { name: 'OM-1', no_qt: 'OM-1' },
            { name: 'OM-2', no_qt: 'OM-2' },
            { name: 'OM-3', no_qt: 'OM-4' },
            { name: 'OM-4', no_qt: 'OM-3' },
            { name: 'OM-5', no_qt: 'OM-6' },
            { name: 'OM-6', no_qt: 'OM-5' },
          ],
        },
        {
          name: 'space',
          children: [
            {
              name: 'Comm Array ST3-35',
              qt_point: [
                { name: 'OM-1' },
                { name: 'OM-2' },
                { name: 'OM-4' },
                { name: 'OM-5' },
              ],
            },
          ],
        },
        {
          name: 'no_qt',
          children: [
            {
              name: 'Good Times Temple',
              near: [{ name: 'Shady Glen Farms' }],
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
  // console.log(graphData);
  graph.import(graphData);
  // console.log(graph);
  graph.nodes().forEach((node, i) => {
    // console.log(graph.getNodeAttributes(node));
    graph.setNodeAttribute(node, 'size', 5);
    // graph.setNodeAttribute(node, 'type', 'image');
    // graph.setNodeAttribute(
    //   node,
    //   'image',
    //   'https://img95.699pic.com/photo/50034/0209.jpg_wh300.jpg'
    // );
    let bob = graph.getNodeAttribute(node, 'label');
    let bob2 = String(bob);
    // console.log('Value :', bob);
    // console.log(bob.toString());
    // console.log(bob2 == 'ArcCorp' ? 'True' : 'False');
    switch (bob2) {
      case 'space':
        graph.setNodeAttribute(node, 'color', '#0066cc');
        // console.log('True : space');
        break;
      case 'no_qt':
        graph.setNodeAttribute(node, 'color', '#ff9900');
        // console.log('True : no_qt');
        break;
      case 'ArcCorp':
        graph.setNodeAttribute(node, 'color', '#00cc66');
        //graph.setNodeAttribute(node, 'label', node.name);
        graph.setNodeAttribute(node, 'size', '15');
        // console.log('True : ArcCorp');
        break;
      case 'Lyria':
        graph.setNodeAttribute(node, 'color', '#cc0066');
        //graph.setNodeAttribute(node, 'label', node.name);
        graph.setNodeAttribute(node, 'size', '10');
        // console.log('True : Lyria');
        break;
      case 'Wala':
        graph.setNodeAttribute(node, 'color', '#9900cc');
        //graph.setNodeAttribute(node, 'label', node.name);
        // console.log('True : Wala');
        graph.setNodeAttribute(node, 'size', '10');
        break;
      default:
        graph.setNodeAttribute(node, 'color', '#cccccc');
        // console.log('Default');
        break;
    }
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
  console.log(render);

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

function transformData(data) {
  const nodes = [];
  let edges = [];
  let count = 0;

  // Créer les noeuds à partir des données de l'API
  function createNodes(obj, parent) {
    const node = {
      key: count++,
      id: parent ? `${parent}.${obj.name}` : obj.name,
      attributes: {
        label: obj.name,
        parent: parent ? `${parent}.${obj.name}` : obj.name,
        sc_type: obj['type'] || '',
      },
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

  //WARNING if 2 space objects are too close to eachothers this will create dupes
  function addSpaceObjQt_point(obj, parent) {
    const results = [];

    function findSpaceObj(obj, parent) {
      if (obj.children) {
        for (const child of obj.children) {
          findSpaceObj(child, parent ? `${parent}.${obj.name}` : obj.name);
        }
      } else if (obj.qt_point) {
        // console.log('Space : obj :', obj);
        results.push(obj);
      }
    }

    function removeDupes() {
      //please code me
      //problem is linked to the bidirectional edges
    }

    findSpaceObj(obj, parent);
    // console.log('results :', results);
    removeDupes();
    for (const openresult of results) {
      // console.log('openresult :', openresult);
      const spaceObjNode = nodes.find(
        (node) => node.attributes.label === openresult.name
      );
      // console.log('spaceObjNode :', spaceObjNode);
      let temp_id = String(`${spaceObjNode.id}`);
      let temp_charcount = String(`${spaceObjNode.label}`);
      // console.log('temp_id 1:', temp_id);
      // console.log('temp_charcount :', temp_charcount);
      // console.log('temp_charcount.length :', temp_charcount.length);
      let temp_parent =
        temp_id.slice(0, -1 * parseInt(temp_charcount.length + 1) - 6) +
        '.' +
        'OM' +
        '.';
      console.log('temp_id 2:', temp_parent);
      for (const qpoint of openresult.qt_point) {
        // console.log('qpoint :', qpoint);
        let realparent = temp_parent + qpoint.name;
        // console.log('realparent :', realparent);
        const reachableSpaceObjNode = nodes.find(
          (node) => node.attributes.parent === realparent
        );
        // console.log('reachableSpaceObjNode :', reachableSpaceObjNode);
        edges.push(
          {
            key: count++,
            source: spaceObjNode.key,
            target: reachableSpaceObjNode.key,
          },
          {
            key: count++,
            source: reachableSpaceObjNode.key,
            target: spaceObjNode.key,
          }
        );
      }
    }

    return results;
  }

  // function addJumpEdges(nodes, edges) {
  //   const arcCorpNode = nodes.find(
  //     (node) => node.attributes.label === 'ArcCorp'
  //   );
  //   console.log('arcCorpNode :', arcCorpNode);
  //   console.log('arcCorpNode.atr.parent :', arcCorpNode.attributes.parent);
  //   const arcCorpChildrenLabels = edges
  //     .filter((edge) => edge.source === arcCorpNode.key)
  //     .map(
  //       (edge) =>
  //         nodes.find((node) => node.key === edge.target).attributes.label
  //     );
  //   console.log('arcCorpChildrenLabels :', arcCorpChildrenLabels);
  //   const jumpNode = nodes.find(
  //     (node) =>
  //       node.attributes.parent ===
  //       String(arcCorpNode.attributes.parent) + '.' + 'OM'
  //   );
  //   console.log('jumpNode :', jumpNode);

  //   if (jumpNode) {
  //     const jumpNodeChildrenLabels = edges
  //       .filter((edge) => edge.source === jumpNode.key)
  //       .map(
  //         (edge) =>
  //           nodes.find((node) => node.key === edge.target).attributes.label
  //       );
  //     console.log('jumpNodeChildrenLabels :', jumpNodeChildrenLabels);

  //     for (const jumpchild of jumpNodeChildrenLabels) {
  //       console.log('jumpchild :', jumpchild);
  //       const jumpchildNode = nodes.find(
  //         (node) => node.attributes.label === jumpchild
  //       );
  //       console.log('jumpchildNode :', jumpchildNode);
  //       for (const arcchild of arcCorpChildrenLabels) {
  //         console.log('arcchild :', arcchild);
  //         const arcchildNode = nodes.find(
  //           (node) => node.attributes.label === arcchild
  //         );
  //         console.log('arcchildNode :', arcchildNode);
  //         if (
  //           arcchildNode.attributes.label !== 'Self' &&
  //           arcchildNode.attributes.label !== 'Jump'
  //         ) {
  //           edges.push({
  //             key: count++,
  //             source: jumpchildNode.key,
  //             target: arcchildNode.key,
  //           });
  //         }
  //       }
  //     }
  //   }
  //   return { edges };
  // }

  function addOMJumpEdges(nodes, edges) {
    const planets = nodes.filter(
      (node) => node.attributes.sc_type === 'Planet'
    );
    // console.log('planets :', planets);
    for (const planet of planets) {
      const planetChildrenLabels = edges
        .filter((edge) => edge.source === planet.key)
        .map(
          (edge) =>
            nodes.find((node) => node.key === edge.target).attributes.label
        );
      // console.log('planetChildrenLabels :', planetChildrenLabels);
      const omNode = nodes.find(
        (node) =>
          node.attributes.parent === `${planet.attributes.parent}` + `.` + `OM`
      );
      console.log('omNode :', omNode);
      const spaceNode = nodes.find(
        (node) =>
          node.attributes.parent ===
          `${planet.attributes.parent}` + `.` + `space`
      );
      console.log('spaceNode :', spaceNode);
      const reachableNodes = [];
      reachableNodes.push(omNode);
      reachableNodes.push(spaceNode);
      for (const jumpNode of reachableNodes) {
        const jumpNodeChildrenLabels = edges
          .filter((edge) => edge.source === jumpNode.key)
          .map(
            (edge) =>
              nodes.find((node) => node.key === edge.target).attributes.label
          );
        // console.log('jumpNodeChildrenLabels :', jumpNodeChildrenLabels);
        for (const jumpchild of jumpNodeChildrenLabels) {
          const jumpchildNode = nodes.find(
            (node) => node.attributes.label === jumpchild
          );
          // console.log('jumpchild :', jumpchild);
          // console.log('jumpchildNode :', jumpchildNode);
          for (const planetChild of planetChildrenLabels) {
            const planetChildNode = nodes.find(
              (node) => node.attributes.label === planetChild
            );
            // console.log('planetChild :', planetChild);
            // console.log('planetChildNode :', planetChildNode);
            if (
              planetChildNode.attributes.label !== 'Self' &&
              planetChildNode.attributes.label !== 'space' &&
              planetChildNode.attributes.label !== 'OM'
            ) {
              edges.push(
                {
                  key: count++,
                  source: jumpchildNode.key,
                  target: planetChildNode.key,
                },
                {
                  key: count++,
                  source: planetChildNode.key,
                  target: jumpchildNode.key,
                }
              );
            }
          }
        }
      }
    }

    return { edges };
  }

  function addOMSelfEdges(nodes, edges) {
    const OMParents = nodes.filter((node) => node.attributes.label === 'OM');
    // console.log('OMParents :', OMParents);
    for (const OMParent of OMParents) {
      const OMChildrenLabels = edges
        .filter((edge) => edge.source === OMParent.key)
        .map((edge) => nodes.find((node) => node.key === edge.target));
      // console.log('OMChildrenLabels :', OMChildrenLabels);

      for (const OMChildrenNode of OMChildrenLabels) {
        // console.log('OMChildrenNode.id', OMChildrenNode.id);
        const id_temp = String(`${OMChildrenNode.id}`).slice(-1);
        // console.log('id_temp :', id_temp);
        let exclude_id = String(`${OMChildrenNode.id}`).slice(0, -1);
        if (parseInt(id_temp) % 2 == 0) {
          exclude_id += `${parseInt(id_temp) - 1}`;
          // console.log('0 : exclude_id :', exclude_id);
        } else {
          exclude_id += `${parseInt(id_temp) + 1}`;
          // console.log('1 : exclude_id :', exclude_id);
        }
        for (const OMChildBrotherNode of OMChildrenLabels) {
          // console.log(
          //   'Exclude id : |',
          //   `${exclude_id}`,
          //   '| OMChildBrotherNode.attributes.id : |',
          //   `${OMChildBrotherNode.id}`,
          //   '| Different ?',
          //   `${OMChildBrotherNode.id}` !== `${exclude_id}` ? 'True' : 'False',
          //   '| Allowed ?',
          //   OMChildBrotherNode.id !== OMChildrenNode.id ? 'True' : 'False'
          // );
          if (
            `${OMChildBrotherNode.id}` !== `${exclude_id}` &&
            OMChildBrotherNode.id !== OMChildrenNode.id
          ) {
            edges.push({
              key: count++,
              source: OMChildrenNode.key,
              target: OMChildBrotherNode.key,
            });
          }
        }
      }
    }

    return { edges };
  }

  function addNoQTEdges(obj, nodes, edges) {
    const results = [];
    function findNoQTObj(obj, parent) {
      if (obj.near) {
        // console.log('Space : obj :', obj);
        results.push(obj);
      } else if (obj.children) {
        for (const child of obj.children) {
          findNoQTObj(child, parent ? `${parent}.${obj.name}` : obj.name);
        }
        return 1;
      }
    }

    const dummy = findNoQTObj(obj, parent);
    console.log('NoQTresults :', results);
    for (const openresult of results) {
      console.log('openresult :', openresult);
      const spaceObjNode = nodes.find(
        (node) => node.attributes.label === openresult.name
      );
    }

    const NoQTParents = nodes.filter(
      (node) => node.attributes.label === 'no_qt'
    );
    console.log('NoQTParents :', NoQTParents);
    for (const NoQTParent of NoQTParents) {
      const NoQTChildrenLabels = edges
        .filter((edge) => edge.source === NoQTParent.key)
        .map((edge) => nodes.find((node) => node.key === edge.target));

      console.log('NoQTChildrenLabels :', NoQTChildrenLabels);
    }

    return { edges };
  }

  createNodes(data, null);
  addSpaceObjQt_point(data, null);
  // console.log(edges);
  edges = addOMJumpEdges(nodes, edges).edges;
  console.log('nodes :', nodes);
  // console.log('edges :', edges);
  edges = addOMSelfEdges(nodes, edges).edges;
  console.log('edges :', edges);
  addNoQTEdges(data, nodes, edges);

  //console.log(JSON.stringify({ nodes, edges }));

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
  text-align: left;
}
button + button {
  margin-left: 10px;
}
</style>
