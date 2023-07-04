<script lang="ts">
    import { onMount } from 'svelte';
    import centralGeoJson from './centralRegion.geo.json'
  import Modal from './modal.svelte';
  import NorthernGeo from './NorthernGeo.svelte';
  import { ChartView } from 'echarts';
    

    let showModal = false;

    function openModal () {
        showModal = true;
    }

    function closeModal () {
        showModal = false;
    }

    const map = 'accra'
    const importValue: any = centralGeoJson;

    const data = [
          { name: 'Constituency A', value: 4822023 },
          { name: 'Constituency B', value: 3590347 },
          { name: 'Constituency C', value: 731449 },
          { name: 'Constituency D', value: 6553255 },
          { name: 'Constituency E', value: 2949131 },
          { name: 'Constituency F', value: 38041430 },
          { name: 'Constituency G', value: 9919945 },
          { name: 'Constituency H', value: 5187582 },
          { name: 'Constituency I', value: 917092 },
          { name: 'Constituency J', value: 632323 },
          { name: 'Constituency K', value: 19317568 },
          { name: 'Constituency L', value: 1392313 },
          { name: 'Constituency M', value: 1595728 },
          { name: 'Constituency N', value: 12875255 },
          { name: 'Constituency O', value: 6537334 },
          { name: 'Constituency P', value: 13074186 },
          { name: 'Constituency Q', value: 103341746 },
          { name: 'Constituency R', value: 124186 },
          { name: 'Constituency S', value: 12684186 },
          { name: 'Constituency T', value: 864186 },
          { name: 'Constituency U', value: 664186 },
          { name: 'Constituency V', value: 1934186 },
          { name: 'Constituency W', value: 12374186 },
          { name: 'Constituency X', value: 86 },

        ]
    let selectedOption: any = null;
    onMount(() => {
      import('echarts').then(echarts => {
        const chart = echarts.init(document.getElementById('echarts-container')!);
        
        // ECharts configuration options for the Geo Map
    const options = {
    title: {
      text: 'Ghana Map Test - Echarts::Svelte',
      subtext: 'Central Region with constituents. Click on the Geo map to show a bar chart',
    
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
        name: 'Greater Accra',
        type: 'map',
        roam: true,
        map: 'central',
        emphasis: {
          label: {
            show: true
          }
        },
        width: 800,
        height: 600,
        data: data
      }
    ]
  };

    
  const barChartOptions = {
      title: {
        text: 'Bar Chart',
        left: 'center',
      },
      tooltip: {
        trigger: 'axis',
      },
      xAxis: {
        type: 'value',
      },
      yAxis: {
        type: 'category',
      },
      series: [
        {
          type: 'bar',
          data: data,
        },
      ],
    };

    let currentOptions = options

    echarts.registerMap('central', centralGeoJson); // Register the GeoJSON data for the map
    chart.setOption(currentOptions);
    chart.on('click', currentOptions === options ? transitionToBarChart : transitionToDefault);

    function transitionToBarChart() {
      currentOptions = barChartOptions
      chart.setOption(currentOptions);
    }

    function transitionToDefault()  {
      chart.setOption(options);
    }
      });

      
    });

    
  </script>
  
  <!-- <div class="p-2">
    <a href="/">Main Map</a>
  </div> -->
  <div id="echarts-container" style="width: 100%; height: 650px;"></div>

  