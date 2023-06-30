<script lang="ts" context="module">
    export interface IButton {
      text: string
      ghost?: boolean
      primary?: boolean
      dangerous?: boolean
      handler: (_: IButton) => void
    }

    import northernGeoJson from './northern.geo.json'
    
    
  </script>
  
  <script lang="ts">
    // import { Icon } from "@steeze-ui/svelte-icon"
    // import { ExclamationCircle, Check, XMark } from "@steeze-ui/heroicons"
    import { fade, fly } from "svelte/transition"
    import { createEventDispatcher, onMount } from "svelte"
    import { cubicIn, cubicOut, elasticInOut } from "svelte/easing"
      import Icon from '@iconify/svelte';
    export let title: string = ""
    export let closeFn: () => boolean = null
  
    export let dangerous = false
    export let open = true
    export let busy = false
    export let grayBg = false
    export let padded = true
    // export let force = true
    export let icon = dangerous ? "bi:exclamation-circle" : "material-symbols:check" 
    export let xl = false
    export let lg = false
    export let md = false
  
    export let showCloseButton = true
    export let showIcon =  true
    export let buttons: IButton[] = []
  
    let dispatch = createEventDispatcher()
  
    function onClose(){
      dispatch("close", { src: "closeButton"})
      if (closeFn) {
        if (closeFn())
          return
      }
      open = false
    }
    
   
    export let map: string;
    export let importValue: any;

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
        map: 'accra',
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
          { name: 'Q', value: 10746 },
          { name: 'R', value: 374186 },

        ]
      }
    ]
  };

      
    echarts.registerMap('accra', northernGeoJson); // Register the GeoJSON data for the map
    chart.setOption(options);
      });
    });
  
  </script>
  
  
  <div class="relative z-[1000] bg-white" aria-labelledby="modal-title" role="dialog" aria-modal="true" class:hidden={!open}>
    {#if open}
    <div class="fixed inset-0 bg-gray-500 bg-opacity-50 transition-opacity" 
      in:fade={{duration:300, easing: cubicIn}} 
      out:fade={{duration:200, easing: elasticInOut}}></div>
    {/if}   
    <div class="fixed  z-10 inset-0  overflow-y-auto " >
      <div class="flex  items-end sm:items-center justify-center min-h-full p-4 text-center sm:p-0 ">
        {#if open}
        <div class="relative bg-white text-left bg-none overflow-hidden shadow-xl transform transition-all sm:my-8 lg:max-w-xl xl:max-w-2xl sm:w-full  "
            class:bg-gray={grayBg}
            class:sm:max-w-7xl={xl}
            class:sm:max-w-4xl={lg}
            class:sm:max-w-3xl={md}
            in:fly={{ y: 50 }} out:fly={{ y: -50 }} on:introstart on:outroend
          >
          <div class="{padded && 'px-4 pt-5 pb-4 sm:p-6 sm:pb-4'} ">
            {#if showCloseButton}
            <div class="hiddenx sm:block absolute top-0 right-0 pt-4 pr-4">
              <button type="button" class=" rounded-md text-gray-900 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500" on:click={onClose} class:bg-gray={grayBg}>
                <span class="sr-only">Close</span>
                <Icon icon="carbon:close" style="font-size: 32px"/>
              </button>
            </div>
            {/if}
            <div class="sm:flex sm:items-start ">
              {#if showIcon}
              <div class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full sm:mx-0 sm:h-10 sm:w-10 {dangerous ? "text-red-600" : "text-green-600"}" class:bg-red-100={dangerous} class:bg-green-100={!dangerous}>
                
                <Icon {icon}  style="font-size: 25px"/>
              </div>
              {/if}
              <div class="mt-3 text-center sm:mt-0 sm:text-left w-full ">
                <h3 class="text-lg leading-6 font-medium text-gray-900" class:hidden={!title}>{title}</h3>
                <div class=" p-0 m-0 border" class:hidden={!title}></div>
                <div class="mt-2 ">
                  <div id="echarts-container" style="width: 100%; height: 650px;"></div>
                </div>
              </div>
            </div>
          </div>
          {#if buttons && buttons.length}
          <div class="bg-primary-600 px-4 py-3 sm:px-6 sm:flex sm:xflex-row-reverse sm:place-content-end gap-3">
            {#each buttons as button}
              <button type="button" on:click={_ => button.handler(button)} class:primary={button.primary} class:ghost={button.ghost} class:dangerous={button.dangerous} class="w-full inline-flex justify-center rounded-md px-4 py-2 text-base font-medium text-white border-transparent border shadow-sm sm:w-auto sm:text-sm mt-3">
                {button.text}
              </button>
            {/each}
          </div>
          {/if}
        </div>
        {/if}
      </div>
    </div>
  </div>
  
  