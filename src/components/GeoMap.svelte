<script lang="ts">
    import { onMount } from 'svelte';
    import worldGeoJson from './world.geo.json'; // Import the GeoJSON file for the world map

    onMount(() => {
      import('echarts').then(echarts => {
        const chart = echarts.init(document.getElementById('echarts-container')!);
        
        // ECharts configuration options for the Geo Map
        const options = {
    title: {
      text: 'Ghana Map Test - Echarts::Svelte',
      subtext: 'Gh',
      // sublink: 'http://www.census.gov/popest/data/datasets.html',
      left: 'right'
    },
    tooltip: {
      trigger: 'item',
      showDelay: 0,
      transitionDuration: 0.2
    },
    visualMap: {
      left: 'right',
      min: 500000,
      max: 38000000,
      inRange: {
        color: [
          '#313695',
          '#4575b4',
          '#74add1',
          '#abd9e9',
          '#e0f3f8',
          '#ffffbf',
          '#fee090',
          '#fdae61',
          '#f46d43',
          '#d73027',
          '#a50026'
        ]
      },
      text: ['High', 'Low'],
      calculable: true
    },
    toolbox: {
      show: true,
      //orient: 'vertical',
      left: 'left',
      top: 'top',
      feature: {
        dataView: { readOnly: false },
        restore: {},
        saveAsImage: {}
      }
    },
    series: [
      {
        name: 'Ghana',
        type: 'map',
        roam: true,
        map: 'Ghana',
        emphasis: {
          label: {
            show: true
          },
        },
        width: 600,

        height: 800,
        data: [
          { name: 'Upper West', value: 4822023 },
          { name: 'North East', value: 3590347 },
          { name: 'Upper East', value: 731449 },
          { name: 'Northern', value: 6553255 },
          { name: 'Savannah', value: 2949131 },
          { name: 'Bono', value: 38041430 },
          { name: 'Bono East', value: 9919945 },
          { name: 'Oti', value: 5187582 },
          { name: 'Western North', value: 917092 },
          { name: 'Ahafo', value: 632323 },
          { name: 'Ashanti', value: 19317568 },
          { name: 'Eastern', value: 1392313 },
          { name: 'Volta', value: 1595728 },
          { name: 'Western', value: 12875255 },
          { name: 'Central', value: 6537334 },
          { name: 'Greater Accra', value: 3074186 },
        ]
      }
    ]
  };

      
    echarts.registerMap('Ghana', worldGeoJson); // Register the GeoJSON data for the map
    chart.setOption(options);
      });
    });
  </script>
  
  <!-- <div class="p-2">
    <a href="/">Main Map</a>
  </div> -->
  <div class="mt-20" id="echarts-container" style="width: 100%; height: 900px;"></div>