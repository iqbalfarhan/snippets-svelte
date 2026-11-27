<script lang="ts">
	let { data }: { data: unknown } = $props();
</script>

<pre>{JSON.stringify(data, null, 2)}</pre>
