<main>
	<slot />
</main>

<style global>
	main {
		position: relative;
		background-color: white;
		/* padding: 2em; */
		margin: 0 auto;
		box-sizing: border-box;
		overflow-y: hidden;
	}

	@import "tailwindcss/base";
	@import "tailwindcss/components";
	@import "tailwindcss/utilities";
	/* @tailwind forms; */
</style>
