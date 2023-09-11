<script>
	import Card from '$ui/Card.svelte';
	import Autocomplete from '$ui/Autocomplete.svelte';
	import InputChips from '$ui/InputChips.svelte';
	import Checkbox from '$ui/Checkbox.svelte';
	import Input from '$ui/Input.svelte';
	import { createEventDispatcher } from 'svelte';

	const options = [
		{ value: 'hexadecimal', label: 'Hexadecimal' },
		{ value: 'string', label: 'String' },
		{ value: 'float', label: 'Float' },
		{ value: 'boolean', label: 'Boolean' },
		{ value: 'integer', label: 'Integer' },
		{ value: 'uuid', label: 'UUID' },
		{ value: 'email', label: 'EmailAddress' },
		{ value: 'json', label: 'JSON' },
		{ value: 'url', label: 'URL' },
		{ value: 'enum', label: 'Enum' },
		{ value: 'date', label: 'DateTime' },
		{ value: 'dateOnly', label: 'LocalDate' }
	].sort((a, b) => a.label.localeCompare(b.label));

	const dispatch = createEventDispatcher();

	function destroy({ detail: model}) {
		dispatch('destroy', model);
	}

	export let label = '';
	export let placeholder = '';
	export let klass = '';
	export let model = {
		name: '',
		errors: [],
		defaultValue: '',
		values: [],
		type: '',
		required: false
	};
</script>

<Card {label} {placeholder} {klass} bind:model on:destroy={destroy}>
	<svelte:fragment>
		<Input label="Label" placeholder="Input label" bind:value={model.label} />
		<Autocomplete label="Type" placeholder="Select type" {options} bind:value={model.type} />
		<Checkbox label="Required" bind:value={model.required} />
		{#if model.type === 'enum'}
			<InputChips label="Values" placeholder="Input values" bind:value={model.values} />
			<Autocomplete
				label="Default value"
				placeholder="Select default value"
				plain={true}
				bind:options={model.values}
				bind:value={model.defaultValue}
			/>
		{:else}
			<Input
				label="Default value"
				placeholder="Input default value"
				bind:value={model.defaultValue}
			/>
		{/if}
	</svelte:fragment>
</Card>
