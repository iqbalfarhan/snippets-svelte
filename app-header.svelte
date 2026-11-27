<script lang="ts">
	import { ChevronLeft } from '@lucide/svelte';
	import { Button } from '../ui/button';
	import { tv, type VariantProps } from 'tailwind-variants';
	import type { Snippet } from 'svelte';
	import { goto } from '$app/navigation';
	import { cn } from 'cn';

	const appHeader = tv({
		slots: {
			container: 'grid md:flex items-center justify-center gap-4 p-8',
			inner: 'grid w-full',
			title: 'uppercase',
			lead: 'text-muted-foreground'
		},
		variants: {
			size: {
				sm: {
					inner: '',
					title: 'text-xl font-bold',
					lead: 'text-sm'
				},
				default: {
					inner: ' space-y-1.5',
					title: 'text-3xl font-bold',
					lead: ''
				},
				big: {
					inner: ' space-y-2',
					title: 'text-4xl font-bold',
					lead: ''
				}
			},
			align: {
				default: 'text-left',
				center: 'text-center',
				right: 'text-right'
			}
		},
		defaultVariants: {
			align: 'default',
			size: 'default'
		}
	});

	type Props = VariantProps<typeof appHeader> & {
		title?: string;
		lead?: string;
		back?: boolean;
		backUrl?: string;
		class?: string;
		children?: Snippet;
	};

	let {
		size,
		align,
		title,
		lead,
		back = false,
		backUrl,
		class: className,
		children
	}: Props = $props();

	let style = $derived(appHeader({ size, align }));
</script>

<div class={cn(style.container(), className)}>
	{#if back}
		<Button
			onclick={() => (backUrl ? goto(backUrl) : history.back())}
			variant="secondary"
			size="icon"
		>
			<ChevronLeft />
		</Button>
	{/if}
	<div class={style.inner()}>
		<h2 class={style.title()}>{title}</h2>
		<p class={style.lead()}>{lead}</p>
	</div>
	{#if children}
		<div class="flex items-center gap-1">
			{@render children?.()}
		</div>
	{/if}
</div>
