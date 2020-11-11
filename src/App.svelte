<script>
import { onMount } from 'svelte';
let wordpressUrl = 'https://patrickdoran.com/headless/wp-json/wp/v2/posts';
let items = [];

onMount(async () => {
	const res = await fetch(wordpressUrl);
	items = await res.json();
});
</script>

{#each items as listitem}
<li>
	{@html listitem.content.rendered}
</li>
{:else}
<p>Loading data from headless WordPress into a Svelte component...</p>
{/each}
