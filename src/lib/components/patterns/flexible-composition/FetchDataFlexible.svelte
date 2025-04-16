<script lang="ts" generics="T = unknown">
	import type { Snippet } from 'svelte';
	import { browser } from '$app/environment';

	type Props = {
		url: string;
		loading: Snippet;
		error: Snippet<[error: Error]>;
		data: Snippet<[data: T]>;
	};

	let { url, loading: loadingSnippet, error: errorSnippet, data: dataSnippet }: Props = $props();

	let loading = $state(true);
	let error = $state<Error | null>(null);
	let data = $state<T | null>(null);

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
				// Assume the consumer knows the expected type T
				const fetchedData = (await response.json()) as T;
				if (currentUrl === url) {
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
			controller.abort();
		};
	});
</script>

<div>
	{#if loading}
		{@render loadingSnippet()}
	{:else if error}
		{@render errorSnippet(error)}
	{:else if data !== null}
		{@render dataSnippet(data)}
	{:else}
		<!-- Optional: Render nothing or a default empty state if data is null/undefined after loading -->
	{/if}
</div>
