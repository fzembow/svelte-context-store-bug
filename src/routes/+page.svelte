<script lang="ts">
	import { derived, writable } from 'svelte/store';
	import ChildComponent from './ChildComponent.svelte';

	const numberStore = writable<number>(21);

	let calcCount = 0;
	const derivedStore = derived([numberStore], ([number]) => {
		calcCount += 1;
		console.log(`calculating the derived store, attempt ${calcCount}`);
		return number * 2;
	});
</script>

<ChildComponent store={derivedStore} />
<ChildComponent store={derivedStore} />
<ChildComponent store={derivedStore} />
<ChildComponent store={derivedStore} />
