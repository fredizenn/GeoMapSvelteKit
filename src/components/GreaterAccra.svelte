<script lang="ts">
    import { onMount } from 'svelte';
    import greaterAccraGeoJson from './greaterAccra.geo.json'
  import Modal from './modal.svelte';
  import NorthernGeo from './NorthernGeo.svelte';
    

    let showModal = false;

    function openModal () {
        showModal = true;
    }

    function closeModal () {
        showModal = false;
    }

    let selectedOption: any = null;
    onMount(() => {
      import('echarts').then(echarts => {
        const chart = echarts.init(document.getElementById('echarts-container')!);
        
        // ECharts configuration options for the Geo Map
        const options = {
    title: {
      text: 'Ghana Map Test - Echarts::Svelte',
      subtext: 'Greater Accra Region with constituents',
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
        map: 'accra',
        emphasis: {
          label: {
            show: true
          }
        },
        width: 800,
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
          { name: 'P', value: 13074186 },
          { name: 'Q', value: 103341746 },
          { name: 'R', value: 124186 },
          { name: 'S', value: 12684186 },
          { name: 'T', value: 864186 },
          { name: 'U', value: 664186 },
          { name: 'V', value: 1934186 },
          { name: 'W', value: 12374186 },
          { name: 'X', value: 86 },
          { name: 'Y', value: 3342186 },
          { name: 'Z', value: 1074186 },
          { name: 'Z1', value: 4186 },
          { name: 'Z2', value: 174186 },
          { name: 'Z3', value: 74186 },
          { name: 'Z4', value: 11374186 },


        ]
      }
    ]
  };

    chart.on('click', handleStateClick);
    echarts.registerMap('accra', greaterAccraGeoJson); // Register the GeoJSON data for the map
    chart.setOption(options);
      });
    });

    function handleStateClick(params: any) {
        const stateName = params.name;
        selectedOption = {
            name: stateName
        }
        console.log(stateName);
        openModal()
    }
  </script>
  
  <!-- <div class="p-2">
    <a href="/">Main Map</a>
  </div> -->
  <div id="echarts-container" style="width: 100%; height: 650px;"></div>

  <Modal title={selectedOption?.name} bind:open={showModal} showIcon={false} on:close={() => (showModal = false)}>
    <NorthernGeo />
</Modal>