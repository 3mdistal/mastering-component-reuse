<script lang="ts">
	import CardBasic from '$lib/components/patterns/basic-composition/CardBasic.svelte';
	import CardFlexible from '$lib/components/patterns/flexible-composition/CardFlexible.svelte';

	// Data for examples
	const basicCardData = {
		header: 'Basic Card Header',
		body: 'This is the main content for the basic card. It uses the default slot.',
		footerTitle: 'Action Required',
		footerBody: 'Please review the details before proceeding.',
		footerAction: 'Confirm Setup'
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
				<CardBasic
					headerText={basicCardData.header}
					footerTitle={basicCardData.footerTitle}
					footerBody={basicCardData.footerBody}
					footerActionText={basicCardData.footerAction}
				>
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
				<h3 class="example-title">Challenge: Customizing the Footer</h3>
				<CardBasic headerText="Needs Different Footer">
					<p>
						To put different content or structure in the footer (e.g., two buttons, an icon), we
						would need to add more specific props to <code>CardBasic.svelte</code> (like
						<code>footerButton2Text</code>, <code>footerIcon</code>, etc.), leading to prop
						explosion.
					</p>
					<!-- Cannot easily customize footer structure here beyond the predefined props -->
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
	}
	.page-title {
		font-size: 1.875rem;
		font-weight: 700;
		margin-bottom: 2rem;
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
	.grid-container {
		display: grid;
		grid-template-columns: repeat(1, minmax(0, 1fr));
		gap: 1.5rem;
	}
	@media (min-width: 768px) {
		.grid-container {
			grid-template-columns: repeat(2, minmax(0, 1fr));
		}
	}
	.example-title {
		font-size: 1.25rem;
		margin-bottom: 0.5rem;
	}
	code {
		font-family: monospace;
		background-color: #f3f4f6;
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
		background-color: #10b981;
	}
	.button-success:hover {
		background-color: #059669;
	}
	.button-danger {
		background-color: #ef4444;
	}
	.button-danger:hover {
		background-color: #dc2626;
	}
</style>
