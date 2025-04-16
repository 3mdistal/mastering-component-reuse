<script lang="ts">
	import type { Snippet } from 'svelte';

	type Props = {
		children: Snippet;
		header?: Snippet;
		footer?: Snippet;
	};

	let { children, header, footer }: Props = $props();
</script>

<div class="card-flexible">
	{#if header}
		<header class="card-header">
			{@render header()}
		</header>
	{/if}

	<div class="card-body">
		{@render children()}
	</div>

	{#if footer}
		<footer class="card-footer">
			{@render footer()}
		</footer>
	{/if}
</div>

<style>
	.card-flexible {
		display: flex;
		flex-direction: column;
		gap: 10px;
		background-color: white;
		border-radius: 8px;
		padding: 12px;
		box-shadow:
			0 1px 3px 0 rgb(0 0 0 / 0.1),
			0 1px 2px -1px rgb(0 0 0 / 0.1);
	}
	.card-header {
		border-bottom: 1px solid #e5e7eb;
		padding-bottom: 8px;
		margin-bottom: 8px;
	}
	.card-body {
		flex-grow: 1;
	}
	.card-footer {
		border-top: 1px solid #e5e7eb;
		padding-top: 8px;
		margin-top: 8px;
	}
	/* Add any component-specific styles here if needed */
</style>
