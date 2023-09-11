<script>
	import { Autocomplete, popup } from '@skeletonlabs/skeleton';
	import { onMount } from 'svelte';
	import { v4 as uuid } from 'uuid';

	let search = '';
	let target = uuid();
	let popupSettings = {
		event: 'focus-click',
		target,
		// target: 'popupAutocomplete',
		placement: 'bottom'
	};

	function onSelect({ detail: { value: v, label } }) {
		value = v;
		search = label;
	}

	export let value = null;
	export let options = [];
	export let event = 'edit';
	export let label = 'label';
	export let placeholder = '';
	export let plain = false;

	let _options = [];

	$: _options = !plain ? options : options.map((v) => ({ label: v, value: v }));
	
	onMount(() => {
		search = _options.find((o) => o.value === value)?.label || '';
	});
</script>

<div class="relative label">
	<span>{label}</span>
	<input
		class="input autocomplete"
		type="search"
		name="autocomplete-search"
		autocomplete="off"
		disabled={event !== 'edit'}
		bind:value={search}
		{placeholder}
		use:popup={popupSettings}
	/>
	<div data-popup={target} class="card w-full p-4 z-10">
		<Autocomplete
			emptyState="No results found"
			limit={5}
			bind:input={search}
			options={_options}
			on:selection={onSelect}
		/>
	</div>
</div>
