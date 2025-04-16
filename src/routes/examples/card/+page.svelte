<script lang="ts">
	import CardBasic from '$lib/components/patterns/basic-composition/CardBasic.svelte';
	import CardFlexible from '$lib/components/patterns/flexible-composition/CardFlexible.svelte';

	// Demo component for the basic card's footer prop
	import BasicFooter from './_BasicFooter.svelte';

	// Data for examples
	const basicCardData = {
		header: 'Basic Card Header',
		body: 'This is the main content for the basic card. It uses the default slot.',
		footerComponent: BasicFooter
	};

	const flexibleCardData = {
		header: 'Flexible Card Header (Snippet)',
		body: 'This is the main content provided via the default children snippet.',
		footerActionLabel: 'Confirm Action'
	};
</script>

<div class="page-container">
	<h1 class="page-title">Card Component Examples</h1>

	<!-- Basic Composition Example -->
	<section class="example-section">
		<h2 class="section-title">Basic Composition (Props-based)</h2>
		<div class="grid-container">
			<div>
				<h3 class="example-title">Full Example</h3>
				<CardBasic headerText={basicCardData.header} footerComponent={BasicFooter}>
					<p>{basicCardData.body}</p>
				</CardBasic>
			</div>

			<div>
				<h3 class="example-title">No Header/Footer</h3>
				<CardBasic>
					<p>Just some simple content.</p>
				</CardBasic>
			</div>

			<div>
				<h3 class="example-title">With Header Only</h3>
				<CardBasic headerText="Header Only">
					<p>This card demonstrates using only the headerText prop.</p>
				</CardBasic>
			</div>

			<div>
				<h3 class="example-title">Challenge: Adding Custom Footer Markup</h3>
				<CardBasic headerText="Needs Custom Footer">
					<p>
						To put complex markup or logic in the footer here, we had to pass a whole <code
							>{`<svelte:component>`}</code
						>
						(now deprecated, replaced by direct component rendering in CardBasic). See
						<code>_BasicFooter.svelte</code>.
					</p>
					<!-- Cannot easily add inline footer markup here -->
				</CardBasic>
			</div>
		</div>
	</section>

	<!-- Flexible Composition Example -->
	<section class="example-section">
		<h2 class="section-title">Flexible Composition (Snippet-based)</h2>
		<div class="grid-container">
			<div>
				<h3 class="example-title">Full Example</h3>
				<CardFlexible>
					{#snippet header()}
						<h3 style="color: blue; font-weight: bold;">{flexibleCardData.header}</h3>
					{/snippet}

					<p>{flexibleCardData.body}</p>

					{#snippet footer()}
						<div style="display: flex; justify-content: flex-end;">
							<button class="button button-success">
								{flexibleCardData.footerActionLabel}
							</button>
						</div>
					{/snippet}
				</CardFlexible>
			</div>

			<div>
				<h3 class="example-title">No Header/Footer</h3>
				<CardFlexible>
					<p>Just some simple content. No header/footer snippets provided.</p>
				</CardFlexible>
			</div>

			<div>
				<h3 class="example-title">Header Only (Different Style)</h3>
				<CardFlexible>
					{#snippet header()}
						<h4 style="font-style: italic; color: grey;">Optional Subtitle Header</h4>
					{/snippet}
					<p>This card has only a header snippet, styled differently.</p>
				</CardFlexible>
			</div>

			<div>
				<h3 class="example-title">Inline Footer Markup</h3>
				<CardFlexible>
					<p>Putting complex markup or logic in the footer is easy inline using snippets.</p>
					{#snippet footer()}
						<!-- This could be its own component. -->
						<div style="display: flex; justify-content: space-between; align-items: center;">
							<span style="font-size: 0.875rem; color: grey;">Status: Pending</span>
							<button class="button button-danger"> Cancel </button>
						</div>
					{/snippet}
				</CardFlexible>
			</div>
		</div>
	</section>
</div>

<style>
	.page-container {
		padding: 2rem;
		/* space-y-12 replaced by margin on sections */
	}
	.page-title {
		font-size: 1.875rem; /* text-3xl */
		font-weight: 700; /* font-bold */
		margin-bottom: 2rem; /* mb-8 */
	}
	.example-section {
		margin-top: 3rem; /* space-y-12 */
	}
	.example-section:first-child {
		margin-top: 0;
	}
	.section-title {
		font-size: 1.5rem; /* text-2xl */
		font-weight: 600; /* font-semibold */
		margin-bottom: 1rem; /* mb-4 */
		border-bottom: 1px solid #e5e7eb; /* border-b */
		padding-bottom: 0.5rem; /* pb-2 */
	}
	.grid-container {
		display: grid;
		grid-template-columns: repeat(1, minmax(0, 1fr)); /* grid-cols-1 */
		gap: 1.5rem; /* gap-6 */
	}
	@media (min-width: 768px) {
		/* md: */
		.grid-container {
			grid-template-columns: repeat(2, minmax(0, 1fr)); /* md:grid-cols-2 */
		}
	}
	.example-title {
		font-size: 1.25rem; /* text-xl */
		margin-bottom: 0.5rem; /* mb-2 */
	}
	code {
		font-family: monospace;
		background-color: #f3f4f6; /* bg-gray-100 or 200 approx */
		padding: 0.1rem 0.25rem;
		border-radius: 0.25rem;
		font-size: 0.9em;
	}

	/* Basic reusable button styles */
	.button {
		padding: 0.5rem 1rem;
		border: none;
		border-radius: 0.25rem;
		color: white;
		cursor: pointer;
		font-size: 0.875rem;
		transition: background-color 0.2s ease;
	}
	.button-success {
		background-color: #10b981; /* bg-green-500 */
	}
	.button-success:hover {
		background-color: #059669; /* hover:bg-green-600 */
	}
	.button-danger {
		background-color: #ef4444; /* bg-red-500 */
	}
	.button-danger:hover {
		background-color: #dc2626; /* hover:bg-red-600 */
	}
</style>
