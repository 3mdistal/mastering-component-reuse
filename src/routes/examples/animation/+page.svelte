<script lang="ts">
	import AnimateOnScroll from '$lib/components/patterns/flexible-composition/AnimateOnScroll.svelte';
	// Import a card component to show wrapping components
	import CardFlexible from '$lib/components/patterns/flexible-composition/CardFlexible.svelte';
</script>

<div class="page-container">
	<h1 class="page-title">Animation Wrapper Example</h1>

	<p class="explanation">
		The <code>AnimateOnScroll</code> component uses the flexible composition pattern (accepting
		<code>children</code> snippets) to apply a fade-in-up animation effect to *any* content placed inside
		it when that content scrolls into the viewport. It encapsulates the Intersection Observer logic and
		CSS transitions. The alternative would be duplicating this logic and styling in every place the animation
		is needed, violating the DRY principle.
	</p>

	<div class="demo-area">
		<p>Scroll down to see elements animate in...</p>

		<div class="spacer"></div>

		<AnimateOnScroll>
			<h2>Section 1</h2>
			<p>
				This paragraph is wrapped in the AnimateOnScroll component. Lorem ipsum dolor sit amet,
				consectetur adipiscing elit. Vivamus lacinia odio vitae vestibulum vestibulum.
			</p>
		</AnimateOnScroll>

		<div class="spacer"></div>

		<AnimateOnScroll>
			<img src="https://unsplash.it/500/500" alt="" width="500" height="500" />
		</AnimateOnScroll>

		<div class="spacer"></div>

		<AnimateOnScroll>
			<CardFlexible>
				{#snippet header()}
					<h3>Animated Card</h3>
				{/snippet}
				<p>Even components can be wrapped easily to gain the animation behavior.</p>
			</CardFlexible>
		</AnimateOnScroll>

		<div class="spacer"></div>

		<AnimateOnScroll>
			<button class="button">An Animated Button</button>
		</AnimateOnScroll>

		<div class="spacer"></div>
	</div>
</div>

<style>
	.page-container {
		padding: 2rem;
		max-width: 800px;
		margin: 0 auto;
	}
	.page-title {
		font-size: 1.875rem;
		font-weight: 700;
		margin-bottom: 2rem;
		text-align: center;
	}
	.explanation {
		font-size: 0.95rem;
		color: #333;
		margin-bottom: 2rem;
		padding-bottom: 1rem;
		border-bottom: 1px solid #eee;
		line-height: 1.6;
	}
	.explanation code {
		font-family: monospace;
		background-color: #eee;
		padding: 0.1em 0.3em;
		border-radius: 3px;
	}
	.demo-area {
		margin-top: 2rem;
	}
	.spacer {
		height: 70vh; /* Force scrolling */
		border-bottom: 1px dashed lightgray;
		margin: 2rem 0;
	}
	img {
		max-width: 100%;
		height: auto;
		display: block;
		margin: 1rem 0;
	}
	/* Basic button style for demo */
	.button {
		padding: 0.75rem 1.5rem;
		font-size: 1rem;
		background-color: #3b82f6;
		color: white;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}
</style>
