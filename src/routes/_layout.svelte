<style global lang="postcss">
	main {
		position: relative;
		background-color: white;
		/* padding: 2em; */
		margin: 0 auto;
		box-sizing: border-box;
		overflow-y: hidden;
	}

	@tailwind base;
	@tailwind components;
  @tailwind utilities;
</style>

<main>
	<slot></slot>
</main>