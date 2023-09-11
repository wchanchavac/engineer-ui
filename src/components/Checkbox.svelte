<script>
	export let value = false;
	export let label = 'checkbox';
</script>

<label class="flex items-center space-x-2">
	<input class="checkbox" type="checkbox" bind:checked={value} />
	<p>{label}</p>
</label>
