<script>
	export let title = '';
</script>

<div class="flex flex-col p-4">
	<div class="flex flex-row justify-between items-center mb-4">
		<div>
			<h3 class="h3">{title}</h3>
		</div>
		<div>
			<slot name="actions" />
		</div>
	</div>
	<div class="grid xl:grid-cols-4 md:grid-cols-3 sm:grid-cols-2  gap-4 mb-8">
		<slot name="main" />
	</div>
	<div class="flex flex-col">
		<slot name="main-two" />
	</div>
</div>
