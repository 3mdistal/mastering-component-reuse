<script lang="ts">
	import type { Snippet } from 'svelte';

	type Props = {
		headerText?: string;
		footerTitle?: string;
		footerBody?: string;
		footerActionText?: string;
		children: Snippet;
	};

	let { headerText, footerTitle, footerBody, footerActionText, children }: Props = $props();
</script>

<div class="card-basic">
	{#if headerText}
		<header class="card-header">
			<h3 class="card-header-title">{headerText}</h3>
		</header>
	{/if}

	<div class="card-body">
		{@render children()}
	</div>

	{#if footerTitle || footerBody || footerActionText}
		<footer class="card-footer">
			{#if footerTitle}
				<h4 class="card-footer-title">{footerTitle}</h4>
			{/if}
			{#if footerBody}
				<p class="card-footer-body">{footerBody}</p>
			{/if}
			{#if footerActionText}
				<div class="card-footer-action">
					<button class="button button-primary">{footerActionText}</button>
				</div>
			{/if}
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
		display: flex;
		flex-direction: column;
		gap: 6px;
	}
	.card-footer-title {
		font-weight: 600;
	}
	.card-footer-body {
		font-size: 0.9rem;
		color: #4b5563;
	}
	.card-footer-action {
		margin-top: 4px;
		align-self: flex-end;
	}
	.button {
		padding: 0.25rem 0.75rem;
		border-radius: 0.25rem;
		font-size: 0.875rem;
		cursor: pointer;
		transition: background-color 0.2s ease;
	}
	.button-primary {
		background-color: #3b82f6;
		color: white;
		border: 1px solid transparent;
	}
	.button-primary:hover {
		background-color: #2563eb;
	}
</style>
