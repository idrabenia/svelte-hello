<script>
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';

	let user = {};
	export let data;

	onMount(loadData);

	async function loadData() {
		if (data.id) {
			let res = await fetch(`http://localhost:8000/${data.id}`);
			user = await res.json();
		} else {
			user = {};
		}
	}

	async function submit() {
		if (data.id) {
			await fetch(`http://localhost:8000/${data.id}`, {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
					'Accept': 'application/json'
				},
				body: JSON.stringify(user)
			});
		} else {
			await fetch(`http://localhost:8000/`, {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
					'Accept': 'application/json'
				},
				body: JSON.stringify(user)
			});
		}
		goto('/users');
	}


</script>

<svelte:head>
	<title>Edit User</title>
	<meta name="description" content="Edit User" />
</svelte:head>

<div class="text-column">
	<h1>Users List</h1>

	<form action="#" on:submit|preventDefault="{submit}">
		<div>Name</div>
		<input type="text" bind:value="{user.name}" /> <br />

		<div>Email</div>
		<input type="text" bind:value="{user.email}" /> <br />

		<div>Active</div>
		<input type="checkbox" bind:checked="{user.active}" /> <br />

		<div>Sign In Count</div>
		<input type="number" bind:value="{user.sign_in_count}" /> <br />

		<button type="submit">Submit</button>
	</form>
</div>
