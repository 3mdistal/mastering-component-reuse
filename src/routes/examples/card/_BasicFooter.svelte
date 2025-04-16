<!-- This component exists solely to be passed as a prop 
 to CardBasic to demonstrate that pattern's limitation. -->

<div class="basic-footer">
	<button class="button button-secondary">Cancel</button>
	<button class="button button-primary">Save</button>
</div>

<style>
	.basic-footer {
		display: flex;
		justify-content: flex-end;
		gap: 0.5rem; /* space-x-2 */
	}

	/* Reusing button styles from page for consistency, */
	/* ideally these would be global or themed */
	.button {
		padding: 0.25rem 0.75rem; /* px-3 py-1 */
		border-radius: 0.25rem; /* rounded */
		font-size: 0.875rem; /* text-sm */
		cursor: pointer;
		transition: background-color 0.2s ease;
	}
	.button-secondary {
		border: 1px solid #d1d5db;
		background-color: white;
		color: #374151;
	}
	.button-secondary:hover {
		background-color: #f3f4f6; /* hover:bg-gray-100 */
	}
	.button-primary {
		background-color: #3b82f6; /* bg-blue-500 */
		color: white;
		border: 1px solid transparent;
	}
	.button-primary:hover {
		background-color: #2563eb; /* hover:bg-blue-600 */
	}
</style>
