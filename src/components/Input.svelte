<script>
	export let value = null;
	export let label = 'label';
	export let disabled = false;
	// export let type = 'text';
	export let placeholder = '';
	export let errors = [];
</script>

<label class="label">
	{#if label}
		<span>{label}</span>
	{/if}
	<input
		class="input"
		type="text"
		{placeholder}
		bind:value
		name="model-name"
		autocomplete="off"
		{disabled}
	/>
	{#if errors && errors.length > 0}
		{#each errors as error}
			<span class="text-xs text-red-500">{error}</span>
		{/each}
	{/if}
</label>
