<script lang="ts">
import '@skeletonlabs/skeleton/themes/theme-skeleton.css';
import '@skeletonlabs/skeleton/styles/skeleton.css';
import '../app.postcss';	
import { TabGroup, Tab } from '@skeletonlabs/skeleton';
  import GeoMap from '../components/GeoMap.svelte';
  import VoltaGeoMap from '../components/VoltaGeoMap.svelte';
  import NorthernGeo from '../components/NorthernGeo.svelte';
  import GreaterAccra from '../components/GreaterAccra.svelte';
let tabSet: number = 0;

</script>


<TabGroup>
	<Tab bind:group={tabSet} name="tab1" value={0}>Ghana Map</Tab>
	<Tab bind:group={tabSet} name="tab2" value={1}>Volta</Tab>
	<Tab bind:group={tabSet} name="tab3" value={2}>Northern</Tab>
	<Tab bind:group={tabSet} name="tab3" value={3}>Greater Accra</Tab>

	<!-- Tab Panels --->
	<svelte:fragment slot="panel">
		{#if tabSet === 0}
			<GeoMap />
		{:else if tabSet === 1}
			<VoltaGeoMap />
		{:else if tabSet === 2}
			<NorthernGeo />
		{:else if tabSet === 3}
			<GreaterAccra />
		{/if}
	</svelte:fragment>
</TabGroup>
			

<slot />
