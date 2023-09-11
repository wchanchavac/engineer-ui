<script>
	import { InputChip } from '@skeletonlabs/skeleton';

	export let value = [];
    export let label = 'label';
</script>

<div class="label">
    <span>{label}</span>
    <InputChip class="chip-text-sm" minlength={1} bind:value={value} />
</div>
