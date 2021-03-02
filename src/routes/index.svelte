<script>
	import { onMount } from 'svelte';
	let L
	import MapToolbar from './MapToolbar.svelte'
	const lng = 106.71
	const lat = 10.76
	const zoom = 13
	let mapElement
	let searchText
	let map
	const initialView = [lat,lng]
	function createMap(container) {
	  let m = L.map(container, {preferCanvas: true }).setView(initialView, zoom);
    L.tileLayer(
	    'https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png',
	    {
	      attribution: `&copy;<a href="https://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>,
	        &copy;<a href="https://carto.com/attributions" target="_blank">CARTO</a>`,
	      subdomains: 'abcd',
	    }
	  ).addTo(m);

    return m;
	}

	let toolbar = {}
	let toolbarComponent

	function resizeMap() {
	  if(map) { map.invalidateSize(); }
  }

  async function mapAction(container) {
		L = await import('leaflet')
		toolbar = L.control({ position: 'topright' })
		toolbar.onAdd = (map) => {
		let div = L.DomUtil.create('div');
		toolbarComponent = new MapToolbar({
			target: div,
			props: {}
		});

		toolbarComponent.$on('click-eye', ({ detail }) => eye = detail);
		toolbarComponent.$on('click-lines', ({ detail }) => lines = detail);
		toolbarComponent.$on('click-reset', () => {
			map.setView(initialView, 5, { animate: true })
		})

		return div;
	}

	toolbar.onRemove = () => {
		if(toolbarComponent) {
			toolbarComponent.$destroy();
			toolbarComponent = null;
		}
	};

    map = createMap(container);
		toolbar.addTo(map);

		// markerLayers = L.layerGroup()
 		// for(let location of markerLocations) {
 		// 	let m = createMarker(location);
		// 	markerLayers.addLayer(m);
 		// }

		// lineLayers = createLines();

		// markerLayers.addTo(map);
		// lineLayers.addTo(map);
    return {
       destroy: () => {
				 toolbar.remove();
				 map.remove();
				 map = null;
			 }
    };
	}

	onMount(async () => {
		console.log({ mapElement })
		await mapAction(mapElement)
	})

</script>
<svelte:window on:resize={resizeMap} />
<style>
	.map :global(.marker-text) {
		width:100%;
		text-align:center;
		font-weight:600;
		background-color:#444;
		color:#EEE;
		border-radius:0.5rem;
	}
	.map :global(.map-marker) {
		width:30px;
		transform:translateX(-50%) translateY(-25%);
	}
</style>

<svelte:head>
	<title>Thông tin địa ốc</title>
	<link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
		 integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
		 crossorigin=""/>
</svelte:head>
<div class="fixed search-bar p-2 bg-white rounded flex">
	<input class="w-full" type="text" bind:value={searchText} placeholder="Tìm kiếm quận huyện" />
</div>
<div class="map" style="width:100vw; height:100vh" bind:this={mapElement}/>
