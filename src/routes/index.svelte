<script>
	import { onMount } from 'svelte';
	import { Map } from '@beyonk/svelte-mapbox'
	const lng = 106.71
	const lat = 10.76
	const zoom = 11
	let searchText
	let mapComponent
	onMount(() => {
		mapComponent.setCenter([lng, lat], zoom)
		mapComponent.flyTo({ center: [lng, lat] })
	})
</script>

<style>
	:global(.mapboxgl-map) {
		height: 100vh !important;
	}
	.search-bar {
		left: 25px;
		top: 25px;
		z-index: 2;
		width: calc(100vw - 50px)
	}
</style>

<svelte:head>
	<title>Sapper project template</title>
</svelte:head>
<div class="fixed search-bar p-2 bg-white rounded flex">
	<input class="w-full" type="text" bind:value={searchText} placeholder="Tìm kiếm quận huyện" />
</div>
<Map
	accessToken='pk.eyJ1IjoiZG9yYWVtaW5lbW9uIiwiYSI6ImNrbGpvMXBzaTFtc3UzMWxkNzZ4OGZvNDEifQ.6nhLO4DNHok5J17TugRB2A'
	bind:this={mapComponent}
	on:recentre={e => console.log(e.detail.center.lat, e.detail.center.lng) }
	options={{ scrollZoom: false }}
/>
