<script>

	import { AppBar } from '@skeletonlabs/skeleton';
	import Button from '../../components/Button.svelte';
</script>


<AppBar gridColumns="grid-cols-3" slotDefault="place-self-center" slotTrail="place-content-end">
	<svelte:fragment slot="lead">
		<h2 class="h2 font-medium">engineer-ui</h2>
	</svelte:fragment>
	<!-- (title) -->
	<svelte:fragment slot="trail">
		<a href="https://github.com/wchanchavac" target="_blank" rel="noopener noreferrer" class="btn-icon hover:variant-soft-primary">
			<i class="fa-brands fa-github fa-xl"></i>
		</a>
		<a href="https://www.linkedin.com/in/wchanchavac/" target="_blank" rel="noopener noreferrer" class="btn-icon hover:variant-soft-primary">
			<i class="fa-brands fa-linkedin fa-xl"></i>
		</a>
	</svelte:fragment>
</AppBar>

