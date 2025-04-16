<script lang="ts">
	// This component is specifically designed to work with FetchDataBasic
	// It expects a `data` prop which should be an array of users.
	type User = { id: number; name: string; email: string };

	type Props = {
		data: User[];
	};
	let { data }: Props = $props();
</script>

{#if data && data.length > 0}
	<ul class="user-list">
		{#each data as user (user.id)}
			<li>
				<strong>{user.name}</strong> ({user.email})
			</li>
		{/each}
	</ul>
{:else}
	<p>No users found.</p>
{/if}

<style>
	.user-list {
		list-style: none;
		padding: 0;
	}
	.user-list li {
		border-bottom: 1px solid #eee;
		padding: 8px 0;
	}
	.user-list li:last-child {
		border-bottom: none;
	}
</style>
