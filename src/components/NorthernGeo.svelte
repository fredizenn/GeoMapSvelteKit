<script lang="ts">
    import { onMount } from 'svelte';
    import northernGeoJson from './northern.geo.json'
    

    onMount(() => {
      import('echarts').then(echarts => {
        const chart = echarts.init(document.getElementById('echarts-container')!);
        
        // ECharts configuration options for the Geo Map
        const options = {
    title: {
      text: 'Ghana Map Test - Echarts::Svelte',
      subtext: 'Northern Region with constituents',
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
        name: 'North',
        type: 'map',
        roam: true,
        map: 'north',
        emphasis: {
          label: {
            show: true
          }
        },
        width: 500,
        height: 600,
        data: [
          { name: 'A', value: 4822023 },
          { name: 'B', value: 3590347 },
          { name: 'C', value: 731449 },
          { name: 'D', value: 6553255 },
          { name: 'E', value: 2949131 },
          { name: 'F', value: 38041430 },
          { name: 'G', value: 9919945 },
          { name: 'H', value: 5187582 },
          { name: 'I', value: 917092 },
          { name: 'J', value: 632323 },
          { name: 'K', value: 19317568 },
          { name: 'L', value: 1392313 },
          { name: 'M', value: 1595728 },
          { name: 'N', value: 12875255 },
          { name: 'O', value: 6537334 },
          { name: 'P', value: 3074186 },
        ]
      }
    ]
  };

      
    echarts.registerMap('north', northernGeoJson); // Register the GeoJSON data for the map
    chart.setOption(options);
      });
    });
  </script>
  
  <!-- <div class="p-2">
    <a href="/">Main Map</a>
  </div> -->
  <div id="echarts-container" style="width: 100%; height: 650px;"></div>