<script>
	import { localStorageStore } from '@skeletonlabs/skeleton';
	import { goto } from '$app/navigation';
	import { v4 as uuid } from 'uuid';
	import Button from '$ui/Button.svelte';
	import Input from '$ui/Input.svelte';
	import Module from '$ui/Module.svelte';
	import Card from '$ui/Card.svelte';
	import { page } from '$app/stores';

	const parents = localStorageStore('schemas', []);
	const parent = $parents.find((s) => s.id === $page.params.id);
	const store = localStorageStore('models', []);

	let disabled = true;
	let models = $store.filter((m) => m.schemaId === parent.id);

	async function save() {
		let errors = false;

		for (let i = 0; i < models.length; i++) {
			models[i].errors = [];
			if (models[i].name.trim() === '') {
				models[i].errors.push('Name is required');
				errors = true;
			}
		}

		if (!errors) {
			disabled = true;
			$store = [...models, ...$store.filter((m) => m.schemaId !== parent.id)];
		}
	}

	function discard() {
		disabled = true;
		models = $store.filter((m) => m.schemaId === parent.id);
	}

	function addModel() {
		disabled = false;
		models = [
			...models,
			{ id: uuid(), name: '', errors: [], schemaId: parent.id, singularLabel: '', pluralLabel: '' }
		];
	}

	function destroy({ detail: m }) {
		models = models.filter((model) => model.id !== m.id);
	}

	function detail(model) {
		goto(`/model/${model.id}`);
	}
</script>

<Module title="Modules - {parent?.name}">
	<svelte:fragment slot="actions">
		{#if !disabled}
			<Button
				icon="fa-xmark"
				klass="btn variant-filled-secondary"
				label="Cancel"
				on:click={discard}
			/>
			<Button icon="fa-floppy-disk" label="Save" on:click={save} />
		{/if}
		{#if disabled}
			<Button
				icon="fa-arrow-left"
				klass="btn variant-filled-secondary"
				label="Back"
				on:click={() => goto('/')}
			/>
			<Button icon="fa-pen" label="Edit" on:click={() => (disabled = false)} />
		{/if}
	</svelte:fragment>
	<svelte:fragment slot="main">
		{#each models as model}
			{#if !disabled}
				<Card label="Model name" placeholder="Input name" bind:model on:destroy={destroy}>
					<svelte:fragment>
						<Input
							label="Label singular"
							placeholder="Input label singular"
							bind:value={model.singularLabel}
						/>
						<Input
							label="Label plural"
							placeholder="Input label plural"
							bind:value={model.pluralLabel}
						/>
					</svelte:fragment>
				</Card>
			{:else}
				<Button klass="card btn h-32" label={model.name} on:click={() => detail(model)} />
			{/if}
		{/each}

		<Button klass="card btn h-32" icon="fa-plus" label="New model" on:click={addModel} />
	</svelte:fragment>
</Module>
