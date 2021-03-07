<script>
	import VirtualList from "@sveltejs/svelte-virtual-list";
	import coffeeShops from "../coffeesaigon.js";
</script>

<svelte:head>
	<title>Cà phê ở đâu ?</title>
	<link
		rel="stylesheet"
		href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
		integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
		crossorigin=""
	/>
	<link rel="preconnect" href="https://fonts.gstatic.com" />
	<link
		href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;600&display=swap"
		rel="stylesheet"
	/>
</svelte:head>
<VirtualList class="flex w-screen h-80" items={coffeeShops} let:item>
	<div
		class="flex max-w-md mx-auto overflow-hidden bg-white border-b-2 border-gray-400 md:max-w-2xl"
	>
		<div class="flex-row py-4 pr-2">
			<h1>{item.contact.instagram}</h1>
			<div class="flex pt-2 md:flex">
				<div class="flex-1 md:flex-shrink-0">
					<img
						class="object-cover h-48 rounded-sm md:w-48"
						src={item.photos[0]}
						alt="coffee shop"
						style="min-width: 40%"
					/>
				</div>
				<div class="flex-1 px-4">
					<p>🆔: {item.photo_credits}</p>
					<p>🕙: {item.opening_days}</p>
					<p>💰: {item.price_range}</p>
					<p>📍: {item.location}</p>
				</div>
			</div>
		</div>
	</div>
</VirtualList>
<div
	class="fixed w-full p-2 bg-white border-t-2 border-gray-400 search-bar"
	style="bottom:0px; left:0px;z-index:2"
>
	<input
		class="w-full h-8 pl-2 border-2 border-gray-600"
		type="text"
		placeholder="Tìm kiếm theo chữ viết tắt.V.D: P: 40k, L: Q3"
	/>
</div>

<style>
	p {
		font-family: "Space Grotesk", -apple-system, BlinkMacSystemFont, "Segoe UI",
			Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
			sans-serif;
		font-weight: 400;
	}
	h1 {
		font-family: "Space Grotesk", -apple-system, BlinkMacSystemFont, "Segoe UI",
			Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
			sans-serif;
		font-weight: 600;
	}
</style>
