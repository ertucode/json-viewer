<script lang="ts">
	import JsonField from './JsonField.svelte';

	export let data: Record<string, any>;

	export function changeExpandAll(expanded: boolean) {
		jsonFields.forEach((field) => {
			field?.changeExpandNested(expanded);
		});
	}

	$: fields = Object.keys(data);
	$: jsonFields = [] as JsonField[];
</script>

<div class="container">
	{#each fields as field, idx}
		<JsonField bind:this={jsonFields[idx]} label={field} value={data[field]} />
	{/each}
</div>

<style>
	.container {
		font-family: 'monaco', 'Consolas', 'monospace';
		font-size: 1rem;
		font-weight: 600;

		--first-line-gap: 0.5em;
	}
</style>
