<script lang="ts">
	import { cn } from 'cn';
	import type { Snippet } from 'svelte';
	import { tv, type VariantProps } from 'tailwind-variants';

	const grid = tv({
		base: 'grid',
		variants: {
			cols: {
				'1': 'grid-cols-1 xl:grid-cols-1',
				'2': 'grid-cols-1 xl:grid-cols-2',
				'3': 'grid-cols-1 xl:grid-cols-3',
				'4': 'grid-cols-1 xl:grid-cols-4',
				'5': 'grid-cols-1 xl:grid-cols-5',
				'6': 'grid-cols-1 xl:grid-cols-6'
			},
			gap: {
				'0': 'gap-0',
				'2': 'gap-2',
				'4': 'gap-4',
				'6': 'gap-6',
				'8': 'gap-8'
			}
		},
		defaultVariants: {
			cols: '3',
			gap: '0'
		}
	});

	type Props = VariantProps<typeof grid> & {
		children?: Snippet;
		class?: string;
	};

	let { children, class: className, cols, gap }: Props = $props();

	let style = $derived(grid({ cols, gap }));
</script>

<div class={cn(style, className)}>
	{@render children?.()}
</div>
