<script lang="ts">
	import type { Readable } from 'svelte/store';
	export let store: Readable<number>;
</script>

<h1>The number is {$store}</h1>
