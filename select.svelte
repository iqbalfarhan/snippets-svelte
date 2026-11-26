<script lang="ts" module>
	export type SelectOption = { value: string; label: string };
</script>

<script lang="ts">
	import * as Select from '#lib/components/ui/select/index.ts';

	type Props = {
		value?: string;
		options?: string[] | SelectOption[];
		placeholder?: string;
		disabled?: boolean;
		name?: string;
		class?: string;
		onValueChange?: (value: string) => void;
	};

	let {
		value = $bindable(''),
		options = [],
		placeholder = 'Pilih',
		disabled = false,
		name,
		class: className,
		onValueChange
	}: Props = $props();

	const normalized = $derived<SelectOption[]>(
		(options as (string | SelectOption)[]).map((opt) =>
			typeof opt === 'string' ? { value: opt, label: opt } : opt
		)
	);

	const selectedLabel = $derived(normalized.find((opt) => opt.value === value)?.label);
</script>

<Select.Root type="single" bind:value {name} {disabled} {onValueChange}>
	<Select.Trigger class={className}>
		{selectedLabel ?? placeholder}
	</Select.Trigger>
	<Select.Content>
		{#each normalized as opt (opt.value)}
			<Select.Item value={opt.value} label={opt.label}>
				{opt.label}
			</Select.Item>
		{/each}
	</Select.Content>
</Select.Root>
