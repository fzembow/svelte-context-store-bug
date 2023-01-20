<script lang="ts">
	import { getContext } from 'svelte';
	import type { Readable } from 'svelte/store';

	export let id: number;

	console.log(`getting the context in component ${id}`);
	const store: Readable<number> = getContext('doubledStore');
</script>

<h1>The number is {$store}</h1>
