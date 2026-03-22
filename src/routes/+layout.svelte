<script lang="ts">
	import GloomIcon from "$lib/assets/gloom_icon.png"

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={GloomIcon} />
	<title>Gloom</title>
</svelte:head>

<style>
	/* Inter */
	@import url(https://fonts.googleapis.com/css?family=Inter:100,200,300,regular,500,600,700,800,900,100italic,200italic,300italic,italic,500italic,600italic,700italic,800italic,900italic);
	
	/* Nunito Sans */
	@import url(https://fonts.googleapis.com/css?family=Nunito+Sans:200,300,regular,500,600,700,800,900,200italic,300italic,italic,500italic,600italic,700italic,800italic,900italic);

	:global(*) {
		margin: 0;
        padding: 0;
        box-sizing: border-box;
	}

	:global(:root) {
        --bg-primary: #121212;
        --bg-secondary: #181818;
        --bg-third: #242424;
        --input-bg: #141414;
        --secondary-text-color: #effff980;
        --muted-text-color: #ffffff80;
        --input-label-color: #ffffffbf;
        --accent-color: #79FFAD;
        --accent-color-dark: #45A469;
        --primary-font: "Inter";
        --secondary-font: "Nunito Sans";
        --border-radius: 8px;
	}
</style>

{@render children()}
