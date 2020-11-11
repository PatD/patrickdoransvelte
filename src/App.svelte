<script>
import { onMount } from 'svelte';
let wordpressUrl = 'https://patrickdoran.com/headless/wp-json/wp/v2/posts';
let items = [];

function filterResults(loadedResults){
	// 
	console.log(loadedResults)

	loadedResults.forEach((element, index, array) => {
		
		console.log(element.content.rendered); // strip html here
		console.log(element.title.rendered); // 100, 200, 300
		console.log(element.excerpt.rendered); // strip html here
		console.log(element.date); // 100, 200, 300
		console.log(element.id); // 100, 200, 300
		
		console.log(index); // 0, 1, 2
		console.log(array); // same myArray object 3 times
	})
}


onMount(async () => {
	const res = await fetch(wordpressUrl);
	// loop results, strip markup

	// items = await res.json()

	let loadedResults = await res.json();
	filterResults(loadedResults)

});
</script>

{#each items as listitem}
<li>
	{listitem.content.rendered} <a href={listitem.excerpt.rendered}>{listitem.title.rendered}</a> {listitem.date}
</li>
{:else}
<p>Loading data from headless WordPress into a Svelte component...</p>
{/each}
