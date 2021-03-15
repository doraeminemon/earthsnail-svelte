<script>
	import VirtualList from "@sveltejs/svelte-virtual-list";
	import { onMount } from "svelte";

	let coffeeShops = [];

	onMount(async () => {
		await fetch(
			"https://doraeminemon.api.stdlib.com/gsheets-database-example/select/coffee-shops"
		)
			.then((r) => r.json())
			.then((data) => {
				coffeeShops = data;
			});
	});
</script>

<svelte:head>
	<title>nghiện là dở bạn ơi</title>
	<link rel="preconnect" href="https://fonts.gstatic.com" />
	<link
		href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;600&display=swap"
		rel="stylesheet"
	/>
</svelte:head>
<h1 class="p-2 title">Ún Gì ? Coffee ☕️</h1>
<VirtualList class="flex w-screen h-80" items={coffeeShops} let:item>
	<div
		class="flex max-w-md px-4 mx-auto overflow-hidden bg-white border-b-2 border-gray-400 md:max-w-2xl"
	>
		<div class="flex-row py-4 pr-2">
			<h1>{item.Name}</h1>
			<div class="flex pt-2 md:flex">
				<div class="flex-1 md:flex-shrink-0">
					<img
						class="object-cover h-48 rounded-sm md:w-48"
						src={item.photos_1}
						alt="coffee shop"
						style="min-width: 40%"
					/>
				</div>
				<div class="flex-1 px-4">
					<p>🕙: {item.opening_days}</p>
					<p>💰: {item.price_range}</p>
					<a href={item.location_link}>
						<p>📍: {item.location}</p>
					</a>
				</div>
			</div>
		</div>
	</div>
</VirtualList>
<div
	class="fixed w-full p-2 bg-white border-2 border-gray-600 search-bar"
	style="bottom:0px; left:0px;z-index:2"
>
	<input
		class="w-full h-8 px-4 text-base"
		type="text"
		placeholder="Tìm kiếm v tên hoặc tt khác P: 40k, L: Q3"
	/>
</div>

<style lang="postcss">
	:root {
		--oxford: "#011936";
	}

	h1.title {
		@apply text-lg text-center bg-oxford text-maize;
	}
	p,
	h1,
	input {
		font-family: "Space Grotesk", -apple-system, BlinkMacSystemFont, "Segoe UI",
			Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
			sans-serif;
		color: var(--oxford);
	}
	p {
		font-weight: 400;
	}
	h1 {
		font-weight: 600;
	}
</style>
