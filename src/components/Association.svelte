<script>
	import { createEventDispatcher } from 'svelte';
	import Input from '$ui/Input.svelte';
	import Autocomplete from '$ui/Autocomplete.svelte';
	import Checkbox from '$ui/Checkbox.svelte';
	import Button from '$ui/Button.svelte';
	import { localStorageStore } from '@skeletonlabs/skeleton';
	const dispatch = createEventDispatcher();
	const parents = localStorageStore('models', []);

	function destroy(model) {
		dispatch('destroy', model);
	}

	let types = [{
		value:'belongsTo',
		label:'BelongsTo'
	},{
		value:'hasMany',
		label:'HasMany'
	},{
		value:'belongsToMany',
		label:'BelongsToMany'
	}]

	export let model = { id: null, model, type: '', required: false, label: '' };
	export let klass = '';
	export let schema = null;

	let models = $parents.filter((m) => m.id !== schema).map((m) => ({ value: m.name, label: m.name }));	

</script>

<div class="card p-4 relative flex flex-col gap-4 {klass}">
	<Button
		icon="fa-trash"
		klass="btn-icon !bg-transparent absolute top-0 right-0 z-10"
		on:click={() => destroy(model)}
	/>
	<Autocomplete label="Model" placeholder="Select model" bind:value={model.model} bind:options={models}/>
	<Input label="Label" placeholder="Input label"  bind:value={model.label} />
	<Autocomplete label="Type" placeholder="Select type" bind:value={model.type} options={types}/>
	<Checkbox label="Required" bind:value={model.required}/>
</div>
