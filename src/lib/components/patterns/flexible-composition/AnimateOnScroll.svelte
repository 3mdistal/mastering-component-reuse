<script lang="ts">
	import type { Snippet } from 'svelte';
	import { browser } from '$app/environment';

	type Props = {
		children: Snippet;
		// Optional props for customization, could add later (e.g., threshold, rootMargin, animationClass)
	};

	let { children }: Props = $props();

	let element: HTMLElement | undefined = $state(); // Element to observe
	let isVisible = $state(false); // State to trigger animation

	$effect(() => {
		if (!browser || !element) return;

		const observer = new IntersectionObserver(
			(entries) => {
				// Use isIntersecting to determine if element is in viewport
				const entry = entries[0];
				if (entry.isIntersecting) {
					isVisible = true;
					// Optional: Unobserve after first intersection if animation should only run once
					// observer.unobserve(element);
				}
			},
			{
				threshold: 0.1 // Trigger when 10% of the element is visible
				// Add rootMargin here if needed for offset triggering
			}
		);

		observer.observe(element);

		// Cleanup function
		return () => {
			if (element) {
				// Ensure element exists before trying to unobserve
				try {
					observer.unobserve(element);
				} catch (e) {
					// Ignore errors if element was already removed from DOM somehow
				}
			}
			observer.disconnect(); // Disconnect observer fully
		};
	});
</script>

<!-- Bind the root element to our state variable -->
<!-- Apply visibility class based on state -->
<div bind:this={element} class="animate-on-scroll {isVisible ? 'is-visible' : ''}">
	{@render children()}
</div>

<style>
	.animate-on-scroll {
		opacity: 0;
		transform: translateY(20px); /* Start slightly lower */
		transition:
			opacity 0.6s ease-out,
			transform 0.6s ease-out;
		will-change: opacity, transform; /* Optimize animation performance */
	}

	.animate-on-scroll.is-visible {
		opacity: 1;
		transform: translateY(0);
	}
</style>
