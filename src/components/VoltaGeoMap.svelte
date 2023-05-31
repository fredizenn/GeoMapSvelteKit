<script lang="ts">
    import { onMount } from 'svelte';
    import voltaGeoJson from './volta.geo.json'; // Import the GeoJSON file for the world map

    onMount(() => {
      import('echarts').then(echarts => {
        const chart = echarts.init(document.getElementById('echarts-container')!);
        
        // ECharts configuration options for the Geo Map
        const options = {
    title: {
      text: 'Ghana Map Test - Echarts::Svelte',
      subtext: 'Volta ',
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
        map: 'volta',
        emphasis: {
          label: {
            show: true
          }
        },
        data: [
          { name: 'Ahafo', value: 4822023 },
          { name: 'Ashanti', value: 3590347 },
          { name: 'Bono', value: 731449 },
          { name: 'Bono East', value: 6553255 },
          { name: 'Central', value: 2949131 },
          { name: 'Eastern', value: 38041430 },
          { name: 'Greater Accra', value: 9919945 },
          { name: 'Northern', value: 5187582 },
          { name: 'North East', value: 917092 },
          { name: 'Oti', value: 632323 },
          { name: 'Savannah', value: 19317568 },
          { name: 'Upper East', value: 1392313 },
          { name: 'Upper West', value: 1595728 },
          { name: 'Volta', value: 12875255 },
          { name: 'Western', value: 6537334 },
          { name: 'Western North', value: 3074186 },
        ]
      }
    ]
  };

      
    echarts.registerMap('volta', voltaGeoJson); // Register the GeoJSON data for the map
    chart.setOption(options);
      });
    });
  </script>
  
  <div class="p-2">
    <a href="/">Main Map</a>
  </div>
  <div id="echarts-container" style="width: 100%; height: 650px;"></div>