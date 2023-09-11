<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function click() {
		dispatch('click');
	}

	export let label = null;
	export let icon = null;
	export let klass = 'btn variant-filled-primary';
</script>

<button type="button" class={klass} on:click|preventDefault|stopPropagation={click}>
	{#if icon}
		<span>
			<i class="fa-solid {icon}" />
		</span>
	{/if}
	{#if label}
		<span>{label}</span>
	{/if}
	<slot />
</button>
