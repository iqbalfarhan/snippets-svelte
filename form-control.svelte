<script lang="ts">
	import type { Snippet } from 'svelte';
	import * as Field from '../ui/field/index.ts';

	type Props = {
		children?: Snippet;
		label?: string;
		hint?: string;
		error?: string;
		required?: boolean;
	};
	let { children, label, hint, error, required }: Props = $props();
</script>

<Field.Field>
	{#if label}
		<Field.Label>
			{label}
			{#if required}
				<span class="text-destructive">*</span>
			{/if}
		</Field.Label>
	{/if}
	{@render children?.()}
	{#if hint}
		<Field.Description>{hint}</Field.Description>
	{/if}
	{#if error}
		<Field.Error>{error}</Field.Error>
	{/if}
</Field.Field>
