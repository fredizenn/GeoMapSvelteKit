<script lang="ts">
  import { afterUpdate, createEventDispatcher, onMount } from "svelte";
  import ghanaGeoJson from "./world.geo.json";
  import northernGeoJson from "./northern.geo.json";
  import centralGeoJson from "./centralRegion.geo.json";
  import upperWestGeoJson from "./upperWest.geo.json";
  import upperEastGeoJson from "./upperEast.geo.json";
  import northEastGeoJson from "./northEast.geo.json";
  import savannahGeoJson from "./savannah.geo.json";
  import bonoGeoJson from "./bono.geo.json";
  import bonoEastGeoJson from "./bono-east.geo.json";
  import westernNorthGeoJson from "./western-north.geo.json";
  import westernGeoJson from "./western.geo.json";
  import easternGeoJson from "./eastern.geo.json";
  import ahafoGeoJson from "./ahafo.geo.json";
  import ashantiGeoJson from "./ashanti.geo.json";
  import voltaGeoJson from "./volta.geo.json";
  import greaterAccraGeoJson from "./greaterAccra.geo.json";
  import otiGeoJson from "./oti.geo.json";
  import * as echarts from "echarts";

  $: map = "Ghana";
  let geoJson: any = ghanaGeoJson;
  let chartInstance: any = null;
  const dispatch = createEventDispatcher();
  let stateName = "Ghana Map";

  let reset = false;

  $: showReset = () => {
    stateName !== "Ghana Map" ? (reset = true) : (reset = false);
  };



  function handleStateClick(params: any) {
    stateName = params.name;

    showReset();
    dispatch("stateClick", { state: stateName });

    switch (stateName) {
      case "Northern":
        map = "north";
        geoJson = northernGeoJson;
        echarts.registerMap("north", northernGeoJson);
        chartInstance.setOption({
          series: [
            {
              width: 400,
            },
          ],
        });
        break;
      case "Central":
        map = "central";
        geoJson = centralGeoJson;
        echarts.registerMap("central", centralGeoJson);
        break;
      case "Upper West":
        map = "upper-west";
        geoJson = upperWestGeoJson;
        echarts.registerMap("upper-west", upperWestGeoJson);
        break;
      case "Upper East":
        map = "upper-east";
        geoJson = upperEastGeoJson;
        echarts.registerMap("upper-east", upperEastGeoJson);
        break;
      case "North East":
        map = "north-east";
        geoJson = northEastGeoJson;
        echarts.registerMap("north-east", northEastGeoJson);
        break;
      case "Savannah":
        map = "savannah";
        geoJson = savannahGeoJson;
        echarts.registerMap("savannah", savannahGeoJson);
        break;
      case "Bono":
        map = "bono";
        geoJson = bonoGeoJson;
        echarts.registerMap("bono", bonoGeoJson);
        break;
      case "Bono East":
        map = "bono-east";
        geoJson = bonoEastGeoJson;
        echarts.registerMap("bono-east", bonoEastGeoJson);
        break;
      case "Western North":
        map = "western-north";
        geoJson = westernNorthGeoJson;
        echarts.registerMap("western-north", westernNorthGeoJson);
        break;
      case "Western":
        map = "western";
        geoJson = westernGeoJson;
        echarts.registerMap("western", westernGeoJson);
        break;
      case "Eastern":
        map = "eastern";
        geoJson = easternGeoJson;
        echarts.registerMap("eastern", easternGeoJson);
        break;
      case "Ahafo":
        map = "ahafo";
        geoJson = ahafoGeoJson;
        echarts.registerMap("ahafo", ahafoGeoJson);
        break;
      case "Ashanti":
        map = "ashanti";
        geoJson = ashantiGeoJson;
        echarts.registerMap("ashanti", ashantiGeoJson);
        break;
      case "Volta":
        map = "volta";
        geoJson = voltaGeoJson;
        echarts.registerMap("volta", voltaGeoJson);
        chartInstance.setOption({
          series: [
            {
              height: 800,
            },
          ],
        });
        break;
      case "Greater Accra":
        map = "greater-accra";
        geoJson = greaterAccraGeoJson;
        echarts.registerMap("greater-accra", greaterAccraGeoJson);
        break;
      case "Oti":
        map = "oti";
        geoJson = otiGeoJson;
        echarts.registerMap("oti", otiGeoJson);
        break;
      default:
        map = "Ghana";
        geoJson = otiGeoJson;
        echarts.registerMap("ghana", ghanaGeoJson);
        break;
    }
    console.log({ stateName });

    chartInstance.setOption({
      title: {
        subtext: stateName,
        // sublink: 'http://www.census.gov/popest/data/datasets.html',
        left: "right",
      },
      series: [
        {
          map: map,
          height: 400,
          data: geoJson?.features.map((feature: any) => {
            return {
              name: feature.properties.name,
              value: Math.random() * 10000000,
              itemStyle:
                feature.properties.NPP > feature.properties.NDC
                  ? "#0B4A95"
                  : "#336C57",
            };
          }),
        },
      ],
    });
  }

  function handleReset() {
    location.reload();
  }

  onMount(() => {
    map = "Ghana";
    geoJson = ghanaGeoJson;
    import("echarts").then((echarts) => {
      const chart = echarts.init(document.getElementById("echarts-container")!);
      chartInstance = chart;

      // ECharts configuration options for the Geo Map
      const options = {
        title: {
          text: "Ghana Map Test - Echarts::Svelte",
          subtext: stateName,
          // sublink: 'http://www.census.gov/popest/data/datasets.html',
          left: "right",
        },
        tooltip: {
          trigger: "item",
          showDelay: 0,
          transitionDuration: 0.2,
        },
        visualMap: {
          left: "right",
          min: 500000,
          max: 38000000,
          // inRange: {
          //   color: [
          //     "#313695",
          //     "#4575b4",
          //     "#74add1",
          //     "#abd9e9",
          //     "#e0f3f8",
          //     "#ffffbf",
          //     "#fee090",
          //     "#fdae61",
          //     "#f46d43",
          //     "#d73027",
          //     "#a50026",
          //   ],
          // },
          text: ["High", "Low"],
          calculable: true,
        },
        toolbox: {
          show: true,
          left: "left",
          top: "top",
          feature: {
            dataView: { readOnly: false },
            restore: {},
            saveAsImage: {},
          },
        },
        series: [
          {
            name: "Ghana",
            type: "map",
            roam: false,
            map: map,
            emphasis: {
              label: {
                show: true,
              },
            },
            width: 600,
            height: 800,
            data: geoJson?.features.map((feature: any) => {
              return {
                name: feature.properties.name,
                
                value: Math.random() * 10000000,
                itemStyle: {
                  areaColor: "#fff",
                  borderColor: "#808080",
                  color:
                    feature.properties.NPP > feature.properties.NDC
                      ? "#0B4A95"
                      : "#336C57",
                },
              };
            }),
          },
        ],
      };

      chart.on("click", handleStateClick);
      echarts.registerMap(map, geoJson);
      chart.setOption(options);
    });
  });

  // afterUpdate(() => {
  //   if (chartInstance) {
  //     // Update the chart instance when the map variable changes
  //     echarts.registerMap(map, geoJson);

  //     chartInstance.setOption({
  //       series: [
  //         {
  //           map: map,
  //           data: geoJson?.features.map((feature: any) => {
  //             return {
  //               name: feature.properties.name,
  //               value: Math.random() * 10000000,
  //               itemStyle:
  //                 feature.properties.NPP > feature.properties.NDC
  //                   ? "#0B4A95"
  //                   : "#336C57",
  //             };
  //           }),
  //         },
  //       ],
  //     });
  //   }
  // });

  $: {
    if (chartInstance) {
      echarts.registerMap(map, geoJson);

      chartInstance.setOption({
        series: [
          {
            map: map,
            // data: geoJson?.features.map((feature: any) => {
            //   return {
            //     name: feature.properties.name,
            //     value: Math.random() * 10000000,
            //     itemStyle:
            //       feature.properties.NPP > feature.properties.NDC
            //         ? "#0B4A95"
            //         : "#336C57",
            //   };
            // }),
          },
        ],
      });
    }
  }
</script>

{#if reset}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <button
    type="button"
    class="btn variant-filled rounded-none m-2"
    on:click={() => handleReset()}>Reset</button
  >
{/if}

<div class="" id="echarts-container" style="width: 100%; height: 900px;" />
