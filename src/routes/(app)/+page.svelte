<script>
	import { saveAs } from 'file-saver';
	import { filter, localStorageStore } from '@skeletonlabs/skeleton';
	import { goto } from '$app/navigation';
	import { v4 as uuid } from 'uuid';
	import Button from '$ui/Button.svelte';
	import Module from '$ui/Module.svelte';
	import Card from '$ui/Card.svelte';

	const store = localStorageStore('schemas', []);
	const tables = localStorageStore('models', []);
	const fields = localStorageStore('fields', []);
	const associations = localStorageStore('associations', []);

	let disabled = true;
	let models = $store;

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
			$store = [...models];
		}
	}

	function discard() {
		disabled = true;
		models = [...$store];
	}

	function addModel() {
		disabled = false;
		models = [...models, { id: uuid(), name: '', errors: [] }];
	}

	function destroy({ detail: m }) {
		models = models.filter((model) => model.id !== m.id);
	}

	function detail(model) {
		console.log(model.id);
		goto(`/schema/${model.id}`);
	}

	function download(model) {
		// console.log('download');
		const schema = $store.find((m) => m.id === model.id);
		let models = $tables.filter((m) => m.schemaId === schema.id);

		let data = {
			$schema: 'https://json-schema-o8im9w3et-wchanchavac.vercel.app/backend-01.json',
			project: {
				name: schema.name,
				description: `This is a backend from ${schema.name}`
			},
			data: []
		};

		for (let i = 0; i < models.length; i++) {
			let item = {
				name: models[i].name,
				attributes: [],
				associations: [],
				singularLabel: models[i].singularLabel,
				pluralLabel: models[i].pluralLabel
			};

			let model = models[i];
			let properties = $fields.filter((f) => f.modelId === model.id);
			let assoc = $associations.filter((f) => f.modelId === model.id);

			for (let j = 0; j < properties.length; j++) {
				const element = properties[j];
				item.attributes.push({
					name: element.name,
					type: element.type,
					required: element.required,
					defaultValue: element.defaultValue,
					label: element.label,
					values: element.values
				});
			}

			for (let i = 0; i < assoc.length; i++) {
				const element = assoc[i];
				item.associations.push({
					model: element.model,
					type: element.type,
					required: element.required,
					label: element.label,
				});
			}

			data.data.push(item);
		}

		var file = new File([JSON.stringify(data)], 'project.json', {
			type: 'text/plain;charset=utf-8'
		});
		saveAs(file);
	}
</script>

<Module title="Schemas">
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
			<Button icon="fa-pen" label="Edit" on:click={() => (disabled = false)} />
		{/if}
	</svelte:fragment>
	<svelte:fragment slot="main">
		{#each models as model}
			{#if !disabled}
				<Card
					label="Schema name"
					placeholder="Input name"
					bind:model
					on:destroy={destroy}
				/>
			{:else}
				<Button klass="card btn h-32 cursor-pointer" on:click={() => detail(model)}>
					<Button
						klass="btn-icon !bg-transparent"
						icon="fa-download"
						on:click={() => download(model)}
					/>
					{model.name}
				</Button>
			{/if}
		{/each}

		<Button klass="card btn h-32" icon="fa-plus" label="New schema" on:click={addModel} />
	</svelte:fragment>
</Module>
