<script>
	import { AppShell, Modal } from '@skeletonlabs/skeleton';
	import Header from './Header.svelte';

	import { computePosition, autoUpdate, offset, shift, flip, arrow } from '@floating-ui/dom';
	import { storePopup, initializeStores } from '@skeletonlabs/skeleton';

	initializeStores();

	storePopup.set({ computePosition, autoUpdate, offset, shift, flip, arrow });
</script>

<Modal />
<AppShell regionPage="relative" slotPageHeader="sticky top-0 z-10">
	<svelte:fragment slot="header"><Header /></svelte:fragment>
	<!-- <svelte:fragment slot="sidebarLeft"><Sidebar /></svelte:fragment> -->
	<!-- (sidebarRight) -->
	<!-- (pageHeader) -->
	<!-- <div class="divide-y divide-current"></div> -->
	<!-- <svelte:fragment slot="pageHeader"> -->
	<!-- <Header /> -->
	<!-- </svelte:fragment> -->
	<!-- <main> -->
		<slot />
	<!-- </main> -->
	<!-- Router Slot -->
	<!-- (pageFooter) -->
	<!-- (footer) -->
</AppShell>
