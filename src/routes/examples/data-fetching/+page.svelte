<script lang="ts">
	import FetchDataBasic from '$lib/components/patterns/basic-composition/FetchDataBasic.svelte';
	import FetchDataFlexible from '$lib/components/patterns/flexible-composition/FetchDataFlexible.svelte';
	import UserListComponent from './_UserListComponent.svelte'; // Component for the basic example
	import type { Component } from 'svelte'; // Import Component for assertion

	type User = { id: number; name: string; email: string };
	type Post = { userId: number; id: number; title: string; body: string };

	const usersUrl = 'https://jsonplaceholder.typicode.com/users?_limit=5';
	const postsUrl = 'https://jsonplaceholder.typicode.com/posts?_limit=3';
	const errorUrl = 'https://jsonplaceholder.typicode.com/bad-url'; // To test error state

	let showPosts = $state(false);
	let useErrorUrl = $state(false);

	const flexibleUrl = $derived(useErrorUrl ? errorUrl : showPosts ? postsUrl : usersUrl);
</script>

<div class="page-container">
	<h1 class="page-title">Data Fetching Component Examples</h1>

	<div class="controls">
		<button
			onclick={() => {
				showPosts = !showPosts;
			}}>Toggle Users/Posts (Flexible Example)</button
		>
		<label>
			<input type="checkbox" bind:checked={useErrorUrl} />
			Use Bad URL (Flexible Example)
		</label>
	</div>

	<!-- Basic Composition Example -->
	<section class="example-section">
		<h2 class="section-title">Basic Composition (Prop-based DisplayComponent)</h2>
		<div class="component-demo">
			<p class="explanation">
				This component fetches data from <code>{usersUrl}</code>. Loading and error states are
				hardcoded inside <code>FetchDataBasic</code>. It requires a specific
				<code>displayComponent</code> prop (<code>UserListComponent</code> in this case) that knows how
				to handle the fetched data via a `data` prop.
			</p>
			<FetchDataBasic
				url={usersUrl}
				displayComponent={UserListComponent as Component<{ data?: unknown }>}
			/>
		</div>
	</section>

	<!-- Flexible Composition Example -->
	<section class="example-section">
		<h2 class="section-title">Flexible Composition (Snippet-based)</h2>
		<div class="component-demo">
			<p class="explanation">
				This component fetches from <code>{flexibleUrl}</code>. The UI for loading, error, and
				success states is provided by the consumer using snippets. This makes the component highly
				reusable for different data types and UI needs.
			</p>
			{#if showPosts}
				<svelte:component this={FetchDataFlexible<Post[]>} url={flexibleUrl}>
					{#snippet loading()}
						<p class="loading-state-flexible"><i>⏳ Loading posts... please wait...</i></p>
					{/snippet}
					{#snippet error(error)}
						<div class="error-state-flexible">
							<strong>🚨 Post Fetch Error!</strong>
							<pre>{error.message}</pre>
						</div>
					{/snippet}
					{#snippet data(data)}
						<div class="post-list">
							<h3>Fetched Posts:</h3>
							{#each data as post (post.id)}
								<article>
									<h4>{post.title}</h4>
									<p>{post.body}</p>
								</article>
							{/each}
						</div>
					{/snippet}
				</svelte:component>
			{:else}
				<svelte:component this={FetchDataFlexible<User[]>} url={flexibleUrl}>
					{#snippet loading()}
						<p class="loading-state-flexible"><i>⏳ Loading users... please wait...</i></p>
					{/snippet}
					{#snippet error(error)}
						<div class="error-state-flexible">
							<strong>🚨 User Fetch Error!</strong>
							<pre>{error.message}</pre>
						</div>
					{/snippet}
					{#snippet data(data)}
						<div class="user-list-flexible">
							<h3>Fetched Users:</h3>
							<ul>
								{#each data as user (user.id)}
									<li>{user.name}</li>
								{/each}
							</ul>
						</div>
					{/snippet}
				</svelte:component>
			{/if}
		</div>
	</section>
</div>

<style>
	.page-container {
		padding: 2rem;
	}
	.page-title {
		font-size: 1.875rem;
		font-weight: 700;
		margin-bottom: 2rem;
	}
	.controls {
		margin-bottom: 2rem;
		display: flex;
		gap: 1rem;
		align-items: center;
	}
	.controls button,
	.controls label {
		padding: 0.5rem 1rem;
		border: 1px solid #ccc;
		border-radius: 4px;
		background-color: #f0f0f0;
		cursor: pointer;
	}
	.controls button:hover {
		background-color: #e0e0e0;
	}
	.controls input[type='checkbox'] {
		margin-right: 0.5rem;
	}
	.example-section {
		margin-top: 3rem;
	}
	.example-section:first-child {
		margin-top: 0;
	}
	.section-title {
		font-size: 1.5rem;
		font-weight: 600;
		margin-bottom: 1rem;
		border-bottom: 1px solid #e5e7eb;
		padding-bottom: 0.5rem;
	}
	.component-demo {
		border: 1px dashed #ccc;
		padding: 1rem;
		background-color: #f9f9f9;
	}
	.explanation {
		font-size: 0.9rem;
		color: #333;
		margin-bottom: 1rem;
		padding-bottom: 1rem;
		border-bottom: 1px solid #eee;
	}
	.explanation code {
		font-family: monospace;
		background-color: #eee;
		padding: 0.1em 0.3em;
		border-radius: 3px;
	}

	/* Styles specific to flexible example snippets */
	.loading-state-flexible {
		color: blue;
		padding: 1rem;
		background-color: #e0f2fe;
		border-left: 4px solid blue;
	}
	.error-state-flexible {
		color: #991b1b; /* red-800 */
		padding: 1rem;
		background-color: #fee2e2; /* red-100 */
		border: 1px solid #fca5a5; /* red-300 */
		border-radius: 4px;
	}
	.error-state-flexible pre {
		margin-top: 0.5rem;
		font-family: monospace;
		white-space: pre-wrap;
		font-size: 0.8rem;
	}
	.post-list article {
		margin-bottom: 1rem;
		padding-bottom: 1rem;
		border-bottom: 1px dotted #ccc;
	}
	.post-list article:last-child {
		border-bottom: none;
	}
	.post-list h4 {
		margin: 0 0 0.5rem 0;
	}
	.user-list-flexible ul {
		list-style: square;
		padding-left: 20px;
	}
</style>
