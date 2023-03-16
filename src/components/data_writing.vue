<template>
  <div>
    <button @click="writeToFile">Write to File</button>
  </div>
</template>

<script>
import fs from 'fs';

export default {
  name: 'MyComponent',
  methods: {
    transformData(data) {
      // Your transformData function implementation
    },
    writeToFile() {
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
      // Assuming you have your data stored in a variable called `jsonData`
      //const transformedData = this.transformData(jsonData);
      const transformedData = this.jsonData;

      // Write the transformed data to a file called "data.json"
      fs.writeFile('new_data.json', JSON.stringify(transformedData), (err) => {
        if (err) throw err;
        console.log('The file has been saved!');
      });
    },
  },
};
</script>
