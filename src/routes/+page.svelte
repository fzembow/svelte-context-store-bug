<script lang="ts">
	import { setContext } from 'svelte';
	import { derived, writable } from 'svelte/store';
	import ChildComponent from './ChildComponent.svelte';

	const numberStore = writable<number>(21);

	console.log('setting the context in the parent');

	let calcCount = 0;

	setContext(
		'doubledStore',
		derived([numberStore], ([number]) => {
			calcCount += 1;
			console.log(`calculating the derived store, attempt ${calcCount}`);
			return number * 2;
		})
	);
</script>

<ChildComponent id={1} />
<ChildComponent id={2} />
<ChildComponent id={3} />
<ChildComponent id={4} />
