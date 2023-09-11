<script>
	import { localStorageStore } from '@skeletonlabs/skeleton';
	import { goto } from '$app/navigation';
	import { v4 as uuid } from 'uuid';
	import Button from '$ui/Button.svelte';
	import Module from '$ui/Module.svelte';
	import Field from '$ui/Field.svelte';
	import Association from '$ui/Association.svelte';
	import { page } from '$app/stores';

	const parents = localStorageStore('models', []);
	const parent = $parents.find((s) => s.id === $page.params.id);
	const store = localStorageStore('fields', []);
	const store_associations = localStorageStore('associations', []);

	let disabled = true;
	let models = $store.filter((m) => m.modelId === parent?.id);
	let associations = $store_associations.filter((m) => m.modelId === parent?.id);

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
			$store = [...models, ...$store.filter((m) => m.modelId !== parent?.id)];
			$store_associations = [
				...associations,
				...$store_associations.filter((m) => m.modelId !== parent?.id)
			];
		}
	}

	function discard() {
		disabled = true;
		models = $store.filter((m) => m.modelId === parent?.id);
	}

	function addModel() {
		disabled = false;
		models = [
			...models,
			{
				id: uuid(),
				name: '',
				errors: [],
				modelId: parent?.id,
				defaultValue: '',
				values: [],
				type: '',
				required: false,
				associations: []
			}
		];
	}

	function destroy({ detail: m }) {
		models = models.filter((model) => model.id !== m.id);
	}

	function destroyAssociation({ detail: m }) {
		associations = associations.filter((model) => model.id !== m.id);
	}

	function addAssociation() {
		disabled = false;
		associations = [
			...associations,
			{
				id: uuid(),
				modelId: parent?.id,
				model: '',
				required: false,
				type: '',
				label: '',
			}
		];
	}
</script>

<Module title="Fields - {parent?.name}">
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
				on:click={() => goto(`/schema/${parent?.schemaId}`)}
			/>
			<Button icon="fa-pen" label="Edit" on:click={() => (disabled = false)} />
		{/if}
	</svelte:fragment>
	<svelte:fragment slot="main">
		{#each models as model}
			{#if !disabled}
				<Field label="Field name" placeholder="Input name" bind:model on:destroy={destroy} />
			{:else}
				<Button klass="card btn h-32" label="{model.name} - {model.type}" />
			{/if}
		{/each}
		<Button klass="card btn h-32" icon="fa-plus" label="New field" on:click={addModel} />
	</svelte:fragment>
	<svelte:fragment slot="main-two">
		<h3 class="h3 mb-4">Associations - {parent?.name}</h3>
		<div class="grid xl:grid-cols-4 md:grid-cols-3 sm:grid-cols-2 gap-4">
			{#each associations as model}
				{#if !disabled}
					<Association
						bind:schema={parent.schemaId}
						label="Field name"
						placeholder="Input name"
						bind:model
						on:destroy={destroyAssociation}
					/>
				{:else}
					<Button
						klass="card btn h-32"
						label="{model.model} [{model.required ? '*' : ''}] - {model.type}"
					/>
				{/if}
			{/each}
			<Button
				klass="card btn h-32"
				icon="fa-plus"
				label="New association"
				on:click={addAssociation}
			/>
		</div>
	</svelte:fragment>
</Module>
