<script lang="ts">
	import type { Snippet } from 'svelte';

	type Props = {
		children: Snippet;
		header?: Snippet;
		footer?: Snippet;
		padding?: 'small' | 'medium' | 'large';
		showBorder?: boolean;
	};

	let { children, header, footer, padding = 'medium', showBorder = true }: Props = $props();

	const paddingClass = $derived(
		{
			small: '2px',
			medium: '4px',
			large: '6px'
		}[padding]
	);

	const borderClass = $derived(showBorder ? 'card-border' : '');
</script>

<div class={`card-flexible ${borderClass}`} style={`padding: ${paddingClass};`}>
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
		box-shadow:
			0 1px 3px 0 rgb(0 0 0 / 0.1),
			0 1px 2px -1px rgb(0 0 0 / 0.1);
	}
	.card-border {
		border: 1px solid #d1d5db;
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
