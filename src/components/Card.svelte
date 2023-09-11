<script>
	import { createEventDispatcher } from 'svelte';
	import Input from '$ui/Input.svelte';
	import Button from '$ui/Button.svelte';
	const dispatch = createEventDispatcher();

	function destroy(model) {
		dispatch('destroy', model);
	}

	export let model = { id: null, name: '', errors: [] };
	export let placeholder = '';
	export let label = '';
	export let klass = '';
</script>

<div class="card p-4 relative flex flex-col gap-4 {klass}">
	<Button
		icon="fa-trash"
		klass="btn-icon !bg-transparent absolute top-0 right-0"
		on:click={() => destroy(model)}
	/>
	<Input {label} {placeholder} bind:errors={model.errors} bind:value={model.name} />
	<slot />
</div>
