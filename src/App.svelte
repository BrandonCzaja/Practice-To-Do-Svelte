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

	///////////
	// CREATE
	//////////
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

	//////////
	// EDIT
	/////////
	// Properties for editing form
	let editTitle;
	let editBody;
	let editId;

	// Edit function for form submission
	const updateTodo = async (event) => {
		event.preventDefault();
		await fetch(baseUrl + "/" + editId, {
			method: "put",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify({
				title: editTitle,
				body: editBody,
			}),
		});

		// Refresh todos list
		getTodos();

		// Reset the form
		editTitle = "";
		editBody = "";
		editId = "";
	};

	// Function to edit selected todo
	const editSelect = (todo) => {
		editTitle = todo.title;
		editBody = todo.body;
		editId = todo.id;
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

	<h2>Edit a To Do</h2>
	<form on:submit={updateTodo}>
		<input type="text" bind:value={editTitle} />
		<input type="text" bind:value={editBody} />
		<input type="submit" value="Edit To Do" />
	</form>
	<hr />
	<h2>The To Dos</h2>
	{#each todos as todo}
		<div>
			<h2>{todo.title}</h2>
			<h3>{todo.body}</h3>
			<button on:click={(e) => editSelect(todo)}>Edit</button>
		</div>
	{/each}
</main>
