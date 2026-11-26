<script lang="ts" module>
	const LOWER = 'abcdefghijkmnopqrstuvwxyz'; // tanpa 'l'
	const UPPER = 'ABCDEFGHJKLMNPQRSTUVWXYZ'; // tanpa 'I', 'O'
	const DIGITS = '23456789'; // tanpa '0', '1'
	const SYMBOLS = '!@#$%^&*-_+=?';

	// Angka acak tanpa modulo bias (rejection sampling)
	const randomInt = (max: number) => {
		const limit = Math.floor(0x100000000 / max) * max;
		const buf = new Uint32Array(1);
		do {
			crypto.getRandomValues(buf);
		} while (buf[0] >= limit);
		return buf[0] % max;
	};

	const pick = (chars: string) => chars[randomInt(chars.length)];

	export const generatePassword = (length = 8) => {
		const groups = [LOWER, UPPER, DIGITS, SYMBOLS];
		const all = groups.join('');

		// Pastikan minimal 1 karakter dari tiap grup
		const chars = groups.map(pick);
		while (chars.length < Math.max(length, groups.length)) {
			chars.push(pick(all));
		}

		// Fisher-Yates shuffle
		for (let i = chars.length - 1; i > 0; i--) {
			const j = randomInt(i + 1);
			[chars[i], chars[j]] = [chars[j], chars[i]];
		}

		return chars.join('');
	};
</script>

<script lang="ts">
	import { Eye, EyeOff, Lock, Shuffle } from '@lucide/svelte';
	import * as InputGroup from '#lib/components/ui/input-group/index.ts';
	import type { ComponentProps } from 'svelte';

	type Props = Omit<ComponentProps<typeof InputGroup.Input>, 'value' | 'files' | 'type'> & {
		value?: string;
		generateLength?: number;
	};

	let { value = $bindable(''), generateLength, ...rest }: Props = $props();

	let show = $state(false);
	const type = $derived(show ? 'text' : 'password');

	const shufflePass = () => {
		value = generatePassword(generateLength);
		show = true; // tampilkan supaya user bisa lihat & menyalin
	};
</script>

<InputGroup.Root>
	<InputGroup.Addon>
		<Lock />
	</InputGroup.Addon>
	<InputGroup.Input {...rest} {type} bind:value />
	<InputGroup.Addon align="inline-end">
		<InputGroup.Button
			type="button"
			size="icon-xs"
			disabled={rest.disabled}
			aria-label={show ? 'Sembunyikan password' : 'Tampilkan password'}
			aria-pressed={show}
			onclick={() => (show = !show)}
		>
			{#if show}
				<EyeOff />
			{:else}
				<Eye />
			{/if}
		</InputGroup.Button>
		<InputGroup.Button
			type="button"
			size="icon-xs"
			disabled={rest.disabled}
			aria-label="Buat password acak"
			onclick={shufflePass}
		>
			<Shuffle />
		</InputGroup.Button>
	</InputGroup.Addon>
</InputGroup.Root>
