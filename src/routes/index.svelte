<script>
	import { onMount } from 'svelte';
	import { Map } from '@beyonk/svelte-mapbox'
	const lng = 106.71
	const lat = 10.76
	const zoom = 13
	let mapComponent
	onMount(() => {
		console.log('component')
		mapComponent.setCenter([lng, lat], zoom)
		mapComponent.flyTo({ center: [lng, lat] })
	})
</script>

<style>
	:global(.mapboxgl-map) {
		height: 500px !important;
	}
</style>

<svelte:head>
	<title>Sapper project template</title>
</svelte:head>
<Map
	accessToken='pk.eyJ1IjoiZG9yYWVtaW5lbW9uIiwiYSI6ImNrbGpvMXBzaTFtc3UzMWxkNzZ4OGZvNDEifQ.6nhLO4DNHok5J17TugRB2A'
	bind:this={mapComponent}
	on:recentre={e => console.log(e.detail.center.lat, e.detail.center.lng) }
	options={{ scrollZoom: false }}
/>
