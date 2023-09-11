<script>

	import { AppBar } from '@skeletonlabs/skeleton';
	import Button from '../../components/Button.svelte';
</script>


<AppBar gridColumns="grid-cols-3" slotDefault="place-self-center" slotTrail="place-content-end">
	<svelte:fragment slot="lead">
		<h2 class="h2 font-medium">sequeilize-ui</h2>
	</svelte:fragment>
	<!-- (title) -->
	<svelte:fragment slot="trail">
		<Button icon="fa-brands fa-github fa-xl" klass="btn-icon hover:variant-soft-primary"></Button>
		<Button icon="fa-brands fa-linkedin fa-xl" klass="btn-icon hover:variant-soft-primary"></Button>
	</svelte:fragment>
</AppBar>

