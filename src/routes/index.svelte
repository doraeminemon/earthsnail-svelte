<script>
	import { onMount } from 'svelte';
	import { Map } from '@beyonk/svelte-mapbox'
	const lng = 106.71
	const lat = 10.76
	const zoom = 11
	let searchText
	let mapComponent
	onMount(() => {
		//

		// create a custom style layer to implement the WebGL content
		var highlightLayer = {
				id: 'highlight',
				type: 'custom',

				// method called when the layer is added to the map
				// https://docs.mapbox.com/mapbox-gl-js/api/#styleimageinterface#onadd
				onAdd: function (map, gl) {
						// create GLSL source for vertex shader
						var vertexSource =
								'' +
								'uniform mat4 u_matrix;' +
								'attribute vec2 a_pos;' +
								'void main() {' +
								'    gl_Position = u_matrix * vec4(a_pos, 0.0, 1.0);' +
								'}';

						// create GLSL source for fragment shader
						var fragmentSource =
								'' +
								'void main() {' +
								'    gl_FragColor = vec4(1.0, 0.0, 0.0, 0.5);' +
								'}';

						// create a vertex shader
						var vertexShader = gl.createShader(gl.VERTEX_SHADER);
						gl.shaderSource(vertexShader, vertexSource);
						gl.compileShader(vertexShader);

						// create a fragment shader
						var fragmentShader = gl.createShader(gl.FRAGMENT_SHADER);
						gl.shaderSource(fragmentShader, fragmentSource);
						gl.compileShader(fragmentShader);

						// link the two shaders into a WebGL program
						this.program = gl.createProgram();
						gl.attachShader(this.program, vertexShader);
						gl.attachShader(this.program, fragmentShader);
						gl.linkProgram(this.program);

						this.aPos = gl.getAttribLocation(this.program, 'a_pos');

						// define vertices of the triangle to be rendered in the custom style layer
						var helsinki = mapboxgl.MercatorCoordinate.fromLngLat({
								lng: lng+0.1,
								lat: lat+0.1,
						});
						var berlin = mapboxgl.MercatorCoordinate.fromLngLat({
								lng: lng-0.1,
								lat: lat-0.1,
						});
						var kyiv = mapboxgl.MercatorCoordinate.fromLngLat({
								lng: lng+0.5,
								lat: lat-0.3,
						});

						// create and initialize a WebGLBuffer to store vertex and color data
						this.buffer = gl.createBuffer();
						gl.bindBuffer(gl.ARRAY_BUFFER, this.buffer);
						gl.bufferData(
								gl.ARRAY_BUFFER,
								new Float32Array([
										helsinki.x,
										helsinki.y,
										berlin.x,
										berlin.y,
										kyiv.x,
										kyiv.y
								]),
								gl.STATIC_DRAW
						);
				},

				// method fired on each animation frame
				// https://docs.mapbox.com/mapbox-gl-js/api/#map.event:render
				render: function (gl, matrix) {
						gl.useProgram(this.program);
						gl.uniformMatrix4fv(
								gl.getUniformLocation(this.program, 'u_matrix'),
								false,
								matrix
						);
						gl.bindBuffer(gl.ARRAY_BUFFER, this.buffer);
						gl.enableVertexAttribArray(this.aPos);
						gl.vertexAttribPointer(this.aPos, 2, gl.FLOAT, false, 0, 0);
						gl.enable(gl.BLEND);
						gl.blendFunc(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA);
						gl.drawArrays(gl.TRIANGLE_STRIP, 0, 3);
				}
		};

			// add the custom style layer to the map
		console.log({ item: mapComponent })
		mapComponent.getMap().on('load', function () {
				mapComponent.addLayer(highlightLayer, 'building');
		});
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
		bottom: 100px;
		z-index: 2;
		width: calc(100vw - 50px)
	}
</style>

<svelte:head>
	<title>Thông tin địa ốc</title>
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
