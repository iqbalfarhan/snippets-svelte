<script lang="ts">
	import type { ComponentProps } from 'svelte';
	import * as InputGroup from '#lib/components/ui/input-group/index.ts';

	type Props = Omit<ComponentProps<typeof InputGroup.Textarea>, 'value' | 'maxlength'> & {
		limit?: number;
		value?: string;
	};

	const uid = $props.id();

	let {
		limit = 255,
		value = $bindable(''),
		id = uid,
		'aria-describedby': describedby,
		...rest
	}: Props = $props();

	const counterId = $derived(`${id}-counter`);
	const length = $derived(value?.length ?? 0);
	const remaining = $derived(limit - length);
	const atLimit = $derived(remaining <= 0);
	const nearLimit = $derived(remaining <= Math.ceil(limit * 0.1));
</script>

<InputGroup.Root>
	<InputGroup.Textarea
		{...rest}
		{id}
		maxlength={limit}
		aria-describedby={[describedby, counterId].filter(Boolean).join(' ')}
		bind:value
	/>
	<InputGroup.Addon align="block-end">
		<span
			id={counterId}
			class={[
				'ml-auto text-xs tabular-nums',
				atLimit ? 'text-destructive' : nearLimit ? 'text-amber-600' : 'text-muted-foreground'
			]}
		>
			{length}/{limit}
		</span>
		<span class="sr-only" aria-live="polite">
			{atLimit ? 'Batas karakter tercapai' : ''}
		</span>
	</InputGroup.Addon>
</InputGroup.Root>
