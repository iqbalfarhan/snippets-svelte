<script lang="ts">
	import Button from '../ui/button/button.svelte';
	import { Check, type LucideIcon } from '@lucide/svelte';
	import Spinner from '../ui/spinner/spinner.svelte';
	import type { ComponentProps } from 'svelte';

	type Props = ComponentProps<typeof Button> & {
		loading?: boolean;
		label?: string;
		icon?: LucideIcon;
	};

	let { loading = false, label = 'Submit form', icon: Icon = Check, ...rest }: Props = $props();
</script>

<Button disabled={loading} {...rest}>
	{#if loading}
		<Spinner />
	{:else}
		<Icon />
	{/if}

	{label}
</Button>
