<script lang="ts">
	import type { Snippet, Component } from 'svelte';

	type Props = {
		headerText?: string;
		footerComponent?: Component;
		children: Snippet;
	};

	let { headerText, footerComponent, children }: Props = $props();
</script>

<div class={`card-basic`}>
	{#if headerText}
		<header class="card-header">
			<h3 class="card-header-title">{headerText}</h3>
		</header>
	{/if}

	<div class="card-body">
		{@render children()}
	</div>

	{#if footerComponent}
		<footer class="card-footer">
			<!-- Using svelte:component is necessary here to render the passed Component prop.
			 Note: Svelte 5 issues a deprecation warning, encouraging snippet-based patterns 
			 (like in CardFlexible) for better flexibility and type safety where possible. -->
			<svelte:component this={footerComponent} />
		</footer>
	{/if}
</div>

<style>
	.card-basic {
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
	.card-header-title {
		font-size: 1.125rem;
		font-weight: 600;
	}
	.card-body {
		flex-grow: 1;
	}
	.card-footer {
		border-top: 1px solid #e5e7eb;
		padding-top: 8px;
		margin-top: 8px;
	}
</style>
