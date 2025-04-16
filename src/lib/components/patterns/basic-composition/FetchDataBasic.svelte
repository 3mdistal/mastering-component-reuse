<script lang="ts">
	import type { Component } from 'svelte';
	import { browser } from '$app/environment';

	type Props = {
		url: string;
		displayComponent: Component<{ data?: unknown }>; // Expects component that ACCEPTS a data prop (make optional for initial render)
	};

	let { url, displayComponent }: Props = $props();

	let loading = $state(true);
	let error = $state<Error | null>(null);
	let data = $state<unknown>(null);

	$effect(() => {
		if (!browser) return; // Don't run on server

		loading = true;
		error = null;
		data = null;
		let currentUrl = url;

		const controller = new AbortController();

		async function fetchData() {
			try {
				const response = await fetch(currentUrl, { signal: controller.signal });
				if (!response.ok) {
					throw new Error(`HTTP error! status: ${response.status}`);
				}
				const fetchedData = await response.json();
				if (currentUrl === url) {
					// Check if URL changed during fetch to prevent race condition
					data = fetchedData;
					error = null;
				}
			} catch (e: any) {
				if (e.name !== 'AbortError' && currentUrl === url) {
					error = e instanceof Error ? e : new Error('Fetch failed');
					data = null;
				}
			} finally {
				if (currentUrl === url) {
					loading = false;
				}
			}
		}

		fetchData();

		return () => {
			controller.abort(); // Cleanup effect
		};
	});
</script>

<div>
	{#if loading}
		<p class="loading-state">Loading data...</p>
		<!-- Hardcoded loading UI -->
	{:else if error}
		<p class="error-state">Error fetching data: {error.message}</p>
		<!-- Hardcoded error UI -->
	{:else if data}
		<!-- Render the required component, passing data via prop -->
		<svelte:component this={displayComponent} {data} />
	{:else}
		<p>No data.</p>
	{/if}
</div>

<style>
	.loading-state {
		color: #6b7280; /* gray-500 */
		font-style: italic;
	}
	.error-state {
		color: #ef4444; /* red-500 */
		font-weight: bold;
	}
</style>
