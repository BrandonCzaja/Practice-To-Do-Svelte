<script>
	import { onMount } from "svelte";

	// Variable to hold todos
	let todos = [];

	// Base Url
	const baseUrl = "http://localhost:3000/todos";

	// Get todos from backend
	const getTodos = async () => {
		const response = await fetch(baseUrl);
		const data = await response.json();
		todos = await data;
	};

	// Runs when the component loads like React.useEffect
	onMount(() => {
		getTodos();
	});

	// Properties of Create Form
	let createTitle;
	let createBody;

	// Function for Create Form submission
	const createTodo = async (event) => {
		event.preventDefault();
		await fetch(baseUrl, {
			method: "post",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify({
				title: createTitle,
				body: createBody,
			}),
		});

		// Refresh to list of todos
		getTodos();

		// Reset the form
		createTitle = "";
		createBody = "";
	};
</script>

<style>
</style>

<main>
	<h1>The Svelte To Do App</h1>
	<hr />
	<h2>Create a To Do</h2>
	<form on:submit={createTodo}>
		<input type="text" bind:value={createTitle} />
		<input type="text" bind:value={createBody} />
		<input type="submit" value="Create a To Do" />
	</form>
	<hr />
	<h2>The To Dos</h2>
	{#each todos as todo}
		<div>
			<h2>{todo.title}</h2>
			<h3>{todo.body}</h3>
		</div>
	{/each}
</main>
