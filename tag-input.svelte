<script lang="ts">
	import * as InputGroup from '#lib/components/ui/input-group/index.ts';
	import XIcon from '@lucide/svelte/icons/x';

	let {
		value = $bindable<string[]>([]),
		placeholder = 'Tambah tag...',
		disabled = false,
		id
	}: {
		value?: string[];
		placeholder?: string;
		disabled?: boolean;
		id?: string;
	} = $props();

	let input = $state('');

	function addTag(raw: string) {
		const tag = raw.trim();
		if (!tag || value.includes(tag)) return;
		value = [...value, tag];
	}

	function removeTag(index: number) {
		value = value.filter((_, i) => i !== index);
	}

	function commit() {
		addTag(input);
		input = '';
	}

	function onkeydown(e: KeyboardEvent) {
		if (e.key === 'Enter' || e.key === ',') {
			e.preventDefault();
			commit();
		} else if (e.key === 'Backspace' && input === '' && value.length > 0) {
			removeTag(value.length - 1);
		}
	}

	function onpaste(e: ClipboardEvent) {
		const text = e.clipboardData?.getData('text');
		if (!text || !/[,\n]/.test(text)) return;
		e.preventDefault();
		text.split(/[,\n]/).forEach(addTag);
		input = '';
	}
</script>

<InputGroup.Root>
	{#if value.length > 0}
		<InputGroup.Addon align="block-start" class="flex-wrap gap-1">
			{#each value as tag, i (tag)}
				<InputGroup.Button
					variant="secondary"
					size="xs"
					{disabled}
					aria-label="Hapus {tag}"
					onclick={() => removeTag(i)}
				>
					{tag}
					<XIcon />
				</InputGroup.Button>
			{/each}
		</InputGroup.Addon>
	{/if}

	<InputGroup.Input
		{id}
		{placeholder}
		{disabled}
		{onkeydown}
		{onpaste}
		onblur={commit}
		autocomplete="off"
		bind:value={input}
	/>
</InputGroup.Root>
