<script lang="ts">
	import { browser } from '$app/environment';
	import UserListComponent from '../../../../routes/examples/data-fetching/_UserListComponent.svelte';

	type Props = {
		url: string;
		displayType: 'users';
	};

	let { url, displayType }: Props = $props();

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
			} catch (e: unknown) {
				if (e instanceof Error && e.name !== 'AbortError' && currentUrl === url) {
					error = e;
					data = null;
				} else if (currentUrl === url) {
					error = new Error('An unknown fetch error occurred');
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
		<!-- Conditionally render the imported component based on displayType -->
		{#if displayType === 'users'}
			<!-- eslint-disable-next-line @typescript-eslint/no-explicit-any -- Deliberate 'any' to show type weakness -->
			<UserListComponent data={data as any} />
		{:else}
			<p>Error: Unknown displayType: {displayType}</p>
		{/if}
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
