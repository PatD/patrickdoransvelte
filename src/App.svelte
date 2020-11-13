<script>
import { onMount } from 'svelte';
let wordPressGraphQL = 'https://patrickdoran.com/headless/graphql'
let items = [];
let filteredItems = [];



onMount(async () => {
// Queries Headless WordPress site configured for GraphQL
	const res = await fetch(wordPressGraphQL, {
		method: 'POST',
		headers: {
			'Content-Type': 'application/json',
		},
		body: JSON.stringify({
			query: `
				{
				posts {
					edges {
					node {
						excerpt
						date
						postId
						title
						content
					}
					}
				}
				}
			`,
		}),
	})

	let graphQLresults = await res.json()

	items = graphQLresults.data.posts.edges;

	// Remove Wordpress markup from GraphQL 
	let mappedItems = items.map(x => {
		x.node.excerpt = x.node.excerpt.replace( /(<([^>]+)>)/ig, '');
		x.node.content = x.node.content.replace( /(<([^>]+)>)/ig, '');
		// console.log(x.node)
		filteredItems.push(x.node)
	})

});
</script>

{#each items as listitem}
<li>
	{listitem.node.content} <a href={listitem.node.excerpt} target='_blank' rel='noreferrer'>{listitem.node.title}</a> {listitem.node.date}
</li>
{:else}
<li>Loading GraphQL data from a headless WordPress site into a Svelte component...</li>
{/each}
