<script>
	import { onMount } from 'svelte';

	let users = [];

	onMount(loadData);

	async function loadData() {
		let res = await fetch(`http://localhost:8000`);
		users = await res.json();
	}

	async function deleteUser(id) {
		console.log(id);
		await fetch(`http://localhost:8000/${id}`, { method: 'DELETE' });
		await loadData();
	}
</script>

<svelte:head>
	<title>Users</title>
	<meta name="description" content="Users List" />
</svelte:head>

<div class="text-column">
	<h1>Users List</h1>

	<table>
		<tr>
			<th>Name</th>
			<th>Email</th>
			<th>Active</th>
			<th>Sign In Count</th>
			<th>Actions</th>
		</tr>
		{#each users as user}
		<tr>
			<td><a href="/users/edit/{user.id}">{user.name}</a></td>
			<td>{user.email}</td>
			<td>{user.active}</td>
			<td>{user.sign_in_count}</td>
			<td><a href="#" on:click="{() => deleteUser(user.id)}">Delete</a></td>
		</tr>
		{/each}
	</table>
</div>
