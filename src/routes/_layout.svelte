<main>
	<slot />
</main>

<style global>
	main {
		position: relative;
		background-color: white;
		/* padding: 2em; */
		box-sizing: border-box;
		overflow-y: hidden;
		height: 100vh;
	}

	@import "tailwindcss/base";
	@import "tailwindcss/components";
	@import "tailwindcss/utilities";
</style>
